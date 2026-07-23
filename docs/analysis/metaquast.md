# MetaQUAST Redmine Automator

## Overview

The MetaQUAST Redmine Automator runs a MetaQUAST assembly-quality analysis using FASTA assemblies stored on the OLC NAS.

The automator:

1. Reads SEQIDs from a Redmine issue.
2. Retrieves the requested FASTA assemblies from the NAS.
3. Runs MetaQUAST on the available assemblies.
4. Converts the transposed MetaQUAST report to Excel format.
5. Attaches the Excel report to the Redmine issue.
6. Compresses the complete MetaQUAST output.
7. Uploads the compressed results to Dropbox.
8. Updates and closes the Redmine issue.

---

## When to Use This Analysis

Use the MetaQUAST automator to:

- Evaluate one or more metagenomic assemblies.
- Compare metagenomic assemblies generated using different methods.
- Review assembly statistics across multiple assemblies.
- Assess metrics such as:
  - Total assembly length
  - Number of contigs
  - N50
  - L50
  - GC content
  - Largest contig
- Produce MetaQUAST reports and visualizations.
- Identify potential quality differences among metagenomic assemblies.

MetaQUAST evaluates assembled sequence data. It does not assemble raw sequencing reads.

For non-metagenomic isolate assemblies, use the standard QUAST automator instead.

---

## Redmine Issue Format

Enter one SEQID per line in the issue description.

### Basic analysis

```text
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

This runs MetaQUAST on the listed assemblies.

### Formatting requirements

- Enter one SEQID per line.
- Do not separate SEQIDs with commas.
- Blank lines are ignored.
- Duplicate SEQIDs are removed automatically.
- SEQIDs are processed in the order in which they appear.
- Do not include QUAST or MetaQUAST command-line arguments.
- Do not include a `reference=` setting.
- Every non-empty line is treated as a SEQID.

---

## Arguments

The MetaQUAST automator does not currently support user-supplied analysis arguments through the Redmine issue description.

The automator determines the number of threads from the SLURM allocation and supplies the following options automatically:

```text
-t <threads>
-o <output_directory>
```

The input assemblies are added to the command automatically after they are retrieved from the OLC NAS.

---

## Analysis Workflow

### 1. Initialize the automator

The automator:

- Disables `urllib3` warnings.
- Initializes Sentry error reporting.
- Unpickles the Redmine API instance.
- Unpickles the Redmine issue.
- Unpickles the parsed issue description.

The Redmine automation framework supplies the pickle paths using:

```text
--redmine_instance
--issue
--work_dir
--description
```

These command-line options are mapped internally to variables that identify the corresponding pickle files.

### 2. Determine the thread count

The automator obtains its thread count from the SLURM environment variable:

```text
SLURM_CPUS_PER_TASK
```

For example, if the SLURM script contains:

```bash
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
```

the automator runs MetaQUAST with:

```text
-t 16
```

If `SLURM_CPUS_PER_TASK` is unavailable, the automator uses its configured default thread count.

The thread count must be a positive integer.

### 3. Parse the issue description

Every non-empty line in the issue description is interpreted as a SEQID.

For example:

```text
2026-SEQ-0001
2026-SEQ-0002

2026-SEQ-0003
2026-SEQ-0001
```

is interpreted as:

```text
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

Blank lines are ignored, and duplicate SEQIDs are removed while preserving their original order.

If no SEQIDs are supplied, the automator updates the issue with:

```text
WARNING: No SEQIDs provided!
```

The issue is then assigned status ID `4`, and MetaQUAST is not run.

### 4. Create the sequence directory

The automator creates the following directory:

```text
<work_dir>/sequences
```

The directory is created automatically if it does not already exist.

### 5. Retrieve assemblies

The automator uses `nastools.retrieve_nas_files()` to retrieve the requested FASTA assemblies from the OLC NAS:

```python
retrieve_nas_files(
    seqids=seqids,
    outdir=str(sequence_directory),
    filetype="fasta",
    copyflag=False,
)
```

Because `copyflag` is set to `False`, the retrieval utility may create links to the assemblies rather than copying the original files, depending on the implementation of `nastools`.

### 6. Check for missing FASTA files

For each requested SEQID, the automator searches for a matching FASTA file using the following pattern:

```text
<SEQID>*fasta*
```

If one or more requested assemblies cannot be found, the Redmine issue is updated with a warning resembling:

```text
WARNING: Could not find the following requested SEQIDs on the OLC NAS:
['2026-SEQ-0002', '2026-SEQ-0003']
```

The current implementation reports missing assemblies but does not stop the analysis if at least one requested assembly was found.

MetaQUAST continues using the available assemblies.

If none of the requested assemblies can be found, the automator raises an error and does not run MetaQUAST.

### 7. Identify the input assemblies

The automator collects files associated with the requested SEQIDs using:

```text
<SEQID>*fasta*
```

Only FASTA files associated with the requested SEQIDs are included in the MetaQUAST command.

This prevents unrelated FASTA files that may be present in the sequence directory from being analysed accidentally.

Duplicate file paths are removed while preserving the original SEQID order.

### 8. Construct the MetaQUAST command

The command uses the Python interpreter and `metaquast.py` installed in the QUAST virtual environment:

```text
/mnt/nas2/virtual_environments/quast/bin/python
/mnt/nas2/virtual_environments/quast/bin/metaquast.py
```

A generated command resembles:

```bash
/mnt/nas2/virtual_environments/quast/bin/python \
  /mnt/nas2/virtual_environments/quast/bin/metaquast.py \
  <work_dir>/sequences/2026-SEQ-0001.fasta \
  <work_dir>/sequences/2026-SEQ-0002.fasta \
  <work_dir>/sequences/2026-SEQ-0003.fasta \
  -t 16 \
  -o <work_dir>/metaquast_output_<issue_id>
```

All command arguments are shell-quoted before the command is written to the generated shell script.

No user-supplied reference argument is added by the automator.

The complete generated command is added to the Redmine issue before MetaQUAST is executed.

### 9. Activate the QUAST environment

The automator activates the QUAST Conda environment using:

```bash
source /home/ubuntu/miniconda3/etc/profile.d/conda.sh &&
conda activate /mnt/nas2/virtual_environments/quast
```

MetaQUAST is distributed with QUAST and is run from the same virtual environment.

### 10. Create the MetaQUAST shell script

The environment activation and MetaQUAST command are written to:

```text
<work_dir>/run_metaquast.sh
```

The generated script:

- Uses `#!/bin/bash`.
- Activates the QUAST Conda environment.
- Runs the shell-quoted MetaQUAST command.
- Is made executable automatically.

### 11. Run MetaQUAST

The generated shell script is executed using `subprocess.run()`:

```python
subprocess.run(
    [str(script_path)],
    check=True,
)
```

The `check=True` setting causes Python to raise an exception if the MetaQUAST shell script returns a nonzero exit status.

This prevents the automator from treating a failed MetaQUAST run as successful.

### 12. Validate the MetaQUAST report

The automator expects MetaQUAST to create:

```text
<work_dir>/metaquast_output_<issue_id>/transposed_report.tsv
```

If this report is not present after the shell script finishes, the automator raises an error.

This validation helps detect cases where MetaQUAST exited without producing the expected result, even if the failure was not otherwise detected.

### 13. Convert the report to Excel

The MetaQUAST transposed report is converted from TSV format to Excel format.

Input:

```text
<work_dir>/metaquast_output_<issue_id>/transposed_report.tsv
```

Output:

```text
<work_dir>/metaquast_output_<issue_id>/MetaQUAST_results_redmine<issue_id>.xlsx
```

The Excel file is then copied to:

```text
<work_dir>/MetaQUAST_results_redmine<issue_id>.xlsx
```

The copy in the issue work directory is prepared as a Redmine attachment.

Because the shared TSV-to-XLSX conversion method reports some errors without raising an exception, the automator explicitly verifies that the Excel file was created.

If the Excel file does not exist after conversion, the analysis is treated as failed.

### 14. Determine the MetaQUAST version

The automator determines the installed MetaQUAST version using:

```bash
/mnt/nas2/virtual_environments/quast/bin/python \
  /mnt/nas2/virtual_environments/quast/bin/metaquast.py \
  --version
```

The detected version is included in the final Redmine update.

If the version cannot be determined, the shared version-detection method returns an explanatory message rather than stopping the analysis.

### 15. Compress the complete results

The complete MetaQUAST output directory is compressed as:

```text
<work_dir>/metaquast_output_<issue_id>.zip
```

The automator verifies that the ZIP archive exists before attempting the Dropbox upload.

If the ZIP archive cannot be created, the analysis is treated as failed.

### 16. Upload the complete output to Dropbox

The ZIP archive is uploaded to Dropbox using the configured application credentials.

The upload uses:

- Dropbox access token
- Dropbox refresh token
- Dropbox application key
- Dropbox application secret

If the upload succeeds, the resulting download URL is added to the final Redmine update.

### 17. Complete the issue

When the analysis and Dropbox upload succeed, the automator:

- Attaches the Excel report.
- Reports the MetaQUAST version.
- Provides the Dropbox download URL.
- Assigns the issue status ID `4`.

The final Redmine message resembles:

```text
MetaQUAST analysis complete!
MetaQUAST version is <version>

The report file has been attached to this issue.
Full results are available at the following URL:
<Dropbox URL>
```

If MetaQUAST completes but the Dropbox upload fails, the Excel report is still attached and the issue is updated with a message resembling:

```text
The MetaQUAST analysis completed, but the upload of the full results
to Dropbox was unsuccessful. The Excel report has been attached to
this issue. Please try the full-results upload again later.
```

---

## Outputs

### Excel report

```text
MetaQUAST_results_redmine<issue_id>.xlsx
```

This file is generated from MetaQUAST's `transposed_report.tsv` and attached directly to the Redmine issue.

### Complete output archive

```text
metaquast_output_<issue_id>.zip
```

This archive contains the complete MetaQUAST output and is uploaded to Dropbox.

Depending on the MetaQUAST version, input assemblies, and analysis results, the archive may include:

- HTML reports
- Text reports
- TSV reports
- Transposed reports
- PDF reports
- Assembly statistics
- Per-assembly reports
- Combined reports
- Contig statistics
- Alignment-related output
- Interactive visualizations
- Reference-search-related files
- MetaQUAST intermediate files
- Log files

### Generated shell script

During the analysis, the automator creates:

```text
<work_dir>/run_metaquast.sh
```

This script contains the environment activation and complete MetaQUAST command.

It is removed after successful completion but retained if an error occurs.

### Redmine updates

The issue may receive updates containing:

- Missing SEQID warnings
- The complete MetaQUAST command
- Successful completion
- MetaQUAST version
- Dropbox download URL
- Dropbox upload failure
- Unexpected processing errors
- Python traceback information

---

## Interpreting Common MetaQUAST Metrics

### Number of contigs

The total number of contigs in an assembly.

A lower number may indicate a more contiguous assembly, but the expected number depends on:

- Community complexity
- Sequencing depth
- Sequencing technology
- Assembly method
- Organism abundance
- Repeat content

Metagenomic assemblies usually contain substantially more contigs than individual isolate assemblies.

### Largest contig

The length of the longest contig in the assembly.

A longer largest contig may indicate improved assembly continuity, but this metric should not be interpreted by itself.

### Total length

The combined length of all contigs in the assembly.

For a metagenomic assembly, total length depends on:

- Community complexity
- Sequencing depth
- Organism abundance
- Assembly parameters
- Contig-length filtering
- Contamination
- Unassembled sequence

A larger total length does not necessarily indicate a better assembly.

### N50

The contig length at which at least 50% of the total assembly length is contained in contigs of that length or longer.

A larger N50 generally indicates a more contiguous assembly.

However, N50 should not be interpreted by itself because:

- Misassembled contigs may increase N50.
- Highly abundant organisms may dominate the calculation.
- Low-abundance organisms may remain fragmented.
- Assemblies with different total lengths may not be directly comparable.

### L50

The minimum number of the largest contigs required to account for at least 50% of the total assembly length.

A lower L50 generally indicates that a smaller number of contigs accounts for a larger portion of the assembly.

### GC content

The percentage of assembled bases that are guanine or cytosine.

Metagenomic assemblies may contain organisms with substantially different GC contents. Therefore, the overall GC percentage represents a mixture of the organisms and sequences present in the assembly.

Unexpected GC distributions may indicate:

- Contamination
- Assembly bias
- Unexpected organisms
- Uneven organism abundance
- Incomplete representation of the community

### Reference-associated results

When MetaQUAST identifies or obtains appropriate references, it may generate additional results associated with those references.

Reference-associated results depend heavily on:

- The availability of suitable reference genomes
- The relatedness of the references to organisms in the sample
- Database availability
- Network availability
- The completeness of the reference-search process
- The complexity of the metagenomic community

A lack of reference-associated results does not necessarily mean that the assembly failed. It may indicate that suitable references were not available or could not be identified.

### Misassemblies

A misassembly is a contig structure that conflicts with the expected structure based on an available reference.

Potential misassemblies may include:

- Relocations
- Inversions
- Translocations
- Interspecies misassemblies

Misassembly counts should be interpreted together with reference quality, organism relatedness, and assembly continuity.

### Unaligned contigs

Unaligned contigs are contigs that could not be aligned to the references used during the analysis.

They may represent:

- Organisms without suitable references
- Novel sequence
- Contamination
- Low-quality contigs
- Incorrectly assembled sequence
- Mobile genetic elements
- Plasmid sequence
- Phage sequence

Unaligned contigs are not automatically erroneous.

---

## Troubleshooting

### No SEQIDs provided

**Redmine message:**

```text
WARNING: No SEQIDs provided!
```

**Cause:** The issue description did not contain any non-empty lines that could be interpreted as SEQIDs.

**Resolution:**

- Add at least one SEQID.
- Enter one SEQID per line.
- Do not separate SEQIDs with commas.
- Confirm that the issue description was saved correctly.

---

### One or more FASTA files could not be found

**Redmine message:**

```text
WARNING: Could not find the following requested SEQIDs on the OLC NAS:
[...]
```

**Possible causes:**

- A SEQID is incorrect.
- The assembly has not yet been generated.
- The assembly is not stored in the expected NAS location.
- The assembly filename does not match the expected `<SEQID>*fasta*` pattern.
- The NAS is unavailable.
- A link to the assembly could not be created.

**Resolution:**

1. Verify the spelling of each SEQID.
2. Confirm that each assembly exists on the OLC NAS.
3. Confirm that the assembly filename begins with the requested SEQID.
4. Confirm that the filename contains `fasta`.
5. Confirm that the NAS is mounted and accessible.
6. Retry the analysis after correcting any missing assemblies.

The current automator continues with the assemblies that were found.

---

### No requested FASTA assemblies were found

**Possible error:**

```text
None of the requested FASTA assemblies could be found on the OLC NAS.
```

**Cause:** The NAS retrieval completed without making any requested assembly available in the sequence directory.

**Resolution:**

- Verify all requested SEQIDs.
- Confirm that assemblies have been generated.
- Confirm the expected NAS location.
- Confirm the expected filename pattern.
- Confirm that the automator host can access the NAS.
- Review the retained sequence directory and traceback.

MetaQUAST is not run when no input assemblies are available.

---

### MetaQUAST command failed

**Possible symptoms:**

- The SLURM job is marked as failed.
- The Redmine issue contains a Python traceback.
- `subprocess.CalledProcessError` appears in the traceback.
- The expected MetaQUAST report is missing.

**Possible causes:**

- One or more FASTA files are invalid.
- The QUAST environment could not be activated.
- `metaquast.py` is unavailable.
- A MetaQUAST dependency is unavailable.
- The job exceeded its memory allocation.
- The job exceeded its wall-time allocation.
- The output directory is not writable.
- Automatic reference discovery failed.
- Network or external database access failed.
- MetaQUAST returned a nonzero exit status.

**Resolution:**

1. Review the MetaQUAST command posted to the Redmine issue.
2. Review the SLURM standard-output file.
3. Review the SLURM standard-error file.
4. Review the traceback posted to Redmine.
5. Confirm that the QUAST environment exists.
6. Confirm that `metaquast.py` exists in the configured environment.
7. Confirm that the FASTA files are readable.
8. Check the SLURM job state and resource usage.
9. Check whether the job exceeded its memory or time allocation.
10. Retry after correcting the underlying problem.

---

### Expected transposed report was not generated

**Possible error:**

```text
MetaQUAST completed without producing the expected report:
<work_dir>/metaquast_output_<issue_id>/transposed_report.tsv
```

**Possible causes:**

- MetaQUAST failed before report generation.
- MetaQUAST produced output in an unexpected location.
- The installed MetaQUAST version uses a different output structure.
- Automatic reference discovery did not finish.
- The job was interrupted.
- The output directory was not writable.
- Input assemblies were invalid or empty.

**Resolution:**

1. Inspect the retained MetaQUAST output directory.
2. Review MetaQUAST log files.
3. Review the SLURM output and error files.
4. Confirm the installed MetaQUAST version.
5. Confirm the expected report location for that version.
6. Confirm that the input FASTA files contain valid sequence data.

---

### Excel report was not generated

**Possible error:**

```text
The MetaQUAST TSV report was not successfully converted to an Excel
workbook.
```

**Possible causes:**

- `transposed_report.tsv` could not be read.
- The report is empty or malformed.
- The output directory is not writable.
- `pandas` encountered a parsing problem.
- The `openpyxl` package is unavailable.
- The filesystem is full.
- An existing output file could not be overwritten.

**Resolution:**

1. Confirm that `transposed_report.tsv` exists.
2. Confirm that the TSV file contains tab-separated data.
3. Confirm that the issue work directory is writable.
4. Confirm that sufficient disk space is available.
5. Confirm that `pandas` and `openpyxl` are installed.
6. Review the SLURM output for TSV-to-XLSX conversion messages.

---

### MetaQUAST version could not be determined

**Possible final message:**

```text
MetaQUAST version is Could not determine version: ...
```

**Possible causes:**

- The configured Python executable is missing.
- `metaquast.py` is missing.
- The installed version does not support `--version`.
- The version command produced no output.
- The environment is incomplete.

**Resolution:**

Run the following command manually:

```bash
/mnt/nas2/virtual_environments/quast/bin/python \
  /mnt/nas2/virtual_environments/quast/bin/metaquast.py \
  --version
```

Confirm that the command prints a version.

A version-detection failure does not necessarily mean that the analysis itself failed.

---

### ZIP archive was not generated

**Possible error:**

```text
The MetaQUAST output archive was not created:
<work_dir>/metaquast_output_<issue_id>.zip
```

**Possible causes:**

- The output directory does not exist.
- The filesystem is not writable.
- The filesystem is full.
- The output files were removed unexpectedly.
- Archive creation failed.

**Resolution:**

- Confirm that the MetaQUAST output directory exists.
- Confirm that the work directory is writable.
- Confirm that sufficient disk space is available.
- Review the traceback and SLURM error file.
- Attempt to compress the retained output directory manually.

---

### Dropbox upload failed

**Redmine message resembles:**

```text
The MetaQUAST analysis completed, but the upload of the full results
to Dropbox was unsuccessful. The Excel report has been attached to
this issue. Please try the full-results upload again later.
```

**Possible causes:**

- Network connectivity problems
- Expired or invalid Dropbox credentials
- Dropbox API availability problems
- Insufficient Dropbox storage
- A missing or unreadable ZIP archive
- Authentication failure
- Upload-size restrictions

**Resolution:**

- Confirm that the ZIP archive was created.
- Verify the configured Dropbox credentials.
- Check network connectivity from the automator host.
- Confirm that sufficient Dropbox storage is available.
- Retry the upload later.
- Review application and SLURM logs for Dropbox errors.

The Excel summary report should still be attached to the Redmine issue.

---

### SLURM job used an unexpected number of threads

**Possible causes:**

- `SLURM_CPUS_PER_TASK` was not set.
- The SLURM script used `--ntasks` instead of `--cpus-per-task`.
- The job was run manually outside SLURM.
- The configured fallback thread count was used.
- The environment variable contained an invalid value.

**Resolution:**

Confirm that the SLURM script contains:

```bash
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=<thread_count>
```

Inside the job, inspect:

```bash
echo "$SLURM_CPUS_PER_TASK"
```

The MetaQUAST command posted to Redmine should contain the same value:

```text
-t <thread_count>
```

---

### Invalid SLURM thread allocation

**Possible error:**

```text
SLURM_CPUS_PER_TASK must be an integer
```

or:

```text
SLURM_CPUS_PER_TASK must be at least one
```

**Cause:** The SLURM environment variable contained an invalid value.

**Resolution:**

- Confirm that `--cpus-per-task` is a positive integer.
- Review the generated SLURM script.
- Do not manually assign a nonnumeric value to `SLURM_CPUS_PER_TASK`.
- Retry the job with a valid CPU allocation.

---

### Unexpected automator error

**Redmine message begins with:**

```text
Something went wrong! Please contact a bioinformatician to investigate:
```

The remainder of the update contains the Python traceback.

The exception is also sent to Sentry.

Because the exception is re-raised after the Redmine update, SLURM should record the job as failed rather than successfully completed.

**Resolution:**

Provide the following information to a bioinformatician:

- Redmine issue number
- SLURM job ID
- Full Python traceback
- MetaQUAST command posted to the issue
- Requested SEQIDs
- SLURM standard-output file
- SLURM standard-error file
- MetaQUAST output directory
- CPU and memory allocation
- MetaQUAST version, if available

---

## SLURM Resources

The automator is intended to run as one multithreaded SLURM task.

A typical SLURM resource request resembles:

```bash
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
#SBATCH --mem=32000
#SBATCH --time=4-00:00
```

The automator reads:

```text
SLURM_CPUS_PER_TASK
```

and supplies the value to MetaQUAST using:

```text
-t <SLURM_CPUS_PER_TASK>
```

The number of CPUs requested from SLURM and the number of threads supplied to MetaQUAST should therefore remain synchronized.

MetaQUAST resource requirements can vary depending on:

- Number of assemblies
- Total assembly size
- Number of contigs
- Metagenomic complexity
- Reference-search behaviour
- Network connectivity
- MetaQUAST version
- Enabled internal analyses

Resource usage should be reviewed using SLURM accounting tools after representative jobs.

For example:

```bash
sacct \
  -j <job_id> \
  --format=JobID,JobName,State,Elapsed,AllocCPUS,TotalCPU,MaxRSS,ReqMem
```

If available, the following may also be used:

```bash
seff <job_id>
```

---

## Temporary Files and Cleanup

After successful completion, the automator removes:

```text
<work_dir>/sequences
<work_dir>/metaquast_output_<issue_id>
<work_dir>/metaquast_output_<issue_id>.zip
<work_dir>/run_metaquast.sh
```

The Excel report copied to the issue work directory is retained:

```text
<work_dir>/MetaQUAST_results_redmine<issue_id>.xlsx
```

If an exception occurs before successful completion, temporary files are retained for troubleshooting.

Retained files may include:

- Retrieved FASTA links or files
- Partial MetaQUAST output
- MetaQUAST logs
- Generated shell script
- ZIP archive, if archive creation completed
- Partially generated reports

---

## Implementation Notes

Maintainers should be aware of the following implementation details:

- The automator runs as one SLURM task with multiple CPUs.
- The thread count is obtained from `SLURM_CPUS_PER_TASK`.
- A configured fallback is used when the automator runs outside SLURM.
- The MetaQUAST executable is invoked through the Python interpreter in the QUAST environment.
- No user-supplied reference argument is supported.
- Blank description lines are ignored.
- Duplicate SEQIDs are removed while preserving order.
- Missing FASTA files generate a warning but do not stop the analysis if other requested assemblies were found.
- MetaQUAST does not run if no requested assemblies are available.
- Only FASTA files associated with requested SEQIDs are included.
- Command arguments are shell-quoted using `shlex.join()`.
- The generated shell script is made executable.
- The shell script is run using `subprocess.run(..., check=True)`.
- A nonzero MetaQUAST exit status raises an exception.
- The expected `transposed_report.tsv` file is explicitly validated.
- Successful Excel conversion is explicitly validated.
- Successful ZIP archive creation is explicitly validated.
- Failed runs preserve intermediate files.
- Successful runs remove temporary sequences, output, ZIP, and shell-script files.
- The Excel attachment remains in the issue work directory.
- Exceptions are reported to both Sentry and Redmine.
- Exceptions are re-raised so that SLURM records failed analyses correctly.
- The Redmine issue is assigned status ID `4` after successful completion.
- The shared exception handler also assigns status ID `4` after an error.
- Dropbox credentials are imported from the local `tokens` module.
- The traceback posted to Redmine may contain internal paths and command details.

---

## Maintainer Warnings

The following behaviours should be considered during future maintenance:

- The shared `tsv_to_xlsx()` method reports conversion errors without raising them. The automator therefore verifies that the Excel output exists.
- The filename matching pattern is broad:

```text
<SEQID>*fasta*
```

A SEQID could match more than one FASTA file. All matching files are currently added to the MetaQUAST command.

- Missing FASTA files do not stop the analysis when at least one assembly remains.
- The automator assumes that MetaQUAST creates a top-level `transposed_report.tsv`.
- Output structure may differ between MetaQUAST versions.
- Reference-free MetaQUAST behaviour may depend on reference-search resources, databases, and network availability.
- The Dropbox failure message may represent connectivity, authentication, storage, or API problems.
- Cleanup is performed only after the final Redmine update succeeds.
- A cleanup filesystem error could affect the final SLURM state unless cleanup errors are handled separately.
- Status ID `4` is used for successful analyses, missing-SEQID requests, and exceptions through the shared exception handler.
- The default thread count is used only when `SLURM_CPUS_PER_TASK` is unavailable.
- The automator requires Python 3.10 or newer because it uses modern type-annotation syntax such as:

```python
Path | None
```

- The Redmine launcher must continue using the supported external options:

```text
--redmine_instance
--issue
--work_dir
--description
```

These are mapped internally to variables named:

```text
redmine_instance_pickle
issue_pickle
work_dir
description_pickle
```

These implementation details do not necessarily prevent normal operation, but they should be considered when modifying, deploying, or troubleshooting the MetaQUAST automator.
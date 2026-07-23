# QUAST Redmine Automator

## Overview

The QUAST Redmine Automator runs a https://quast.sourceforge.net/ assembly-quality analysis using FASTA assemblies stored on the OLC NAS.

The automator:

1. Reads SEQIDs and optional settings from a Redmine issue.
2. Retrieves the requested FASTA assemblies from the NAS.
3. Runs QUAST on the assemblies.
4. Converts the transposed QUAST report to Excel format.
5. Attaches the Excel report to the Redmine issue.
6. Compresses the complete QUAST output.
7. Uploads the compressed results to Dropbox.
8. Updates and closes the Redmine issue.

---

## When to Use This Analysis

Use the QUAST automator to:

- Compare assembly statistics for one or more isolates.
- Assess assembly quality.
- Review metrics such as:
  - Total assembly length
  - Number of contigs
  - N50
  - L50
  - GC content
  - Largest contig
- Generate QUAST Circos plots.

QUAST evaluates assembled genomes. It does not assemble raw sequencing reads.

---

## Redmine Issue Format

Enter one SEQID per line in the issue description.

### Basic analysis

```text
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

This runs QUAST on the listed assemblies without a reference genome.

### Analysis with Circos output

Add `circos` on a separate line:

```text
circos
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

This adds the `--circos` option to the QUAST command.

### Formatting requirements

- Enter one SEQID per line.
- Do not separate SEQIDs with commas.
- The setting name `circos` must be lowercase.
- Lines that do not contain `circos` are treated as SEQIDs.
- Blank lines should be avoided.

---

## Arguments

### `circos`

Requests QUAST Circos output.

```text
circos
```

When supplied, the automator adds:

```text
--circos
```

---

## Analysis Workflow

### 1. Initialize the automator

The automator:

- Disables `urllib3` warnings.
- Initializes Sentry error reporting.
- Unpickles the Redmine API instance.
- Unpickles the Redmine issue.
- Unpickles the parsed issue description.

### 2. Parse the issue description

Each line is processed as follows:

- A line containing `circos` enables Circos output.
- Every other line is treated as a SEQID.

If no SEQIDs are supplied, the automator updates the issue with:

```text
WARNING: No SEQIDs provided!
```

The issue is then closed without running QUAST.

### 3. Retrieve assemblies

The automator creates:

```text
<work_dir>/sequences
```

It then uses `nastools.retrieve_nas_files()` to retrieve FASTA assemblies from the OLC NAS:

```python
retrieve_nas_files(
    seqids=seqids,
    outdir=seq_dir,
    filetype="fasta",
    copyflag=False,
)
```

Because `copyflag` is set to `False`, the retrieval utility may create links rather than copying the assembly files, depending on the implementation of `nastools`.

### 4. Check for missing FASTA files

For each requested SEQID, the automator searches for a matching file using:

```text
<SEQID>*fasta*
```

If a requested assembly cannot be found, the issue is updated with a warning listing the missing SEQIDs.

The current implementation reports missing assemblies but does not stop the analysis. QUAST continues with any FASTA files that were successfully retrieved.

### 5. Construct the QUAST command

The base command is:

```bash
quast -t 8 -o <work_dir>/quast_output_<issue_id>
```

Optional arguments are appended based on the issue description.

A run without optional arguments resembles:

```bash
quast -t 8 \
  -o <work_dir>/quast_output_<issue_id> \
  <work_dir>/sequences/*.fasta
```

The complete command is added to the Redmine issue before execution.

### 6. Run QUAST

The automator activates the QUAST Conda environment:

```bash
source /home/ubuntu/miniconda3/etc/profile.d/conda.sh &&
conda activate /mnt/nas2/virtual_environments/quast
```

It writes the activation and QUAST commands to:

```text
<work_dir>/run_quast.sh
```

The generated shell script is then executed.

### 7. Convert the report to Excel

QUAST produces:

```text
quast_output_<issue_id>/transposed_report.tsv
```

The automator converts this file to:

```text
Quast_results_redmine<issue_id>.xlsx
```

The Excel report is copied to the Redmine issue work directory and prepared as an issue attachment.

### 8. Determine the QUAST version

The automator obtains the installed QUAST version using the executable located at:

```text
/mnt/nas2/virtual_environments/quast/bin/quast
```

The version is included in the final Redmine update.

### 9. Compress and upload the complete results

The complete QUAST output directory is compressed as:

```text
quast_output_<issue_id>.zip
```

The ZIP archive is uploaded to Dropbox. If successful, the returned download URL is included in the final issue update.

### 10. Complete the issue

When the analysis and upload succeed, the automator:

- Attaches the Excel report.
- Reports the QUAST version.
- Provides the Dropbox download URL.
- Changes the issue status to `4`.

The final message resembles:

```text
Quast analysis complete!
Quast version is <version>

Report file has been attached to this issue.
Full Results are available at the following URL:
<Dropbox URL>
```

If the Dropbox upload fails, the Excel report is still attached and the issue is updated with:

```text
Upload of results to dropbox was unsuccessful due to connectivity issues.
Please try again later.
```

---

## Outputs

### Excel report

```text
Quast_results_redmine<issue_id>.xlsx
```

This file is generated from QUAST's `transposed_report.tsv` and attached directly to the Redmine issue.

### Complete output archive

```text
quast_output_<issue_id>.zip
```

This archive contains the complete QUAST output and is uploaded to Dropbox.

Depending on the QUAST options and input data, the archive may include:

- HTML reports
- Text reports
- TSV reports
- Transposed reports
- PDF reports
- Contig statistics
- Circos-related output

### Redmine updates

The issue receives updates for:

- Missing SEQIDs
- The complete QUAST command
- Successful completion
- QUAST version
- Dropbox download URL
- Dropbox upload failure
- Unexpected processing errors

---

## Interpreting Common QUAST Metrics

### Number of contigs

The total number of contigs in the assembly.

A lower number often indicates a more contiguous assembly, but the expected value depends on the organism, sequencing technology, and assembly method.

### Largest contig

The length of the longest contig in the assembly.

### Total length

The combined length of all contigs in the assembly.

Compare this value with the expected genome size. A substantially larger or smaller assembly may indicate contamination, missing sequence, duplicated sequence, or assembly problems.

### N50

The contig length at which at least 50% of the total assembly length is contained in contigs of that length or longer.

A larger N50 generally indicates a more contiguous assembly. N50 should not be interpreted by itself because assemblies with contamination or incorrectly joined contigs can also have high N50 values.

### L50

The minimum number of largest contigs needed to account for at least 50% of the total assembly length.

A lower L50 generally indicates a more contiguous assembly.

### GC content

The percentage of bases in the assembly that are guanine or cytosine.

Unexpected GC content may indicate contamination, an incorrect species assignment, or poor assembly quality.

---

## Troubleshooting

### No SEQIDs provided

**Redmine message:**

```text
WARNING: No SEQIDs provided!
```

**Cause:** The issue description did not contain any lines that the automator recognized as SEQIDs.

**Resolution:**

- Add at least one SEQID.
- Enter one SEQID per line.
- Ensure that the description does not contain only `circos` settings.

---

### One or more FASTA files could not be found

**Redmine message:**

```text
WARNING: Could not find the following requested SEQIDs on the OLC NAS:
[...]
```

**Possible causes:**

- The SEQID is incorrect.
- The assembly has not been generated.
- The assembly is not stored in the expected NAS location.
- The assembly filename does not match the expected `<SEQID>*fasta*` pattern.
- The NAS is unavailable.

**Resolution:**

1. Verify the SEQID spelling.
2. Confirm that the assembly exists on the NAS.
3. Confirm that its filename begins with the requested SEQID.
4. Confirm that the filename contains `fasta`.
5. Retry once NAS connectivity has been restored.

The analysis may still finish using the assemblies that were found.

---

### Circos output is missing

**Possible causes:**

- `circos` was not included in the issue description.
- The line used an unexpected spelling or capitalization.
- QUAST could not generate Circos output for the supplied inputs.
- A required external Circos dependency is unavailable.

**Resolution:**

Add the following on a separate line:

```text
circos
```

Review the complete QUAST output archive for warnings or errors.

---

### Excel report was not generated

**Possible causes:**

- QUAST failed before creating `transposed_report.tsv`.
- The shell script did not run successfully.
- The QUAST environment could not be activated.
- The output directory was not writable.
- The TSV-to-XLSX conversion failed.

**Resolution:**

1. Review the traceback posted to the issue.
2. Review the QUAST command posted to the issue.
3. Confirm that the QUAST Conda environment exists.
4. Confirm that `transposed_report.tsv` was generated.
5. Confirm that the issue work directory is writable.

---

### Dropbox upload failed

**Redmine message:**

```text
Upload of results to dropbox was unsuccessful due to connectivity issues.
Please try again later.
```

**Possible causes:**

- Network connectivity problems.
- Expired or invalid Dropbox credentials.
- Dropbox API availability issues.
- Insufficient Dropbox storage.
- A missing ZIP archive.

**Resolution:**

- Retry the analysis later.
- Verify the configured Dropbox credentials.
- Confirm that the ZIP archive was created successfully.
- Check network connectivity from the automator host.

The Excel summary report should still be attached to the issue.

---

### Unexpected automator error

**Redmine message begins with:**

```text
Something went wrong! Please contact a bioinformatician to investigate:
```

The remainder of the update contains the Python traceback.

**Resolution:**

Provide the following information to a bioinformatician:

- Redmine issue number
- Full traceback
- QUAST command posted to the issue
- Requested SEQIDs
- Whether Circos output was requested

The exception is also sent to Sentry.

---

## Temporary Files and Cleanup

After successful completion, the automator removes:

```text
<work_dir>/sequences
<work_dir>/quast_output_<issue_id>
<work_dir>/quast_output_<issue_id>.zip
```

The Excel report copied to the work directory is not explicitly removed by this script.

If an exception occurs before cleanup, temporary sequence, output, and ZIP files may remain in the issue work directory.

---

## Implementation Notes

The automator currently has several implementation details that maintainers should be aware of:

- QUAST always runs with eight threads.
- Missing FASTA files generate a warning but do not stop the analysis.
- The input glob includes every `*.fasta` file in the sequence directory.
- The Redmine issue is assigned status ID `4` after normal completion.
- The same status is assigned when no SEQIDs are supplied.
- The shell script is executed with `os.system()`.
- The return code from `os.system()` is not currently checked.
- Cleanup occurs only after the final Redmine update.
- Exceptions are reported to both Sentry and the Redmine issue.
- The traceback may contain internal paths and command details.
- Dropbox credentials are imported from the local `tokens` module.

---

## Maintainer Warnings

The source contains some duplicated or unused elements that should be reviewed during future maintenance:

- `pandas` is imported twice.
- Several imports appear unused.
- `verify_fasta_files_present`, `make_executable`, and `zip_folder` are imported from `methods` and then redefined locally.
- `check_fastqs_present()` is unrelated to the QUAST FASTA workflow.
- The `argument_flags` dictionary is defined but not used.
- The locally defined `verify_fasta_files_present()` docstring initially refers to FASTQ files instead of FASTA files.
- The constructed QUAST command is not shell-quoted, so paths containing spaces or shell-special characters could cause failures.
- Settings are currently case-sensitive.
- Blank lines may be added to the SEQID list.
- The Dropbox failure message attributes all failures to connectivity, although authentication or API errors may also be responsible.

These issues do not necessarily prevent normal operation, but they should be considered when modifying or troubleshooting the automator.
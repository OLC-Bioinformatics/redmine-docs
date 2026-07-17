# fastqmerge

## What does it do?

Use **fastqmerge** to combine raw FASTQ data from multiple sequencing runs of the same biological sample and save the merged data under a new output `SEQID`.

The automator supports:

- paired-end Illumina data — combines all selected R1 files into one merged R1 file and all selected R2 files into one merged R2 file;
- single-end MinION data — combines FASTQ files from multiple MIN `SEQID`s into one merged FASTQ file;
- requests containing both documented data types when the request syntax identifies them appropriately.

After merging, the automator reports the output paths and file sizes so the result can be verified.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
fastqmerge
```

The Subject is case-sensitive and spelling matters.

### Description

Provide one merge declaration per line. Each declaration must contain:

1. a comma-separated list of source `SEQID`s; and
2. the new merged output `SEQID` after an equals sign (`=`).

The request must also identify whether the source data are single-end FASTQ files or paired-end FASTQ files. The supplied documentation does not show the exact keyword or line syntax used to declare `single` versus `paired`; verify that part of the request against a known working issue or the current implementation before publishing this page as final.

General merge-declaration format:

```text
SOURCE_SEQID_1,SOURCE_SEQID_2=NEW_MERGED_SEQID
```

Example identifier structure for MinION data:

```text
2026-MIN-0001,2026-MIN-0002=2026-MER-0001
```

Do not combine data from different biological samples. All source `SEQID`s in a merge declaration must represent sequencing runs from the same sample.

### Attachments

No attachment is required. fastqmerge retrieves the raw FASTQ data associated with the source `SEQID`s.

### Optional parameters

The supplied documentation does not identify optional parameters for fastqmerge.

### Example

See [issue 36575](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/36575) for a working request that shows the required data-type declaration and merge format.

Before copying the example, replace every source and output identifier with the `SEQID`s appropriate for your sample.

## Interpreting results

When fastqmerge finishes, it saves the merged sequence data on NAS2 under the new merged `SEQID`. The merged data are then available for supported downstream analyses.

For paired-end Illumina input, verify that the result contains synchronized merged R1 and R2 files. For MinION input, verify that the result contains the expected single merged FASTQ file.

The automator reports output file paths and file sizes. Use those values to confirm that the expected files were created and that the output size is consistent with the combined inputs.

## How long does it take?

Combining two MIN sequences into one merged MIN sequence typically takes approximately three minutes. Runtime increases with the number and size of source FASTQ files and current service workload.

## What can go wrong?

### A source `SEQID` is unavailable

**Symptom:** The Redmine issue reports that one or more requested source sequences cannot be found.

**Likely cause:** A source `SEQID` is incorrect or its FASTQ data are unavailable.

**What to do:** Verify every source `SEQID` and confirm that the required raw-read files exist.

### The new merged `SEQID` is missing

**Symptom:** The automator rejects the request because no output MER identifier was provided.

**Likely cause:** The merge declaration does not contain a new output `SEQID` after `=`.

**What to do:** Add a valid new merged identifier using the documented comma-list and equals-sign format.

### The data type is missing or incorrect

**Symptom:** The automator cannot determine whether to create a single merged FASTQ file or paired R1/R2 outputs.

**Likely cause:** The request does not correctly identify the source data as single-end or paired-end.

**What to do:** Follow the exact data-type syntax shown in a current known-good request such as issue 36575.

### Source files do not belong to the same sample

**Symptom:** The merged output is biologically inappropriate even though file concatenation succeeds.

**Likely cause:** FASTQ files from different biological samples were included in the same declaration.

**What to do:** Verify sample identity before submitting the request. Create separate merged outputs for unrelated samples.

## Related automators

- [Downsample](downsample.md) — reduces the amount of data in existing FASTQ files rather than combining sequencing runs.
- [FastQC/MultiQC](fastqc.md) — assesses raw-read quality and can be used to review data before or after merging.

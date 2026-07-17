# FastQC/MultiQC

## What does it do?

Use **FastQC/MultiQC** to assess the quality of raw sequence data and review quality-control results across multiple requested sequences.

The automator runs:

- **FastQC** — performs quality-control checks on raw sequence data and creates an individual report for each sequence;
- **MultiQC** — aggregates the individual FastQC results into a combined report for comparison across the request.

For background, see the [FastQC website](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) and [MultiQC website](https://multiqc.info/).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
fastqc
```

Spelling matters, but matching is not case-sensitive.

### Description

In the **Description** field, enter one `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

The requested `SEQID`s must have raw sequence data available.

### Attachments

No attachment is required. The automator retrieves the raw sequence data associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine FastQC/MultiQC automator.

### Example

```text
2026-SEQ-0001
2026-SEQ-0002
```

See [issue 30311](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/30311) for an example FastQC/MultiQC request. Temporary result-download links associated with the issue may expire.

## Interpreting results

When the analysis finishes, the automator uploads:

- the combined MultiQC report; and
- a ZIP archive named using the Redmine issue identifier:

```text
fastqc_redmineID.zip
```

The archive contains individual FastQC report files and the combined MultiQC report.

Results are uploaded to both Redmine and Dropbox. This provides a download option when a request includes enough sequences that the complete report package is too large to attach directly to Redmine.

Use the individual FastQC reports to inspect per-sequence quality metrics. Use the MultiQC report to compare those metrics across all sequences in the request. The supplied documentation does not define project-specific pass/fail thresholds, so interpret warnings and failures in the context of the sequencing platform, intended downstream analysis, and expected library characteristics.

## How long does it take?

FastQC usually takes less than one minute per sequence. Total runtime depends on the number and size of the requested sequence files and current service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** The automator cannot locate raw sequence data for the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its raw sequence files are available, and submit a corrected request.

### The report package is too large for a Redmine attachment

**Symptom:** The complete report package is not attached directly to the Redmine issue.

**Likely cause:** A request containing many sequences produced reports that exceed the practical Redmine attachment size.

**What to do:** Use the Dropbox download provided by the automator.

## Related automators

- [Downsample](downsample.md) — reduces raw FASTQ data by coverage, output size, read count, base count, or other supported BBMap options.

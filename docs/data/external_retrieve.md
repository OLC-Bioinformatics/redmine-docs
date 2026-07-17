# External Retrieve

## What does it do?

Use **External Retrieve** to collect raw reads or draft genome assemblies for requested `SEQID`s and make them available for local download through the configured external transfer service.

External Retrieve is intended for users who need local copies of sequence files or need to share an exported data package. It does not run a biological analysis.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
External Retrieve
```

Spelling matters, but matching is not case-sensitive.

### Description

Specify the requested file type, then enter one `SEQID` per line:

- `fastq` — retrieve raw reads;
- `fasta` — retrieve draft genome assemblies.

#### Retrieve raw reads

```text
fastq
2026-SEQ-0001
2026-SEQ-0002
```

#### Retrieve assemblies

```text
fasta
2026-SEQ-0001
2026-SEQ-0002
```

### SRA filename formatting

The workflow can optionally rename retrieved FASTQ files for SRA submission. For example, a lane-specific filename such as:

```text
2014-SEQ-0349_S11_L001_R1_001.fastq.gz
```

can be simplified to:

```text
2014-SEQ-0349_R1.fastq.gz
```

The supplied documentation says the first Description line controls this option, but it does not provide the exact keyword or accepted value. Verify the syntax from a known working request or the current implementation before publishing a complete SRA-formatting example.

This option requires `fastq` retrieval.

### Attachments

No attachment is required. External Retrieve locates the requested data by `SEQID`.

### Optional parameters

No optional parameter other than the incompletely documented SRA filename-formatting option is identified in the supplied page.

### Examples

See [issue 12822](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/12822) for an example External Retrieve request.

See [issue 18760](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/18760) for an example involving SRA filename formatting.

## Interpreting results

When the request finishes, use the download link posted to the Redmine issue to retrieve the exported files.

Verify that:

- every expected `SEQID` is represented;
- the requested data type is correct;
- paired FASTQ files contain both mates when paired-end data were requested;
- SRA-formatted filenames were applied only when requested;
- the downloaded files are complete before using or sharing them.

The supplied documentation describes an FTP destination, but transfer infrastructure can change. Follow the link and instructions posted by the completed Redmine request.

## How long does it take?

Small requests generally finish within a few minutes. Runtime increases with the number and size of requested files and the time required to upload the resulting package.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that one or more sequences cannot be found.

**Likely cause:** The identifier is incorrect or the requested FASTA or FASTQ data are unavailable.

**What to do:** Verify each `SEQID` and confirm that the requested data type exists.

### The transfer times out

**Symptom:** The issue reports an upload failure such as `Connection reset by peer`.

**Likely cause:** The transfer service timed out, particularly for a large request.

**What to do:** Retry later or divide a large export into smaller requests. Escalate persistent failures to the bioinformatics team.

### SRA filename formatting is not applied

**Symptom:** Retrieved FASTQ files retain lane-specific names.

**Likely cause:** The SRA-renaming option was missing, used with `fasta`, or supplied with incorrect syntax.

**What to do:** Verify the current SRA-formatting keyword from a known working request and use it only with `fastq` retrieval.

## Alternatives when Redmine retrieval is unavailable

The legacy page describes direct NAS tooling and FoodPort File Zone as operational alternatives. These methods depend on local permissions, mounted storage, and the current FoodPort interface. They should be maintained in internal operational documentation rather than on the standard External Retrieve page.

## Related data workflows

- [Report Retrieve](report_retrieve.md) — retrieves COWBAT assembly reports for requested `SEQID`s.
- [SRA Download](sra_download.md) — imports runs from NCBI SRA, stores the reads, and submits them to FoodPort/COWBAT processing.

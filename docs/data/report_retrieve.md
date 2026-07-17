# Report Retrieve

## What does it do?

Use **Report Retrieve** to collect COWBAT assembly-pipeline reports for requested `SEQID`s and make them available through Dropbox.

Report Retrieve retrieves reports rather than raw reads or assembly FASTA files. Use [External Retrieve](external_retrieve.md) when sequence data are required.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
Report Retrieve
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter one assembly `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

No attachment is required. Report Retrieve locates reports associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for Report Retrieve.

### Example

See [issue 13931](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/13931) for an example Report Retrieve request.

The legacy page incorrectly called this an “External Retrieve” example; the link is retained here as the Report Retrieve example identified by that page.

## Interpreting results

When processing finishes, use the Dropbox link posted to the Redmine issue to download the retrieved report package.

Verify that the package contains reports for every available requested `SEQID`. If the issue warns about missing identifiers, the package may contain only the reports that could be located.

The supplied page does not identify the exact archive name or report directory structure. Confirm those details from a recent completed request before adding exact filenames.

## How long does it take?

Report Retrieve is generally fast and should process a normal request within a few minutes. Large result packages can take longer to upload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue warns that one or more requested sequences or reports cannot be found.

**Likely cause:** The identifier is incorrect or no COWBAT reports are available for that sequence.

**What to do:** Verify each `SEQID` and confirm that the corresponding assembly-pipeline reports exist.

### The Dropbox upload times out

**Symptom:** The issue reports that result-file upload was unsuccessful because of network connectivity.

**Likely cause:** The report package is large or the Dropbox connection encountered a temporary failure.

**What to do:** Retry later or split a large request into smaller batches. Contact the bioinformatics team if the problem persists.

## Related data workflows

- [External Retrieve](external_retrieve.md) — retrieves raw reads or draft assemblies for local download.
- [SRA Download](sra_download.md) — imports external SRA runs and submits them for FoodPort/COWBAT processing.

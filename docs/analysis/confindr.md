# ConFindr

## What does it do?

Use **ConFindr** when raw sequencing reads may be contaminated. ConFindr detects evidence of both intra-species and inter-species contamination before assembly or downstream analysis.

Contaminated reads can produce misassemblies and misleading downstream results. ConFindr is therefore a quality-control tool for raw reads, not a general species-identification workflow.

For background, see the [ConFindr documentation](https://lowandrew.github.io/ConFindr/).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
ConFindr
```

Spelling matters, but matching is not case-sensitive.

### Description

In the **Description** field, enter one `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

The requested `SEQID`s must have raw sequencing reads available.

### Attachments

No attachment is required. ConFindr retrieves the raw reads associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine ConFindr automator.

### Example

See [issue 12881](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/12881) for an example ConFindr request.

## Interpreting results

When ConFindr finishes, it uploads:

```text
confindr_output.zip
```

The archive contains:

```text
confindr_report.csv
confindr_log.txt
```

### `confindr_report.csv`

The report contains five columns:

- `Sample` — the sample identifier;
- `Genus` — the genus ConFindr assigns to the sample; more than one genus can be listed when detected;
- `NumContamSNVs` — the number of sites that resemble contaminating SNVs;
- `NumUniqueKmers` — the number of unique k-mers detected for rMLST genes;
- `ContamStatus` — `True` when ConFindr calls the sample contaminated and `False` when it calls the sample clean.

The documented contamination criteria are:

- `NumContamSNVs` of `3` or greater; or
- `NumUniqueKmers` greater than `45000`.

Use `ContamStatus` as the overall workflow call, and review the supporting SNV and k-mer values when investigating a result.

### `confindr_log.txt`

The log is primarily useful for diagnosing unexpected errors. It normally does not need to be interpreted as a biological result.

## How long does it take?

ConFindr generally takes approximately one to two minutes per sample. Total runtime depends on read volume, sample count, and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** ConFindr cannot locate raw reads for the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its raw sequence files are available, and submit a corrected request.

### A contamination call is unexpected

**Symptom:** `ContamStatus` is `True` even though the sample was expected to be clean.

**Likely cause:** ConFindr detected at least three contamination-like SNVs, more than 45,000 unique rMLST k-mers, or other supporting evidence reflected in the report.

**What to do:** Review `NumContamSNVs`, `NumUniqueKmers`, the assigned genus or genera, raw-read quality, and laboratory context before deciding whether to repeat sequencing or exclude the sample.

### The analysis fails unexpectedly

**Symptom:** No usable report is produced, or the Redmine issue reports an execution error.

**Likely cause:** The raw reads may be unavailable, malformed, or unsuitable for processing, or the workflow may have encountered a technical error.

**What to do:** Verify the input files and provide `confindr_log.txt` to a bioinformatician when escalation is required.

## Related automators

- [AutoCLARK](autoclark.md) — identifies species represented in raw reads or assemblies; it is not a dedicated contamination call.
- [FastQC/MultiQC](fastqc.md) — evaluates general raw-read quality but does not make ConFindr contamination calls.
- [Unknown Isolate](unknownisolate.md) — identifies an uncertain isolate from a draft genome assembly rather than testing raw reads for contamination.

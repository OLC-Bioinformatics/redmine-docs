# PSORTb

## What does it do?

Use **PSORTb** to predict the subcellular localization of proteins encoded by a genome assembly.

The workflow first uses Prokka to predict and annotate proteins, then runs PSORTb to assign localization categories. PSORTb was developed by the Brinkman laboratory at Simon Fraser University.

For background, see the [PSORTb publication](https://academic.oup.com/bioinformatics/article/26/13/1608/201357) and [PSORTb documentation](https://www.psort.org/documentation/index.html).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
PSORTb
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter one assembly `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

No attachment is required. PSORTb retrieves the assembly associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine PSORTb automator.

### Example

See [issue 16061](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/16061) for an example PSORTb request.

## Interpreting results

PSORTb uploads a ZIP archive containing:

- FASTA-formatted protein sequences predicted and annotated by Prokka; and
- one text report per `SEQID` containing the predicted subcellular localization of each protein.

Report filenames include either `grampos` or `gramneg`, reflecting whether the workflow classified the isolate as Gram-positive or Gram-negative for PSORTb analysis.

Subcellular-localization assignments are computational predictions. Review the reported localization category, prediction confidence or score when available, protein annotation, and organism biology before drawing conclusions.

The Gram classification affects the PSORTb model used. An incorrect Gram classification can therefore affect localization results.

## How long does it take?

PSORTb generally takes at least 20–30 minutes per requested `SEQID`. Total runtime depends on genome size, sample count, and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue warns that an assembly cannot be found.

**Likely cause:** The identifier is incorrect or the assembly is unavailable.

**What to do:** Verify each `SEQID` and confirm that its assembly exists.

### The isolate is assigned the wrong Gram class

**Symptom:** A result filename or report indicates `grampos` when `gramneg` was expected, or the reverse.

**Likely cause:** The workflow's automatic Gram classification was incorrect for the isolate.

**What to do:** Do not rely on the affected localization predictions. Contact the bioinformatics team with the issue number and expected Gram class so the analysis can be reviewed.

### A localization prediction is uncertain

**Symptom:** A protein is unclassified or has weak or conflicting localization evidence.

**Likely cause:** The sequence lacks sufficient localization signals, the protein is incomplete, or the automated annotation is uncertain.

**What to do:** Review the protein sequence and annotation and use appropriate supporting tools or experimental evidence.

## Related automators

- [Prokka](prokka.md) — produces general genome and protein annotations without PSORTb localization predictions.

# IntiminTyper

## What does it do?

Use **IntiminTyper** to assign subtypes to intimin (`eae`) genes in *Escherichia coli* sequence assemblies.

IntiminTyper uses [Phylotyper](https://github.com/superphy/insilico-subtyping), developed at the Public Health Agency of Canada, to compare an intimin allele with known subtypes and predict the closest subtype when an exact match is unavailable.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
intimin_typer
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter one *E. coli* assembly `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

Assemblies that are not *E. coli* can be included without causing the request to fail, but they are not expected to produce IntiminTyper results.

### Attachments

No attachment is required. IntiminTyper retrieves the assembly associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine IntiminTyper automator.

### Example

See [issue 16512](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/16512) for an example IntiminTyper request.

## Interpreting results

IntiminTyper uploads:

```text
intimin_predictions.tsv
```

The report identifies the predicted intimin subtype for each sample with a result. It distinguishes between:

- an exact match to a known allele; and
- a subtype prediction based on similarity to known alleles.

When the subtype is predicted rather than matched exactly, the report also provides a probability representing the model's confidence in that prediction.

Interpret an exact allele match more strongly than a similarity-based prediction. For predicted subtypes, review the reported probability and supporting biological context before drawing conclusions.

A requested `SEQID` may be absent from the report when it is not *E. coli*, does not contain a detectable `eae` gene, or could not otherwise produce a subtype result.

## How long does it take?

IntiminTyper generally takes approximately 30 seconds per sample. Total runtime depends on sample count and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The requested sample does not appear in the final report.

**Likely cause:** IntiminTyper could not locate the assembly associated with the requested `SEQID`.

**What to do:** Verify the `SEQID` and confirm that its assembly is available.

### A requested sample produces no subtype

**Symptom:** The `SEQID` is absent from `intimin_predictions.tsv` or has no usable subtype result.

**Likely cause:** The assembly may not be *E. coli*, may not contain a detectable `eae` gene, or may have insufficient sequence evidence for typing.

**What to do:** Confirm the organism, review assembly quality, and verify whether the sample is expected to carry `eae`.

### A predicted subtype has low confidence

**Symptom:** The result is similarity-based and has a low reported probability.

**Likely cause:** The detected allele differs from known reference alleles or the available sequence is incomplete.

**What to do:** Treat the subtype as provisional and review the allele sequence, probability, and supporting evidence.

## Related automators

- [ECTyper](ectyper.md) — predicts O- and H-antigen serotypes for *E. coli* assemblies.
- [MLST](mlst.md) — determines an *E. coli* sequence type using a selected Achtman or Pasteur scheme.
- [PrimerFinder](primerfinder.md) — performs *in silico* PCR against assemblies using VTyper or attached custom primers.

# ECTyper

## What does it do?

Use **ECTyper** to predict the O-antigen and H-antigen serotype of *Escherichia coli* draft genome assemblies.

ECTyper reports predicted O-type, H-type, and supporting allele evidence. It is specific to *E. coli* serotyping and does not replace MLST sequence typing or broad organism identification.

The source code is available in the [PHAC-NML ECTyper repository](https://github.com/phac-nml/ecoli_serotyping).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
ec_typer
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter one *E. coli* assembly `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

No attachment is required. ECTyper retrieves the draft assembly associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine ECTyper automator.

### Example

See [issue 15946](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/15946) for an example ECTyper request.

## Interpreting results

ECTyper uploads:

```text
ec_typer_report.tsv
```

The report includes the sample name, predicted O-type, predicted H-type, and supporting allele results. Example:

| Name | O-type | H-type | O-antigen allele evidence | Additional O-antigen evidence | H-antigen allele evidence |
|---|---|---|---|---|---|
| `2014-SEQ-0276` | `O157` | `H7` | `wzx: 1.00` | `wzy: 0.58` | `fliC: 1.00` |
| `2019-SEQ-0137` | `O146` | `H8` | `wzx: 1.00` | `wzy: 0.56` | `fliC: 1.00` |
| `2019-SEQ-0145` | `-` | `H2` |  |  | `fliC: 1.00` |

A dash (`-`) means that ECTyper did not assign that serotype component. In the example above, `2019-SEQ-0145` has an H-type prediction but no O-type prediction.

The allele values provide supporting match evidence for loci such as `wzx`, `wzy`, and `fliC`. Interpret partial or discordant support cautiously and review assembly quality when an expected O-type or H-type is missing.

## How long does it take?

ECTyper generally takes approximately one minute per genome. Total runtime depends on the number of requested assemblies and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable assemblies.

**Likely cause:** ECTyper cannot locate the draft assembly associated with the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its assembly is available, and submit a corrected request.

### The requested assembly is not *E. coli*

**Symptom:** The report is missing, uninformative, or does not provide meaningful O- and H-type predictions.

**Likely cause:** ECTyper is designed for *Escherichia coli* assemblies.

**What to do:** Confirm the organism before interpreting the result. Use an identification workflow such as [Unknown Isolate](unknownisolate.md) when species identity is uncertain.

### An O-type or H-type is not assigned

**Symptom:** The report shows `-` for one serotype component.

**Likely cause:** The required allele was not detected with sufficient support, the assembly is incomplete, or the serotype is not represented adequately by the method.

**What to do:** Review the supporting allele fields and assembly quality before drawing conclusions.

## Related automators

- [MLST](mlst.md) — determines an *E. coli* sequence type using a selected Achtman or Pasteur scheme.
- [Unknown Isolate](unknownisolate.md) — investigates uncertain genus or species identity using rMLST, MASH, and ANI.
- [IntiminTyper](intimintyper.md) — performs intimin typing for supported *E. coli* analyses.

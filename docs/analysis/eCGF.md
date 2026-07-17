# eCGF

## What does it do?

Use **eCGF** to perform *in silico* comparative genomic fingerprinting (CGF) subtyping of *Campylobacter* from whole-genome sequence assemblies in FASTA format.

The automator evaluates a defined group of CGF loci and reports the closest represented profile together with the number of matching loci.

The source code is available in the [cgfprediction repository](https://github.com/dorbarker/cgfprediction).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
eCGF
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter one *Campylobacter* assembly `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

No attachment is required. eCGF retrieves the FASTA-formatted assembly associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine eCGF automator.

### Example

See [issue 16196](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/16196) for an example eCGF request.

## Interpreting results

The automator uploads a summary report named using the Redmine issue number:

```text
ISSUE_NUMBER_summary_report.csv
```

The report contains one row per requested genome. It includes binary results for the CGF loci, the nearest represented profile, and the number of matching loci out of 40.

Example excerpt:

| Sample | `11168_cj0008` | `11168_cj0033` | `11168_cj0035` | Nearest Match | Number of Similarities (/40) |
|---|---:|---:|---:|---|---:|
| `2018-LET-0106` | 0 | 0 | 1 | `27_3_4` | 40 |
| `2018-LET-0012` | 0 | 0 | 1 | `926_2_1` | 40 |

A value of `1` or `0` records the reported state for a CGF locus. `Nearest Match` identifies the closest represented CGF profile. `Number of Similarities (/40)` indicates how many of the 40 loci agree with that profile.

A score of `40` means that all evaluated loci match the reported profile. A lower score means that one or more loci differ; interpret the profile assignment together with the complete locus pattern and assembly quality.

## How long does it take?

eCGF generally takes a few seconds per genome. Total runtime depends on the number of requested assemblies and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable assemblies.

**Likely cause:** eCGF cannot locate the FASTA-formatted assembly associated with the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its assembly is available, and submit a corrected request.

### The requested assembly is not suitable *Campylobacter* data

**Symptom:** The report is missing, incomplete, or does not provide a meaningful nearest profile.

**Likely cause:** eCGF is intended for *Campylobacter* whole-genome assemblies, or the assembly may be incomplete or poor quality.

**What to do:** Confirm the organism and review assembly quality before interpreting the result.

### The nearest profile has fewer than 40 matching loci

**Symptom:** `Number of Similarities (/40)` is less than `40`.

**Likely cause:** The query differs from the nearest represented profile at one or more loci, or loci may be absent because of assembly quality.

**What to do:** Review the complete binary locus pattern and avoid treating a partial match as an exact profile assignment.

## Related automators

- [MLST](mlst.md) — determines a conventional sequence type for supported *Campylobacter* organisms and schemes.
- [IntiminTyper](intimintyper.md) and [ECTyper](ectyper.md) — provide specialized typing for *Escherichia coli*, not *Campylobacter*.

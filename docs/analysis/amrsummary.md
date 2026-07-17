# AMRsummary

## What does it do?

Use **AMRsummary** to detect acquired antimicrobial-resistance genes in FASTA-formatted draft genome assemblies, predict plasmid contigs, and summarize whether detected AMR genes are located on a predicted plasmid or chromosome.

AMRsummary combines:

- **ResFinder** — detects acquired AMR genes in draft genome assemblies;
- **MOB-suite `mob_recon`** — reconstructs and types predicted plasmids from draft genome assemblies.

The Redmine ResFinder workflow does not detect resistance caused by chromosomal point mutations. Use [PointFinder](pointfinder.md), [StarAMR](staramr.md), or [CARD-RGI](cardrgi.md) when mutation-based resistance must also be assessed.

For additional MOB-suite output details, see the [MOB-suite repository](https://github.com/phac-nml/mob-suite).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
amr summary
```

Spelling matters, but matching is not case-sensitive.

### Description

In the **Description** field, enter one `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

Each requested `SEQID` must have a FASTA-formatted draft genome assembly available.

### Attachments

No attachment is required. AMRsummary retrieves the assemblies associated with the requested `SEQID`s.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine AMRsummary automator.

### Example

```text
2026-SEQ-0001
2026-SEQ-0002
```

See [issue 14100](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/14100) for an example AMRsummary request.

## Interpreting results

AMRsummary uploads three reports.

### `resfinder_blastn.xlsx`

This workbook lists acquired AMR genes detected in each sample. Review:

- `PercentIdentity` — sequence identity between the detected gene and reference target;
- `PercentCovered` — coverage of the reference target.

A hit with `100` for both identity and coverage provides stronger evidence that the complete acquired gene is present. Lower-identity or partially covered hits require further review. A listed gene does not by itself establish an expressed resistance phenotype.

### `mob_recon_summary.csv`

This report lists contigs that MOB-suite predicts to be plasmid-derived. Contigs predicted to be chromosomal are omitted.

Important fields include:

- `Location` — the predicted plasmid name;
- `Contig` — the contig predicted to contain plasmid sequence.

A predicted plasmid can contain several contigs when it could not be circularized.

### `amr_summary.csv`

This combined report maps the contigs containing acquired AMR-gene hits to the MOB-suite plasmid predictions. It includes the predicted location and plasmid incompatibility types when available.

The `Location` field reports either:

- `chromosome`; or
- the name of a predicted plasmid.

The location is a computational prediction and should be interpreted with the underlying ResFinder and MOB-suite evidence.

## How long does it take?

ResFinder is fast, while MOB-suite is comparatively slower. Expect AMRsummary to take a few minutes per requested `SEQID`, depending on assembly size, sample count, and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** AMRsummary cannot locate a draft genome assembly for the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its FASTA assembly is available, and submit a corrected request.

### Expected mutation-based resistance is absent

**Symptom:** The report contains acquired-gene findings but not an expected chromosomal resistance mutation.

**Likely cause:** AMRsummary uses the Redmine ResFinder workflow, which does not detect resistance caused by chromosomal point mutations.

**What to do:** Use [PointFinder](pointfinder.md), [StarAMR](staramr.md), or [CARD-RGI](cardrgi.md), as appropriate.

### An AMR gene has an uncertain predicted location

**Symptom:** A detected gene cannot be confidently assigned to a predicted plasmid or chromosome.

**Likely cause:** Draft assemblies can fragment plasmids across contigs, and plasmid reconstruction is predictive.

**What to do:** Review `resfinder_blastn.xlsx`, `mob_recon_summary.csv`, and the underlying assembly evidence together before drawing conclusions.

## Related automators

- [ResFinder](resfinder.md) — detects acquired AMR genes in draft genome assemblies without plasmid-location summarization.
- [MobSuite](mobsuite.md) — detects, reconstructs, and types predicted plasmids in draft genome assemblies.
- [Plasmid-Borne Identity](plasmidborneidentity.md) — searches assemblies for user-supplied target genes and predicts whether matching targets are plasmid-borne.
- [PointFinder](pointfinder.md) — detects supported chromosomal resistance mutations.

# Sipprverse

## What does it do?

Use **Sipprverse** to detect predefined or custom gene targets directly in raw or paired-end FASTQ reads. Sipprverse operates on unassembled read data; use [GeneSeekr](geneseekr.md) when the targets must be detected in FASTA-formatted assemblies. KMA can also analyze raw reads, but Sipprverse is the workflow intended for the supplied Sipprverse target analyses.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
sipprverse
```

Spelling matters, but matching is not case-sensitive.

### Description

The **Description** field must contain:

1. an analysis declaration in the form `analysis=requested_analysis`; and
2. one `SEQID` per line.

Example structure:

```text
analysis=resfinder
2026-SEQ-0001
2026-SEQ-0002
```

#### Supported analyses

- `gdcs` — detects genomically dispersed conserved sequences in *Escherichia*, *Listeria*, *Salmonella*, and *Vibrio*.
- `genesippr` — uses a custom suite of genes derived from *Bacillus*, *Campylobacter*, *Escherichia*, *Listeria*, *Salmonella*, *Staphylococcus*, and *Vibrio*.
- `mash` — finds the closest matching RefSeq genome.
- `mlst` — determines multilocus sequence type for *Bacillus*, *Campylobacter*, *Escherichia*, *Listeria*, *Salmonella*, *Staphylococcus*, and *Vibrio*.
- `pointfinder` — detects chromosomal mutations predictive of drug resistance.
- `resfinder` — identifies acquired antimicrobial-resistance genes.
- `rmlst` — determines ribosomal multilocus sequence type.
- `serosippr` — calculates the serotype for *Escherichia*.
- `sixteens` — determines the closest 16S match.
- `virulence` — detects virulence genes.
- `full` — runs all analyses listed above.
- `custom` — detects targets from a user-supplied FASTA file. This analysis requires an attachment.

### Attachments

Most standard analyses do not require an attachment.

For `analysis=custom`, attach a FASTA-formatted file containing the target sequences.

### Optional parameters

#### `cutoff`

Sets the minimum cutoff for matches included in a report.

- Default: `0.90`
- Example: `cutoff=0.85`

#### `averagedepth`

Sets the average pileup-depth cutoff.

- Default: `2`
- Example: `averagedepth=3`

#### `kmersize`

Sets the k-mer size used for baiting.

- Default: `19`
- Example: `kmersize=11`

Avoid lowering this value excessively. The supplied documentation recommends approximately `11` as the practical lower limit.

#### `allowsoftclips`

Controls whether hits containing internal soft clips are automatically discarded.

- Default: `False`
- Example: `allowsoftclips=True`

### Examples

#### Standard request

```text
analysis=resfinder
2026-SEQ-0001
2026-SEQ-0002
```

See [issue 15706](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/15706) and [issue 15707](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/15707) for example Sipprverse issues.

#### Custom request

```text
analysis=custom
2026-SEQ-0001
```

Attach the FASTA-formatted target file to the Redmine issue.

## Interpreting results

When Sipprverse finishes, it uploads:

```text
sipprverse_output.zip
```

The archive contains the reports generated for the selected analysis. Report contents vary by analysis.

## How long does it take?

Runtime depends on the selected analysis, read volume, and number of requested `SEQID`s. Because Sipprverse processes raw reads, expect a few minutes per `SEQID`.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** Sipprverse cannot locate the raw paired-end FASTQ data for the requested `SEQID`.

**What to do:** Verify each `SEQID` and confirm that its raw paired-end reads are available.

### The analysis is missing, misspelled, or unsupported

**Symptom:** The issue receives an error describing the requested analysis.

**Likely cause:** The Description omits `analysis=...`, contains a spelling error, or requests an unsupported analysis.

**What to do:** Choose one of the documented analysis names and submit a corrected request.

### A custom target file is missing or unreadable

**Symptom:** The custom analysis cannot read or use its target database.

**Likely cause:** The FASTA-formatted target file was not attached or could not be read.

**What to do:** Attach a valid FASTA-formatted target file and submit a corrected `analysis=custom` request.

## Related automators

- [GeneSeekr](geneseekr.md) — use for target detection in FASTA-formatted assemblies.
- [KMA](kma.md) — use for supported resistance or toxin targets, or custom targets, in assemblies or raw reads.

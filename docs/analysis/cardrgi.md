# CARD-RGI

## What does it do?

Use **CARD-RGI** to predict the resistome of bacterial isolate assemblies or raw FASTQ data using the Comprehensive Antibiotic Resistance Database and Resistance Gene Identifier.

CARD-RGI supports two analysis modes:

- `isolate` — analyzes bacterial isolate sequence assemblies;
- `metagenome` — analyzes metagenomic FASTQ files and can also be used to analyze isolate FASTQ files.

The Redmine metagenome workflow uses KMA to align CARD targets to raw reads. The Redmine automator does not expose the alternative BWA or Bowtie 2 aligners available in the standalone CARD-RGI software.

For background and output definitions, see the [CARD website](https://card.mcmaster.ca/analyze/rgi), [CARD-RGI documentation](https://github.com/arpcard/rgi#rgi-usage-documentation), and [Alcock et al. (2020)](https://pubmed.ncbi.nlm.nih.gov/31665441/). Cite the CARD authors when publishing results produced with this automator.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
CARDRGI
```

Spelling matters, but matching is not case-sensitive.

### Description

The **Description** field must contain:

1. an analysis declaration as the first line; and
2. one `SEQID` per line.

#### Isolate assemblies

Use `analysis=isolate` for bacterial isolate sequence assemblies:

```text
analysis=isolate
2026-SEQ-0001
2026-SEQ-0002
```

#### Raw FASTQ data

Use `analysis=metagenome` for metagenomic FASTQ files. This mode can also force resistome analysis of isolate FASTQ files:

```text
analysis=metagenome
2026-SEQ-0001
```

### Attachments

The supplied documentation does not identify a required attachment for either supported analysis mode. CARD-RGI retrieves the requested sequence data using the listed `SEQID`s.

### Optional parameters

#### `loosehits`

Controls whether the results include CARD-RGI loose hits in addition to strict and perfect matches.

- Default: `False`
- Enable with: `loosehits=TRUE`

Loose hits may or may not contribute to resistance and require careful interpretation.

#### `partialgenes`

Controls whether partial gene matches are included.

- Default: `False`
- Enable with: `partialgenes=TRUE`

### Examples

#### Isolate analysis

```text
analysis=isolate
loosehits=TRUE
2026-SEQ-0001
2026-SEQ-0002
```

#### Raw-read analysis

```text
analysis=metagenome
2026-SEQ-0001
```

See [issue 28111](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/28111) for an example CARD-RGI request. The example uses `analysis=metagenome` with an isolate sequence rather than a true metagenome. Temporary result-download links associated with the issue may expire.

## Interpreting results

When CARD-RGI finishes, it uploads an archive named using the Redmine issue identifier:

```text
card-rgi_output_redmineID.zip
```

### Isolate analysis

The archive contains a combined CSV file:

```text
CARDRGI_output.csv
```

This file summarizes AMR gene results for all requested sample sequences. Review the `Best_Identities` column, which reports the percentage identity between the sequence and its top CARD hit.

A result with `100` percent identity provides stronger evidence for the reported target, but identity alone does not establish a resistance phenotype. Efflux systems and point mutations may confer resistance only in particular genera or species. Interpret every result in its organism-specific context.

The isolate archive also contains:

- individual result files for each sequence; and
- an RGI heatmap file summarizing all isolate sequences in the request.

For detailed column definitions, consult the **RGI main Tab-Delimited Output Details** in the [CARD-RGI documentation](https://github.com/arpcard/rgi#rgi-usage-documentation).

### Metagenome analysis

The archive contains individual output files for each analyzed sequence and two combined mapping files:

```text
CARDRGI_gene_mapping_output.csv
CARDRGI_allele_mapping_output.csv
```

- `CARDRGI_gene_mapping_output.csv` summarizes resistance-gene results detected in the raw FASTQ data.
- `CARDRGI_allele_mapping_output.csv` reports the top allele hits for each sequence.

## How long does it take?

Runtime depends on the selected analysis mode, amount of sequence data, enabled options, and number of requested sequences. Expect approximately two to five minutes per sequence.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** CARD-RGI cannot locate the assembly or raw FASTQ files required for the selected analysis mode.

**What to do:** Verify each `SEQID`, confirm that the required sequence-data type is available, and submit a corrected request.

### The analysis mode does not match the available input

**Symptom:** The automator cannot locate or process the expected data for one or more `SEQID`s.

**Likely cause:** `analysis=isolate` was requested for raw reads, or `analysis=metagenome` was requested when the intended input was an assembly.

**What to do:** Use `analysis=isolate` for bacterial isolate assemblies and `analysis=metagenome` for raw FASTQ data.

### Loose or partial hits are overinterpreted

**Symptom:** A reported hit appears weak, incomplete, or inconsistent with the organism's expected resistance profile.

**Likely cause:** `loosehits=TRUE` or `partialgenes=TRUE` included lower-confidence or incomplete matches.

**What to do:** Review identity, match category, gene completeness, organism context, and CARD documentation before drawing conclusions.

## Related automators

- [ResFinder](resfinder.md) — use to detect acquired AMR genes in draft genome assemblies; it does not detect resistance caused by chromosomal point mutations.
- [StarAMR](staramr.md) — combines acquired-gene detection with supported PointFinder mutation detection for *Campylobacter* and *Salmonella* assemblies.
- [KMA](kma.md) — use for the Redmine KMA automator's curated AMR, biocide, metal, verotoxin, or custom target databases in assemblies or raw reads.
- [PointFinder](pointfinder.md) — use for supported chromosomal resistance mutations.

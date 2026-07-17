# Roary/Scoary

## What does it do?

Use **Roary** to calculate the bacterial pan-genome of a set of related isolate assemblies. Roary identifies core genes, accessory genes, and gene-presence/absence patterns across the submitted genomes.

Use **Scoary** to test associations between binary traits and genes in the Roary accessory genome. Scoary uses the `gene_presence_absence.csv` output produced by Roary together with a user-supplied traits file.

Roary is intended for related bacterial isolate genomes. It is not intended for metagenomic data or extremely diverse genome sets.

For background, see the [Roary publication](https://academic.oup.com/bioinformatics/article/31/22/3691/240757), the [Scoary repository](https://github.com/AdmiralenOla/Scoary), and the [Scoary publication](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-1108-8).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
roary
```

Spelling matters, but matching is not case-sensitive.

### Description

The first line must select an analysis:

```text
analysistype=REQUESTED_ANALYSIS
```

Enter analysis-specific parameters next, followed by one assembly `SEQID` per line.

### Supported analyses

#### `union`

Reports the union of genes found across all submitted isolates.

```text
analysistype=union
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

#### `intersection`

Reports genes shared by the submitted isolates—the core-gene intersection.

```text
analysistype=intersection
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

#### `complement`

Reports accessory genes that are not shared by every submitted isolate.

```text
analysistype=complement
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

#### `gene_multifasta`

Extracts aligned protein sequences for requested genes and creates one multi-FASTA file per gene.

Also include a comma-separated `genes` line. Gene names are case-sensitive:

```text
analysistype=gene_multifasta
genes=fliC,gyrA
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

#### `difference`

Reports gene differences between two isolate sets. Put `set_two` immediately before the second group:

```text
analysistype=difference
2026-SEQ-0001
2026-SEQ-0002
set_two
2026-SEQ-0003
2026-SEQ-0004
```

### Attachments and Scoary analysis

To run Scoary automatically, attach a CSV file named:

```text
traits.csv
```

The traits file must follow these rules:

- each row represents one isolate `SEQID`;
- the upper-left cell is blank;
- each remaining column represents one uniquely named trait;
- trait values are binary: `0` for absent and `1` for present;
- all `SEQID`s and trait names must be unique;
- avoid characters such as `%`, `;`, `,`, `/`, `&`, `[`, `]`, `@`, and `?`.

Example:

```csv
,Trait 1,Trait 2,Trait N
2026-SEQ-0001,1,0,1
2026-SEQ-0002,0,0,1
2026-SEQ-0003,1,0,1
```

Attaching a correctly formatted `traits.csv` causes Scoary to test associations between every trait column and accessory-gene presence/absence.

### Optional parameters

The supplied documentation does not identify other user-configurable Roary or Scoary parameters.

### Examples

See [issue 24968](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/24968) for an example Roary/Scoary request.

See [issue 25013](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/25013) for a `gene_multifasta` request. Historical Dropbox files for these issues are no longer available.

## Interpreting results

The automator provides Dropbox links for:

```text
prokka_output.zip
roary_output.zip
```

### `prokka_output.zip`

Contains Prokka annotation output for every submitted genome. Roary uses the `.gff` files from these annotations. See the [Prokka output documentation](https://github.com/tseemann/prokka#output-files) for descriptions of the other files.

### `roary_output.zip`

Contains standard Roary pan-genome outputs and analysis-specific files.

For `gene_multifasta`, the archive contains one protein multi-FASTA per requested gene, for example:

```text
gene_multifasta_fliC.fa
gene_multifasta_gyrA.fa
```

These files contain aligned protein sequences for the requested genes. They can be used for appropriate downstream protein-sequence analysis.

When a valid `traits.csv` is attached, the archive includes one or more Scoary CSV results corresponding to the supplied trait columns.

Interpret Scoary associations as statistical associations, not proof that a gene causes the trait. Review population structure, sample size, trait balance, multiple-testing results, gene annotation, and biological plausibility before drawing conclusions.

## How long does it take?

Prokka generally takes approximately two to three minutes per genome. Roary runtime then depends on genome count and analysis type. `gene_multifasta` and Scoary requests also scale with the number of requested genes or traits.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that one or more assemblies cannot be found.

**Likely cause:** An identifier is incorrect or its assembly is unavailable.

**What to do:** Verify every `SEQID` and confirm that its assembly exists.

### The analysis type is missing or unsupported

**Symptom:** The issue reports an `analysistype` error.

**Likely cause:** `analysistype` is absent, misspelled, or not one of the documented values.

**What to do:** Use `union`, `intersection`, `complement`, `gene_multifasta`, or `difference` exactly as documented.

### `gene_multifasta` does not produce the expected genes

**Symptom:** One or more requested gene FASTA files are absent.

**Likely cause:** The `genes` line is missing, a gene name has incorrect capitalization, or the gene is not represented in the pan-genome results.

**What to do:** Add a comma-separated `genes=` line and copy gene names with exact case.

### Scoary output is absent

**Symptom:** Roary completes, but no trait-specific Scoary CSV files appear.

**Likely cause:** `traits.csv` was missing, named incorrectly, malformed, or inconsistent with the submitted `SEQID`s. The legacy workflow may not report this as an explicit error.

**What to do:** Correct the filename and CSV structure, then submit the complete analysis again.

### The genome set is too diverse

**Symptom:** Annotation, clustering, or pan-genome results are unstable or difficult to interpret.

**Likely cause:** Roary was applied to genomes that are not sufficiently related.

**What to do:** Restrict the request to a coherent related isolate set and reassess organism identity and assembly quality.

## Related automators

- [Pyseer](pyseer.md) — performs microbial genome-wide association using k-mers or a previous Roary gene-presence/absence matrix while accounting for population structure.
- [Prokka](prokka.md) — produces standalone genome annotations without pan-genome analysis.
- [dRep](drep.md) — compares genomes by ANI or dereplicates a genome set before downstream analysis.

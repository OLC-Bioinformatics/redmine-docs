# Snippy

## What does it do?

Use **Snippy** for rapid haploid variant calling and core-genome alignment of closely related genomes against one reference.

Snippy can call variants for multiple isolates, produce a core alignment, and optionally clean the alignment and create phylogenetic trees with Gubbins and IQ-TREE.

Snippy supports paired-end and single-end raw-read data in the same analysis. Use [SNVPhyl](snvphyl.md) when the SNVPhyl-specific matrix, core-genome statistics, and Nextflow outputs are required and all query isolates have paired-end R1 and R2 files.

For background and full output definitions, see the [Snippy repository](https://github.com/tseemann/snippy).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
snippy
```

Spelling matters, but matching is not case-sensitive.

### Description

The first line selects the reference:

```text
reference=REFERENCE-SEQID
```

Enter each query `SEQID` on a subsequent line:

```text
reference=2026-MIN-0001
2026-SEQ-0002
2026-SEQ-0003
```

Choose a high-quality reference assembly that is closely related to all query isolates.

### Use an attached reference

Attach one FASTA-formatted reference file and use:

```text
reference=attached
2026-SEQ-0002
2026-SEQ-0003
```

### Optional parameters

#### `cleanup_&_tree`

Cleans the core SNP alignment with Gubbins and creates phylogenetic trees with IQ-TREE.

- Default: not enabled
- Enable with:

```text
cleanup_&_tree=True
```

The workflow currently uses the configured default Gubbins settings and IQ-TREE. Contact the bioinformatics team if additional options are required.

### Example

```text
reference=2026-MIN-0001
cleanup_&_tree=True
2026-SEQ-0002
2026-SEQ-0003
```

See [issue 30388](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/30388) for an example Snippy request.

## Interpreting results

The Snippy result archive contains one folder for each comparator sequence and shared core-alignment files. Some important files can also be attached directly to the Redmine issue.

### `core.txt`

A tab-separated summary of alignment and core-size statistics for each query relative to the reference, including:

- aligned bases;
- unaligned bases;
- variants;
- low-coverage bases.

A query with `0` in the `Aligned` field is not sufficiently represented in the core alignment and should be removed or analyzed with a more appropriate reference.

### `core.tab`

A tab-separated list of core SNP sites and alleles without annotations.

### `core.vcf`

A multi-sample VCF containing genotype (`GT`) fields for discovered alleles.

### Per-sequence folders

Each query folder contains files specific to that query-to-reference comparison, including a tab-separated summary.

### Optional tree outputs

When `cleanup_&_tree=True` is included, the output contains tree files including:

```text
{issue.id}.snippy.gubbins.iqtree.tree
{issue.id}.snippy_final.clean.core.snp.aln.tree
```

The first is an intermediate tree produced through the Gubbins/IQ-TREE workflow. The second is the final tree built with IQ-TREE from the cleaned core SNP alignment using the GTR substitution model.

Open tree files with a compatible viewer such as FigTree or a web-based Newick viewer.

A pre-cleaning tree may also be present and can retain samples removed during cleanup. This is particularly relevant when a mixed analysis contains Illumina paired-end data and MinION single-end data.

## Variant types

| Type | Meaning | Example |
|---|---|---|
| `snp` | Single-nucleotide polymorphism | `A -> T` |
| `mnp` | Multiple-nucleotide polymorphism | `GC -> AT` |
| `ins` | Insertion | `ATT -> AGTT` |
| `del` | Deletion | `ACGG -> ACG` |
| `complex` | Combination of SNP/MNP changes | `ATTC -> GTTA` |

Consult the Snippy output documentation for definitions of additional files and fields.

## How long does it take?

Small Snippy requests typically take approximately 15 minutes. Runtime depends on sample count, read coverage and size, input type, cleanup/tree generation, and service workload. Requests containing more than approximately 30 strains or large long-read files can take substantially longer.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that one or more query sequences cannot be found.

**Likely cause:** The requested raw-read files are unavailable.

**What to do:** Verify each `SEQID` and confirm that the required read data exist.

### A query is too divergent from the reference

**Symptom:** The analysis fails or `core.txt` reports `0` aligned bases for one or more samples.

**Likely cause:** The selected reference is too distantly related to the query.

**What to do:** Remove the divergent sample or choose a closer, high-quality reference and resubmit the request.

### Single-end samples disappear from the cleaned tree

**Symptom:** MinION single-end samples are present in the pre-cleaning tree but absent from the final cleaned tree.

**Likely cause:** The Gubbins cleanup workflow may remove those samples when paired-end Illumina and single-end MinION data are analyzed together.

**What to do:** Review the pre-cleaning tree, documented in the legacy workflow as `{issue.id}.snippy_core_alignment.iqtree.treefile`, and interpret the cleaned tree with this limitation in mind.

### An attached reference is missing or invalid

**Symptom:** The request cannot initialize the reference.

**Likely cause:** `reference=attached` was supplied without one valid FASTA attachment.

**What to do:** Attach a valid reference FASTA file and resubmit the request.

## Should I use Snippy or SNVPhyl?

Choose according to the workflow requirement:

- **Snippy** is generally faster and can combine paired-end and single-end raw-read data in one analysis.
- **SNVPhyl** currently requires paired-end R1 and R2 query reads and provides `snvMatrix.tsv`, `vcf2core.tsv`, and the SNVPhyl-specific Nextflow result set.

The documentation does not contain a local validation comparing biological results from the two workflows. Do not treat their outputs or filtering behavior as interchangeable without an appropriate comparison.

## Related automators

- [SNVPhyl](snvphyl.md) — provides pairwise SNV counts, core-genome statistics, alignments, and a Newick tree through the SNVPhyl pipeline.
- [COWSNPhR](cowsnphr.md) — calls variants with DeepVariant, annotates variant locations, and produces summary tables, alignments, and a tree.

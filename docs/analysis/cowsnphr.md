# COWSNPhR

## What does it do?

Use **COWSNPhR**—the CFIA OLC Workflow for Single Nucleotide Phylogeny Reporting—to identify high-quality variants between one reference genome and a group of closely related query strains and to create a phylogenetic tree.

The workflow:

1. annotates the reference with Prokka;
2. maps raw query reads to the reference with Bowtie 2;
3. calls variants with DeepVariant;
4. extracts and summarizes variants;
5. creates multiple-sequence alignments;
6. builds a phylogenetic tree with FastTree;
7. maps each variant to the reference annotation.

COWSNPhR was developed with USDA APHIS Veterinary Services. It requires queries that are closely related to the selected reference.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
cowsnphr
```

Spelling matters, but matching is not case-sensitive.

### Description

Use the following structure:

```text
reference
REFERENCE-SEQID
compare
QUERY-SEQID-1
QUERY-SEQID-2
```

Choose a high-quality reference assembly that is closely related to all queries.

### Use an attached reference

Attach one reference file and put `attached` on the line after `reference`:

```text
reference
attached
compare
QUERY-SEQID-1
QUERY-SEQID-2
```

### Optional parameters

The supplied documentation does not identify optional COWSNPhR parameters beyond choosing a reference by `SEQID` or attachment.

### Examples

See [issue 15681](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/15681) for an example using a reference `SEQID`.

See [issue 15682](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/15682) for an example using an uploaded reference file.

## Interpreting results

The COWSNPhR archive contains four main directories.

### `vcf_files`

Contains compressed global VCF files produced by variant calling.

### `summary_tables`

Contains tables summarizing variant location, prevalence among samples, and reference annotation.

### `alignments`

Contains multiple-sequence alignments of the detected variants.

### `tree_files`

Contains the phylogenetic tree generated from the alignment. Open the tree with a compatible viewer such as FigTree or another Newick tree viewer.

Interpret variant and tree results only when the query strains are sufficiently close to the reference and to one another. The tree represents relationships inferred from the workflow's selected variant positions and filtering; it should be interpreted with the alignment, variant summaries, and reference choice.

## How long does it take?

Most COWSNPhR requests take approximately one hour. Requests containing more than about 30 strains can take substantially longer.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that the reference or one or more query sequences cannot be found.

**Likely cause:** The required assembly or raw-read data are unavailable.

**What to do:** Verify all identifiers and confirm that the required files exist.

### Query strains are too distant from the reference

**Symptom:** The issue reports that one or more strains are not sufficiently related to the reference.

**Likely cause:** COWSNPhR requires close reference-query relationships for reliable mapping and variant comparison.

**What to do:** Remove divergent strains or select a closer high-quality reference and submit a new request.

### An uploaded reference is missing or invalid

**Symptom:** The workflow cannot initialize the reference.

**Likely cause:** `attached` was specified without a usable reference attachment.

**What to do:** Attach one valid reference file and resubmit the request.

### A novel reference-query combination fails

**Symptom:** The workflow reports an unexpected error despite apparently valid inputs.

**Likely cause:** COWSNPhR remains under active development and may encounter an untested organism or reference-query combination.

**What to do:** Preserve the issue error and partial outputs and consult the bioinformatics team.

## Related automators

- [SNVPhyl](snvphyl.md) — produces pairwise SNV counts, core-genome statistics, alignments, and a Newick tree from paired-end query reads.
- [Snippy](snippy.md) — provides rapid variant calling, core alignment, and optional Gubbins/IQ-TREE cleanup and tree generation.

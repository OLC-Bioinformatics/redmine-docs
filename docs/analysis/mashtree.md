# MashTree

## What does it do?

Use **MashTree** to build a rapid whole-genome tree from a list of sequence assemblies using Mash distances.

MashTree uses MinHash sketches to estimate pairwise genomic distances and construct a tree. It is useful for rapid exploratory comparison of many genomes, but the resulting tree is distance-based and is not built from a whole-genome alignment.

For background, see the [MashTree repository](https://github.com/lskatz/mashtree), the [MashTree algorithm documentation](https://github.com/lskatz/mashtree/blob/master/docs/ALGORITHM.md), and the Mash publications. Cite Katz et al. (2019), Ondov et al. (2016), and Ondov et al. (2019) when publishing results produced with this workflow.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
mashtree
```

Spelling matters, but matching is not case-sensitive.

### Description

The first line selects the analysis mode:

```text
analysis=ANALYSIS_NAME
```

Enter optional parameters next, followed by one `SEQID` per line.

### Supported analyses

#### `custom`

Compares only the assemblies listed in the Description:

```text
analysis=custom
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

#### `enterobacterales`

Compares the listed assemblies with a reference set representing species from order Enterobacterales.

```text
analysis=enterobacterales
2026-SEQ-0001
2026-SEQ-0002
```

#### `listeriaceae`

Compares the listed assemblies with a reference set representing species from family Listeriaceae.

```text
analysis=listeriaceae
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

The supplied documentation does not identify support for attached assemblies. Use available `SEQID`s unless attachment support has been verified in the current implementation.

### Optional parameters

#### `genomesize`

Sets the expected genome size.

- Default: `5000000`
- Example: `genomesize=3000000`

Choose a value appropriate for the organisms being compared.

#### `mindepth`

Sets the minimum k-mer depth. A value of `0` uses a smarter but slower method to discard lower-abundance k-mers.

- Default: `5`
- Example: `mindepth=10`

#### `kmerlength`

Sets the Mash k-mer size.

- Default: `21`
- Example: `kmerlength=30`

Larger k-mers generally provide more specificity, while smaller k-mers provide more sensitivity. Larger genomes can require larger k-mers to reduce chance sharing.

#### `sketch-size`

Sets the number of non-redundant MinHash values retained in each Mash sketch.

- Default: `10000`
- Example: `sketch-size=1000`

Larger sketches better represent each sequence but increase sketch size and comparison time.

### Example

```text
analysis=custom
genomesize=5000000
mindepth=5
kmerlength=21
sketch-size=10000
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

See [issue 26206](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/26206) for an example MashTree request.

## Interpreting results

When MashTree finishes, it uploads a Newick-formatted tree file to the Redmine issue.

Open the file in a Newick-compatible tree viewer. Branch lengths represent relationships derived from Mash distance estimates. Interpret them as rapid genome-distance comparisons rather than alignment-derived substitutions.

For `enterobacterales` and `listeriaceae`, the tree also includes the workflow's reference genomes. The nearest reference can help place a query within the represented collection, but a nearest tree position alone is not a definitive taxonomic assignment.

## How long does it take?

Runtime depends mainly on the number and size of assemblies, sketch parameters, and selected reference set. Small requests can finish in a few minutes; large sketches or many genomes take longer.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable assemblies.

**Likely cause:** MashTree cannot locate an assembly for the requested `SEQID`.

**What to do:** Verify each `SEQID` and confirm that its assembly is available.

### The selected analysis is unsupported or misspelled

**Symptom:** The automator cannot choose a comparison set.

**Likely cause:** `analysis` is missing or is not one of `custom`, `enterobacterales`, and `listeriaceae`.

**What to do:** Use a supported analysis value exactly as documented.

### Genomes are too dissimilar for a useful tree

**Symptom:** The tree contains very long branches or relationships that are difficult to interpret.

**Likely cause:** The included genomes are too diverse, the reference set is inappropriate, or sketch settings are unsuitable.

**What to do:** Compare a more coherent genome set, select a better analysis mode, and review `genomesize`, `kmerlength`, `mindepth`, and `sketch-size`.

## Related automators

- [bcgTree](bcgtree.md) — builds a bacterial phylogeny from essential single-copy core genes using a partitioned maximum-likelihood analysis.
- [NearTree](neartree.md) — ranks the closest strains to one query among a supplied comparison set.
- [StrainMash](strainmash.md) — identifies the closest represented RefSeq type strain for an assembly.

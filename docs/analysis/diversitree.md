# DiversiTree

## What does it do?

Use **DiversiTree** to build a tree from a supplied set of assemblies and select a requested number of strains that represent the greatest genetic diversity in that set.

By default, DiversiTree builds the tree with Parsnp. It can optionally use MashTree, which is generally faster and tolerates more diversity among samples but produces a distance-based tree rather than a core-genome alignment tree.

Use DiversiTree when the objective is to choose a diverse subset from a defined group. Use [NearTree](neartree.md) when the objective is to select strains closest to one query.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
DiversiTree
```

Spelling matters, but matching is not case-sensitive.

### Description

The first line is the number of diverse strains to select. Enter each assembly `SEQID` on a subsequent line.

Example selecting five diverse strains:

```text
5
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
2026-SEQ-0004
2026-SEQ-0005
2026-SEQ-0006
```

If the tree is needed but subset selection is not important, use `1` on the first line as documented by the existing workflow.

For the default Parsnp workflow, include fairly closely related strains—preferably from the same species—to reduce the risk of tree-construction failure.

### Attachments

The supplied documentation does not identify attachment support. Use available assembly `SEQID`s unless the current implementation has been verified to accept files.

### Optional parameters

#### `treeprogram`

Selects the tree-building program.

- Default: `parsnp`
- Alternative: `treeprogram=mashtree`

Place the option in the Description. The legacy page puts it at the end:

```text
5
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
treeprogram=mashtree
```

MashTree is generally faster and can handle more diverse genomes, but its tree is based on Mash distance estimates. Parsnp is more appropriate when a core-genome alignment tree among suitably related genomes is required.

### Examples

See [issue 12100](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/12100) for an example DiversiTree request.

See [issue 14724](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/14724) for an example using `treeprogram=mashtree`.

## Interpreting results

DiversiTree uploads:

```text
diversitree_report.html
tree.nwk
```

### `diversitree_report.html`

Open this report in a web browser. It displays the tree, highlights the selected strains, and lists the requested number of strains representing diversity within the supplied set.

### `tree.nwk`

Contains the complete tree in Newick format. Open it with a Newick-compatible tree viewer such as FigTree.

Interpret the selected subset relative to the genomes included in the request and the chosen tree program. The selected strains represent diversity within that supplied set; they are not necessarily the most diverse strains in the wider CFIA collection.

Parsnp and MashTree use different methods, so their branch lengths and selected subsets may differ. Record the selected `treeprogram` when results must be reproducible.

## How long does it take?

Runtime depends primarily on the number and relatedness of assemblies and the selected tree program. Requests with 10 or fewer strains can finish in a few minutes. Requests involving approximately 100 strains can take several hours.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that one or more assemblies cannot be found.

**Likely cause:** An identifier is incorrect or its assembly is unavailable.

**What to do:** Verify every `SEQID` and confirm that its assembly exists.

### The strains are too divergent for Parsnp

**Symptom:** DiversiTree warns that some strains are too distant or the default tree construction fails.

**Likely cause:** The supplied group is too diverse for a stable Parsnp core-genome analysis.

**What to do:** Remove divergent strains, analyze a more coherent taxonomic group, or use `treeprogram=mashtree` when a distance-based tree is suitable.

### The requested subset size is invalid

**Symptom:** The workflow cannot select the requested number of diverse strains.

**Likely cause:** The first line is not a positive integer or exceeds the number of valid assemblies.

**What to do:** Use a positive integer no greater than the number of available samples.

### The selected subset is overgeneralized

**Symptom:** The output is described as representing diversity beyond the supplied genome set.

**Likely cause:** DiversiTree selects only from the assemblies included in the request.

**What to do:** Define the candidate set deliberately and describe the result as diversity within that set.

## Related automators

- [NearTree](neartree.md) — selects strains closest to one query rather than a diverse subset.
- [MashTree](mashtree.md) — builds a complete Mash-distance tree without DiversiTree subset selection.
- [bcgTree](bcgtree.md) — builds a bacterial core-gene maximum-likelihood tree.

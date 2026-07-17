# NearTree

## What does it do?

Use **NearTree** to select a requested number of strains that are closest to one query strain within a user-supplied comparison set.

NearTree creates a distance-based tree for the query and reference `SEQID`s, then ranks the tree tips closest to the query. It returns an ordered list rather than a full phylogenetic interpretation.

Use NearTree when the goal is to choose close comparators from a defined set. Use MashTree or bcgTree when the goal is to inspect a complete tree.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
NearTree
```

Spelling matters, but matching is not case-sensitive.

### Description

The Description uses this exact order:

1. number of closest strains to return;
2. the line `query`;
3. one query `SEQID`;
4. the line `reference`;
5. all candidate comparison `SEQID`s, one per line.

Example requesting the five closest strains:

```text
5
query
2026-SEQ-0001
reference
2026-SEQ-0002
2026-SEQ-0003
2026-SEQ-0004
2026-SEQ-0005
2026-SEQ-0006
2026-SEQ-0007
```

The requested count should not exceed the number of valid reference candidates.

### Attachments

The supplied documentation does not identify attachment support. Use available `SEQID`s unless the current implementation has been verified to accept files.

### Optional parameters

The supplied documentation does not identify optional parameters beyond the number of closest strains on the first line.

### Example

See [issue 12940](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/12940) for an example NearTree request.

## Interpreting results

NearTree returns an ordered list of the comparison `SEQID`s closest to the query.

- the first result is the closest strain found in the supplied comparison set;
- each subsequent result is progressively more distant according to the workflow's tree-based distance ranking.

The result identifies relative closeness only among the candidates you supplied. It does not prove that the first result is the closest strain in the full database or population, and it does not provide a universal relatedness threshold.

If the workflow warns that candidates are too distant, reconsider the comparison set before using the ranking for downstream analysis.

## How long does it take?

Runtime depends primarily on the number of candidate strains. A set of 10 or fewer strains can finish in a few minutes, while a comparison involving approximately 100 strains can take several hours.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that the query or one or more reference candidates cannot be found.

**Likely cause:** An identifier is incorrect or its assembly is unavailable.

**What to do:** Verify every `SEQID` and confirm that the required assemblies exist.

### Candidate strains are too distant

**Symptom:** NearTree warns that some supplied strains are not sufficiently related.

**Likely cause:** The reference set spans organisms that are too diverse for a meaningful close-strain ranking.

**What to do:** Remove the flagged distant strains and submit a more coherent comparison set.

### The requested result count is invalid

**Symptom:** NearTree cannot return the requested number of closest strains.

**Likely cause:** The first line is not a valid positive integer or exceeds the number of valid reference candidates.

**What to do:** Use a positive integer no greater than the available reference-set size.

### The closest result is overinterpreted

**Symptom:** The first returned strain is treated as the globally closest available organism.

**Likely cause:** NearTree ranks only the candidate strains included in the request.

**What to do:** Expand or revise the candidate set when broader comparison is required.

## Related automators

- [MashTree](mashtree.md) — creates a complete tree from Mash distances for listed assemblies and optional reference sets.
- [bcgTree](bcgtree.md) — builds a maximum-likelihood bacterial tree from essential single-copy core genes.
- [StrainMash](strainmash.md) — compares one assembly with represented RefSeq type strains.

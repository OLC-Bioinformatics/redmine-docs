# CloseRelatives

## What does it do?

Use **CloseRelatives** to identify genomes in the CFIA sequence collection that are closest to one query genome.

CloseRelatives uses Mash distances to compare the query assembly with the collection and returns the requested number of nearest genomes. It also provides a complete CSV of distances when a broader review is needed.

Use CloseRelatives when the candidate comparison set should come from the CFIA collection. Use [NearTree](neartree.md) when you already have a specific candidate list and want to rank closeness within that list.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
Close Relatives
```

Spelling matters, but matching is not case-sensitive.

### Description

The Description must contain exactly two components:

1. the number of closest genomes to return;
2. the query `SEQID`.

Example requesting the five closest genomes:

```text
5
2014-SEQ-0276
```

The legacy page's prose said “find the 5 closest genomes” but showed `10` in its example. The replacement uses `5` so the requested count and example agree.

### Attachments

The supplied documentation does not identify attachment support. Use an available query `SEQID` unless the current implementation has been verified to accept an assembly attachment.

### Optional parameters

The supplied documentation does not identify optional parameters beyond the requested number of close relatives on the first line.

### Example

See [issue 14676](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/14676) for an example CloseRelatives request.

## Interpreting results

CloseRelatives reports the requested number of closest strains and their Mash distances, ordered from closest to more distant.

It also uploads:

```text
close_relatives_results.csv
```

This file contains Mash distances between the query and every genome evaluated in the CFIA collection.

Mash distance interpretation:

- `0` indicates identical or nearly identical sequence content under the Mash comparison;
- larger values indicate increasing dissimilarity;
- values approaching `1` indicate little detectable similarity.

A small Mash distance indicates genomic similarity, but it is not by itself a species identification, epidemiological linkage, or phylogenetic conclusion. Interpret results with reference coverage, sequence quality, and the intended downstream analysis.

## How long does it take?

CloseRelatives generally finishes in approximately two to three minutes. Runtime can vary with collection size and service workload.

## What can go wrong?

### The query `SEQID` is unavailable

**Symptom:** The Redmine issue reports that the query sequence cannot be found.

**Likely cause:** The identifier is incorrect or its assembly is unavailable.

**What to do:** Verify the query `SEQID` and confirm that its assembly exists.

### The requested count is missing or invalid

**Symptom:** The issue asks for a valid number of strains.

**Likely cause:** The first Description line is absent or is not a positive integer.

**What to do:** Put the desired number of closest genomes on the first line and the query `SEQID` on the second line.

### The closest match is overinterpreted

**Symptom:** A low Mash distance is treated as definitive biological identity or relatedness.

**Likely cause:** Mash distance is being used without supporting taxonomic, phylogenetic, or epidemiological evidence.

**What to do:** Use the result to select candidates for an appropriate follow-up workflow, such as Unknown Isolate, SNVPhyl, Snippy, or another reviewed comparison.

## Related automators

- [NearTree](neartree.md) — ranks close strains within a user-supplied candidate set.
- [StrainMash](strainmash.md) — compares an assembly with represented RefSeq type strains.
- [Unknown Isolate](unknownisolate.md) — combines rMLST, Mash, ANIb, and ANIm evidence for uncertain isolate identification.

# StrainMash

## What does it do?

Use **StrainMash** to identify the RefSeq type strain most similar to a draft genome assembly.

StrainMash uses the MinHash algorithm implemented by [Mash](https://github.com/marbl/mash) to compare an assembly quickly with reference type strains. It is useful when the expected species of an assembled isolate is uncertain.

StrainMash identifies the closest reference in its database; it does not by itself provide the multi-method rMLST and ANI evidence produced by [Unknown Isolate](unknownisolate.md), and it is not a mixed-community profiler.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
StrainMash
```

Spelling matters, but matching is not case-sensitive.

### Description

In the **Description** field, enter one assembly `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

No attachment is required. StrainMash retrieves the draft assembly associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine StrainMash automator.

### Example

See [issue 12860](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/12860) for an example StrainMash request.

## Interpreting results

When StrainMash finishes, it uploads a ZIP archive containing an `output` directory. The directory contains one text report per requested `SEQID`.

Each report contains six columns.

### `MashDistance`

The existing automator documentation describes this value as a closeness score for the query and reference and recommends at least `0.98` for a very close match.

Standard Mash distance convention normally uses smaller values for closer sequences, so the reported field may be a transformed similarity value or may be mislabeled in the legacy documentation. Confirm the current output semantics before using the `0.98` threshold in a final identification decision.

### `NumMatchingHashes`

Reports the number of matching hashes. The documented range uses `1000/1000` as a perfect match and `0` as no similarity. The legacy guidance recommends more than `850` matching hashes for a close reference match.

### `MedianMultiplicity`

The legacy documentation states that this value can usually be ignored and should normally be `1`.

### `Pvalue`

Reports the statistical significance assigned to the hit. The legacy guidance expects `0` for a very close reference match.

### `ReferenceStrain`

Reports the NCBI accession of the reference strain.

### `Organism`

Reports the organism name associated with the reference strain.

Interpret the closest match using the score semantics, matching-hash support, reference coverage, and organism context. A closest database match is not necessarily a definitive species identification when reference representation is limited or the assembly is contaminated, incomplete, or divergent.

## How long does it take?

StrainMash generally takes approximately 30 seconds to one minute per sample. Total runtime depends on sample count and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** StrainMash cannot locate a draft assembly for the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its assembly is available, and submit a corrected request.

### The closest match has weak support

**Symptom:** The top reference result has relatively few matching hashes or an uncertain similarity score.

**Likely cause:** The query may be distant from represented type strains, poorly assembled, contaminated, or absent from the reference collection.

**What to do:** Review assembly quality and contamination, then use [Unknown Isolate](unknownisolate.md) for broader rMLST, MASH, ANIb, and ANIm evidence.

### `MashDistance` appears inconsistent with Mash conventions

**Symptom:** The reported value increases with similarity even though standard Mash distance normally decreases as sequences become more similar.

**Likely cause:** The automator may report a transformed similarity score under the `MashDistance` heading, or the legacy description may be incorrect.

**What to do:** Confirm the current output calculation or inspect a known reference match before interpreting this field quantitatively.

## Related automators

- [Unknown Isolate](unknownisolate.md) — combines rMLST, MASH, ANIb, and ANIm evidence for uncertain isolate identification.
- [AutoCLARK](autoclark.md) — reports species represented in raw reads or draft assemblies.
- [Kraken2/Bracken](kraken2.md) and [MetaPhlAn4](metaphlan.md) — profile taxonomic composition in metagenomic data.

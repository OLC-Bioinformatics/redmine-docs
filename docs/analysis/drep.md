# dRep

## What does it do?

Use **dRep** to compare genome assemblies by average nucleotide identity (ANI) or to dereplicate a genome set.

dRep first performs Mash-based primary clustering. It then applies a selected secondary comparison algorithm only within genome clusters meeting the primary Mash ANI threshold. This two-stage design avoids expensive ANI comparisons between clearly unrelated genomes.

The `compare` command produces relatedness and ANI outputs. The `dereplicate` command groups similar genomes and selects representatives according to dRep's workflow.

For background, see the [dRep documentation](https://drep.readthedocs.io/en/latest/overview.html) and [Olm et al. (2017)](https://www.nature.com/articles/ismej2017126). Cite the dRep authors when publishing results produced with this automator.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
drep
```

Spelling matters, but matching is not case-sensitive.

### Description

The first line selects the analysis mode:

```text
analysis=custom
```

Enter optional parameters next, followed by one assembly `SEQID` per line:

```text
analysis=custom
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

Only `custom` is documented as currently supported. It compares or dereplicates only the listed genomes.

### Attachments

The supplied documentation does not identify attachment support. Use available assembly `SEQID`s unless the current implementation has been verified to accept files.

### Optional parameters

#### `mash_ANI_threshold`

Sets the primary Mash ANI clustering threshold. Only genomes clustering at or above this threshold proceed to secondary ANI comparison.

- Default: `0.9` (90%)
- Example: `mash_ANI_threshold=0.8`

Lowering this threshold can create broader clusters and substantially increase secondary-comparison runtime.

#### `S_ANI`

Sets the secondary clustering ANI threshold.

- Default: `0.99` (99%)
- Example: `S_ANI=0.9`

#### `coverage_threshold`

Sets the minimum overlap required between genomes during secondary comparisons.

- Default: `0.1` (10%)
- Example: `coverage_threshold=0.2`

#### `clusteralgorithm`

Selects the hierarchical clustering linkage method.

- Default: `average`
- Example: `clusteralgorithm=ward`
- Supported values:
  - `median`
  - `weighted`
  - `single`
  - `complete`
  - `average`
  - `ward`
  - `centroid`

#### `comparisonalgorithm`

Selects the secondary ANI comparison algorithm.

- Default: `ANImf`
- Example: `comparisonalgorithm=fastANI`
- Supported values:
  - `fastANI`
  - `ANImf`
  - `ANIn`
  - `gANI`
  - `goANI`

#### `command`

Selects comparison or dereplication behavior.

- Default: `compare`
- Compare genomes: `command=compare`
- Dereplicate genomes: `command=dereplicate`

### Examples

#### Compare genomes with default thresholds

```text
analysis=custom
command=compare
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

#### Compare a broader primary cluster with FastANI

```text
analysis=custom
mash_ANI_threshold=0.8
comparisonalgorithm=fastANI
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

#### Dereplicate a genome set

```text
analysis=custom
command=dereplicate
S_ANI=0.99
coverage_threshold=0.1
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

The supplied documentation does not provide a working Redmine issue link for dRep.

## Interpreting results

When dRep finishes, it uploads a ZIP archive to Dropbox. The archive contains dRep tables, figures, clustering data, and other workflow outputs.

ANI values are calculated only for genomes placed in primary clusters meeting `mash_ANI_threshold`. If two genomes do not have a secondary ANI value, they may have failed the primary threshold rather than failed processing.

dRep does not directly output a tree file in this workflow. A Mash-based dendrogram can be reproduced by applying hierarchical clustering to the Mash-distance table with the selected `clusteralgorithm`, but that reconstruction is not equivalent to receiving a maintained tree artifact from the automator.

For `command=dereplicate`, review the clustering thresholds, coverage requirement, and selected representative genomes. Dereplication results depend directly on these settings and should not be interpreted without recording them.

## How long does it take?

Runtime depends on sample count, genome relatedness, thresholds, and the selected secondary algorithm.

A set of 10 or fewer genomes can finish in a few minutes. Larger sets can take several hours. Closely related genomes can also take longer because more pairs advance from Mash primary clustering to the slower secondary ANI comparison.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that one or more assemblies cannot be found.

**Likely cause:** An identifier is incorrect or its assembly is unavailable.

**What to do:** Verify every `SEQID` and confirm that its assembly exists.

### The analysis type is missing or unsupported

**Symptom:** The automator rejects the request before dRep starts.

**Likely cause:** `analysis=custom` is absent or another analysis type was requested.

**What to do:** Put `analysis=custom` on the first line.

### Expected ANI values are absent

**Symptom:** Some genome pairs have no secondary ANI result.

**Likely cause:** They did not cluster together at or above `mash_ANI_threshold` or did not meet the coverage requirement.

**What to do:** Review primary clustering and overlap before changing thresholds. Lower thresholds only when methodologically justified.

### The comparison or clustering algorithm is unsupported

**Symptom:** The issue reports an invalid `comparisonalgorithm` or `clusteralgorithm`.

**Likely cause:** The value is misspelled or not in the supported list.

**What to do:** Copy a documented value exactly.

### Dereplication removes more genomes than expected

**Symptom:** The representative set is smaller or grouped differently than anticipated.

**Likely cause:** `S_ANI`, `coverage_threshold`, primary clustering, or algorithm settings are more permissive or restrictive than intended.

**What to do:** Review all thresholds and clustering outputs before accepting the representative set.

## Related automators

- [DiversiTree](diversitree.md) — selects strains representing diversity within a supplied set rather than dereplicating by ANI.
- [CloseRelatives](closerelatives.md) — finds genomes in the CFIA collection closest to one query by Mash distance.
- [MashTree](mashtree.md) — builds a Mash-distance tree for a supplied genome set.
- [Unknown Isolate](unknownisolate.md) — uses dRep compare as part of a multi-method organism-identification workflow.

# PointFinder

## What does it do?

Use **PointFinder** to detect known chromosomal point mutations associated with antibiotic resistance in draft genome assemblies.

PointFinder complements [ResFinder](resfinder.md): ResFinder detects acquired resistance genes, while PointFinder detects supported resistance-associated mutations.

In this Redmine workflow, PointFinder supports:

- *Campylobacter*;
- *Escherichia*;
- *Mycobacterium*;
- *Neisseria*;
- *Salmonella*.

The automator determines the genus of each requested sequence and analyzes only sequences belonging to a supported genus.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
PointFinder
```

Spelling matters, but matching is not case-sensitive.

### Description

In the **Description** field, enter one `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

Each requested `SEQID` must have a draft genome assembly available. The automator determines the genus before running PointFinder.

### Attachments

No attachment is required. PointFinder retrieves the assemblies associated with the requested `SEQID`s.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine PointFinder automator.

### Example

```text
2026-SEQ-0001
2026-SEQ-0002
```

See [issue 12933](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/12933) for an example PointFinder request.

## Interpreting results

When PointFinder finishes, it uploads:

```text
pointfinder_output.zip
```

For each `SEQID` with detected resistance, the archive contains three files.

### `SEQID_blastn_PointFinder_prediction.txt`

This file lists antibiotics with known mutations for the identified genus.

- `0` means that no relevant mutation was detected for that antibiotic.
- A nonzero value indicates one or more predicted point mutations for that antibiotic.

### `SEQID_blastn_PointFinder_results.txt`

This file lists:

- genes containing detected mutations;
- the identified mutations; and
- the resistance associated with those mutations.

### `SEQID_blastn_PointFinder_table.txt`

This file contains information similar to the results file and also lists known AMR genes for which no point mutation was detected.

A requested `SEQID` with no detected resistance does not receive these files. The absence of files can therefore indicate that PointFinder detected no supported resistance mutation, provided the sequence was available and belonged to a supported genus.

## How long does it take?

PointFinder generally takes approximately 30 seconds to one minute per analyzed sample. Total runtime depends on the number of samples and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** PointFinder cannot locate a draft genome assembly for the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its assembly is available, and submit a corrected request.

### A requested sequence belongs to an unsupported genus

**Symptom:** The Redmine issue reports that PointFinder cannot analyze one or more sequences.

**Likely cause:** The automator identified the sequence as a genus outside the supported list.

**What to do:** Use another AMR workflow appropriate for the organism. Supported sequences in the same request can still be analyzed.

### No result files are uploaded for a supported sequence

**Symptom:** An available sequence from a supported genus has no PointFinder result files.

**Likely cause:** PointFinder detected no supported resistance mutation for that sequence.

**What to do:** Confirm that the sequence was accepted and classified into a supported genus, then interpret the missing files as no supported mutation detected rather than as an acquired-gene result.

## Related automators

- [ResFinder](resfinder.md) — detects acquired AMR genes but not chromosomal point mutations.
- [StarAMR](staramr.md) — combines ResFinder and supported PointFinder analyses for *Campylobacter* and *Salmonella* assemblies.
- [CARD-RGI](cardrgi.md) — predicts resistomes in isolate assemblies or raw FASTQ data and may report organism-dependent mutation or efflux evidence.
- [AMRsummary](amrsummary.md) — combines acquired-gene detection with predicted plasmid location, but does not add PointFinder mutation detection.

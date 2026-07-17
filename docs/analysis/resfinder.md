# ResFinder

## What does it do?

Use **ResFinder** to detect acquired antibiotic-resistance genes in draft genome assemblies.

The Redmine ResFinder automator detects acquired resistance genes, which are often plasmid-borne. It does **not** detect resistance caused by chromosomal point mutations. For supported mutation detection, use [PointFinder](pointfinder.md), [StarAMR](staramr.md), or [CARD-RGI](cardrgi.md), depending on the organism, input type, and desired analysis.

ResFinder was developed by the Danish Center for Genomic Epidemiology.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
ResFinder
```

Spelling matters, but matching is not case-sensitive.

### Description

In the **Description** field, enter one `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

The requested `SEQID`s must have draft genome assemblies available.

### Attachments

No attachment is required. ResFinder retrieves the draft assemblies associated with the requested `SEQID`s.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine ResFinder automator.

### Example

```text
2026-SEQ-0001
2026-SEQ-0002
```

See [issue 12854](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/12854) for an example ResFinder request.

## Interpreting results

When ResFinder finishes, it uploads:

```text
resfinder.xlsx
```

The workbook lists acquired AMR genes detected in each sample. A listed gene or resistance does not by itself establish that the strain expresses the associated resistance phenotype.

Review at least:

- `PercentIdentity` — sequence identity between the detected gene and reference target;
- `PercentCovered` — coverage of the reference target.

A hit with `100` for both identity and coverage provides stronger evidence that the complete acquired gene is present. Hits with lower identity or coverage require further review.

ResFinder does not report chromosomal point-mutation resistance in this Redmine workflow.

## How long does it take?

ResFinder is generally fast and should take only a few seconds per requested `SEQID`. Total runtime also depends on the number of samples and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** ResFinder cannot locate a draft genome assembly for the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its assembly is available, and submit a corrected request.

### Point-mutation resistance is missing from the report

**Symptom:** The ResFinder result does not include an expected chromosomal resistance mutation.

**Likely cause:** The Redmine ResFinder automator only detects acquired resistance genes.

**What to do:** Use [PointFinder](pointfinder.md), [StarAMR](staramr.md), or [CARD-RGI](cardrgi.md) when mutation-based resistance must also be assessed.

### A lower-identity or partially covered hit is difficult to interpret

**Symptom:** A result has less than `100` in `PercentIdentity`, `PercentCovered`, or both.

**Likely cause:** The detected sequence differs from or only partially covers the reference target.

**What to do:** Review the alignment evidence and organism context before concluding that the complete resistance gene is present.

## Related automators

- [PointFinder](pointfinder.md) — detects supported chromosomal mutations associated with antimicrobial resistance.
- [StarAMR](staramr.md) — combines ResFinder acquired-gene detection with PointFinder mutation detection for supported *Campylobacter* and *Salmonella* assemblies.
- [CARD-RGI](cardrgi.md) — predicts resistomes in isolate assemblies or raw FASTQ data and can include strict, perfect, loose, and partial CARD hits.
- [GeneSeekr](geneseekr.md) — provides `analysis=resfinder` for FASTA-formatted inputs.
- [Sipprverse](sipprverse.md) — provides `analysis=resfinder` for raw, paired-end FASTQ reads.

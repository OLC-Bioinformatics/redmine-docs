# StarAMR

## What does it do?

Use **StarAMR** to detect acquired antibiotic-resistance genes and supported resistance-associated point mutations in draft genome assemblies.

StarAMR combines ResFinder gene detection with PointFinder mutation detection. In this Redmine workflow, mutation analysis is limited to:

- *Campylobacter*;
- *Salmonella*.

The automator determines the genus of each requested sequence and analyzes only sequences belonging to a supported genus. Sequences from other genera do not receive StarAMR result files.

StarAMR was developed by Canada's National Microbiology Laboratory and uses ResFinder and PointFinder resources from the Danish Center for Genomic Epidemiology.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
staramr
```

Spelling matters, but matching is not case-sensitive.

### Description

In the **Description** field, enter one `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

The requested `SEQID`s must have draft genome assemblies available. The automator determines each sequence's genus before running the supported analysis.

### Attachments

No attachment is required. StarAMR retrieves the assemblies associated with the requested `SEQID`s.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine StarAMR automator.

### Example

```text
2026-SEQ-0001
2026-SEQ-0002
```

See [issue 15625](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/15625) for an example StarAMR request.

## Interpreting results

When StarAMR finishes, it uploads:

```text
staramr_output.zip
```

The archive contains genus-specific files. The most important file for each supported genus is:

```text
results.xlsx
```

`results.xlsx` reports AMR findings for each analyzed `SEQID` and indicates whether the evidence is an acquired gene or a point mutation.

Sequences that are not identified as *Campylobacter* or *Salmonella* do not receive StarAMR result files in this Redmine workflow.

## How long does it take?

StarAMR generally takes approximately 30 seconds to one minute per analyzed sample. Total runtime depends on the number of samples and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** StarAMR cannot locate a draft genome assembly for the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its assembly is available, and submit a corrected request.

### A requested sequence belongs to an unsupported genus

**Symptom:** The Redmine issue reports that StarAMR cannot analyze one or more requested sequences, and those sequences have no result files.

**Likely cause:** The automator identified the sequence as a genus other than *Campylobacter* or *Salmonella*.

**What to do:** Use a different AMR automator appropriate for the organism and desired evidence. Supported sequences in the same request can still be analyzed.

### Gene and mutation evidence are confused

**Symptom:** A resistance result is interpreted without distinguishing an acquired gene from a point mutation.

**Likely cause:** The result type in `results.xlsx` was not considered.

**What to do:** Review whether each result is gene-based or mutation-based and interpret it in the relevant organism context.

## Related automators

- [ResFinder](resfinder.md) — detects acquired AMR genes in draft genome assemblies but does not report chromosomal point mutations.
- [PointFinder](pointfinder.md) — focuses on supported chromosomal resistance mutations.
- [CARD-RGI](cardrgi.md) — predicts resistomes in isolate assemblies or raw FASTQ data and supports broader CARD match categories.

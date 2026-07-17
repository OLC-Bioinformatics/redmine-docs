# PHASTEST

## What does it do?

Use **PHASTEST** to identify, annotate, and visualize prophage regions in bacterial genomes and plasmids.

A prophage is phage or bacteriophage sequence integrated into a bacterial genome or plasmid. PHASTEST can also annotate and visualize other genomic features, including protein-coding regions, tRNA genes, and rRNA genes.

Use `phage-only` when only prophage-region annotation is required. Choose `deep` mode when the additional databases used by that mode are needed and the longer runtime is acceptable.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
phastest
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter optional mode lines first, followed by one assembly `SEQID` per line.

Minimal request using the default lite annotation mode:

```text
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

No attachment is required. PHASTEST retrieves the sequence assembly associated with each requested `SEQID`.

### Optional parameters

#### Annotation mode

PHASTEST supports two documented annotation modes:

- `lite` — uses the Prophage Database and Swiss-Prot;
- `deep` — uses the Prophage Database and PHAST-BSD Bacterial Database.

The default is `lite`.

Enable deep mode by adding this line:

```text
deep
```

Deep mode can take significantly longer.

#### `phage-only`

Restricts annotation to phage regions.

- Default: disabled
- Enable with:

```text
phage-only
```

### Examples

#### Lite mode

```text
2026-SEQ-0001
2026-SEQ-0002
```

#### Deep, phage-only analysis

```text
deep
phage-only
2026-SEQ-0001
```

See [issue 35681](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/35681) for a historical PHASTEST request. Its output predates the current issue-number naming convention.

## Interpreting results

When PHASTEST finishes, it uploads:

```text
phastest_output_<issue number>.zip
```

The archive contains all outputs generated for the request.

Review predicted prophage regions in the context of their completeness, genomic coordinates, supporting annotations, and the assembly. Prophage predictions are computational results and can be affected by fragmented contigs or incomplete assemblies.

When full annotation is enabled, distinguish prophage-region output from the additional protein-coding, tRNA, and rRNA annotations. When `phage-only` is used, expect the output to focus on prophage regions.

## How long does it take?

A PHASTEST analysis takes approximately one hour per sample. The automator processes samples concurrently, with up to 20 CPUs allocated.

Approximate batch behavior from the documented configuration:

- 1–20 `SEQID`s: approximately one hour;
- 21–40 `SEQID`s: approximately two hours;
- 41–60 `SEQID`s: approximately three hours.

Deep mode may take significantly longer. Runtime can also vary with input size and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that one or more requested sequences cannot be found.

**Likely cause:** The identifier is incorrect or its assembly is unavailable.

**What to do:** Verify every `SEQID` and confirm that the required assembly exists.

Available sequences can still be analyzed when some requested identifiers are missing.

### Deep mode takes longer than expected

**Symptom:** The request remains active longer than the normal lite-mode estimate.

**Likely cause:** Deep mode uses additional database resources and can take significantly longer.

**What to do:** Allow additional time. Use the default lite mode for future requests when deep annotation is unnecessary.

### A prophage region is incomplete or fragmented

**Symptom:** A predicted region spans a contig boundary or lacks expected phage features.

**Likely cause:** The draft assembly is fragmented or does not contain the complete integrated phage sequence.

**What to do:** Review assembly quality, region coordinates, and supporting annotations before interpreting completeness.

## Related automators

- [MobSuite](mobsuite.md) — reconstructs and types predicted plasmids in draft genome assemblies.
- [Prokka](prokka.md) — performs general genome annotation without PHASTEST's prophage-focused detection and visualization.

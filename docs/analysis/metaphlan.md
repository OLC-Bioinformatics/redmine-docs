# MetaPhlAn4

## What does it do?

Use **MetaPhlAn4** to profile the taxonomic composition of microbial communities from metagenomic shotgun sequence data.

The automator supports:

- draft assemblies under `fastas`;
- short-read FASTQ data under `fastqs`;
- Nanopore FASTQ data under `nanoporefastqs`.

A single request can include more than one input type. If both an assembly and its raw reads must be analyzed, list the same `SEQID` under each applicable input-type heading.

For background, see the [MetaPhlAn website](https://huttenhower.sph.harvard.edu/metaphlan/), the [MetaPhlAn 4 tutorial](https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4), and Blanco-Míguez et al. (2023). Cite the MetaPhlAn authors when publishing results produced with this automator.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
metaphlan
```

Spelling matters, but matching is not case-sensitive.

### Description

Group each `SEQID` under one of these input-type headings:

```text
fastqs
fastas
nanoporefastqs
```

Example containing short reads and an assembly:

```text
fastqs
2023-SEQ-0415
2023-SEQ-0414
fastas
2025-MIN-0333
```

To analyze both the assembly and raw data for the same sample, repeat its `SEQID` under both applicable headings.

### Attachments

No attachment is required. The automator retrieves the requested assemblies or read files according to the input-type headings.

### Optional parameters

#### `analysis`

Selects the MetaPhlAn output mode.

- Default: `rel_ab_w_read_stats`
- Supported values:
  - `analysis=rel_ab_w_read_stats` — relative abundances plus estimated reads from each clade;
  - `analysis=rel_ab` — relative-abundance profile;
  - `analysis=clade_profiles` — normalized marker counts for clades with at least one non-null marker;
  - `analysis=marker_ab_table` — normalized marker counts;
  - `analysis=marker_pres_table` — list of markers present in the sample.

### Examples

#### Relative abundance with read statistics

```text
analysis=rel_ab_w_read_stats
fastqs
2026-SEQ-0001
2026-SEQ-0002
```

#### Compare raw reads and an assembly for one sample

```text
analysis=rel_ab
fastqs
2026-SEQ-0001
fastas
2026-SEQ-0001
```

#### Nanopore reads

```text
nanoporefastqs
2026-MIN-0001
```

See [issue 37794](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/37794) for an example MetaPhlAn4 request. Temporary Dropbox links associated with the issue may expire.

## Interpreting results

When MetaPhlAn4 finishes, it uploads an archive named using the Redmine issue identifier:

```text
metaphlan4_output_redmineID.zip
```

The archive contains MetaPhlAn4 report files. The source documentation also states that it contains Bracken analysis reports; because Bracken is normally associated with Kraken classification, confirm the current archive contents before final publication.

Interpret the output according to the selected `analysis` mode:

- `rel_ab_w_read_stats` reports relative abundance and estimated read counts per clade;
- `rel_ab` reports relative abundance;
- `clade_profiles` reports normalized marker counts for detected clades;
- `marker_ab_table` reports normalized marker abundance;
- `marker_pres_table` reports marker presence.

Results can differ between raw reads and assemblies because assembly may discard, combine, or fail to reconstruct some community sequences. Treat repeated analysis of one `SEQID` under different input headings as separate evidence rather than interchangeable output.

## How long does it take?

Runtime depends on input type, selected analysis mode, amount of sequence data, and number of requested samples.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** The automator cannot locate data matching the input-type heading used for the `SEQID`.

**What to do:** Verify the identifier and place it under `fastqs`, `fastas`, or `nanoporefastqs` according to the available data.

### A `SEQID` is listed under the wrong input type

**Symptom:** The automator cannot retrieve or process the requested sequence data.

**Likely cause:** An assembly was listed under a raw-read heading, or raw reads were listed under `fastas`.

**What to do:** Move the `SEQID` beneath the correct input-type heading and resubmit the request.

### Only one representation of a sample is analyzed

**Symptom:** The output contains raw-read results but no assembly result, or the reverse.

**Likely cause:** The `SEQID` was listed under only one input-type heading.

**What to do:** List the same `SEQID` under every representation that must be analyzed.

### An unsupported analysis mode is requested

**Symptom:** The automator rejects the analysis or produces no expected report.

**Likely cause:** The `analysis` value is misspelled or unsupported.

**What to do:** Use one of the documented analysis values exactly as shown.

## Related automators

- [Kraken2/Bracken](kraken2.md) — provides k-mer-based taxonomic classification and refined abundance estimation with selectable databases.
- [AutoCLARK](autoclark.md) — reports species represented in raw reads or draft assemblies using CLARK.
- [StrainMash](strainmash.md) — identifies the closest RefSeq type strain for an assembly rather than profiling a mixed community.

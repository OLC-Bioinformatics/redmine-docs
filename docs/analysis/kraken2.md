# Kraken2/Bracken

## What does it do?

Use **Kraken2/Bracken** to classify sequence data taxonomically and estimate the organisms represented in a sample.

The workflow runs:

- **Kraken2** — assigns sequencing reads or assembly sequences to taxonomic groups using a selected reference database;
- **Bracken** — refines abundance estimates from Kraken2 classifications.

The automator supports paired-end raw reads, Nanopore reads, and assemblies. Database choice is important because the standard Kraken2 and PlusPF databases are intended for metagenomic analysis, while the Greengenes, RDP, and SILVA options are 16S-oriented databases.

For background, see the [Kraken2 repository](https://github.com/DerrickWood/kraken2), the [Kraken2 protocol](https://www.nature.com/articles/s41596-022-00738-y), Wood and Salzberg (2014), and Wood et al. (2019). Cite the appropriate tool authors when publishing results produced with this automator.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
kraken2
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter optional settings first, followed by one `SEQID` per line.

Minimal request using paired-end raw reads and the standard database:

```text
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

No attachment is required. The automator retrieves the sequence data associated with each requested `SEQID` according to `seqtype`.

### Optional parameters

#### `seqtype`

Selects the input data type.

- Default: `paired`
- Paired-end raw reads: `seqtype=paired`
- Nanopore reads: `seqtype=nanopore`
- Assembly data: `seqtype=assembly`

#### `database`

Selects the taxonomic reference database.

- Default: `kraken2`
- Supported values:
  - `database=kraken2`
  - `database=plusPF`
  - `database=greengenes`
  - `database=rdp`
  - `database=silva`

The default `kraken2` database is documented as the standard database dated September 26, 2022, obtained from the [Kraken2 Index Zone](https://benlangmead.github.io/aws-indexes/k2).

Use `kraken2` or `plusPF` for metagenomic classification. The `greengenes`, `rdp`, and `silva` options are 16S-based databases and should be selected only when appropriate for the input and analysis goal.

### Examples

#### Paired-end metagenomic reads

```text
database=kraken2
2026-SEQ-0001
2026-SEQ-0002
```

#### Nanopore metagenomic reads with PlusPF

```text
seqtype=nanopore
database=plusPF
2026-MIN-0001
```

#### Assembly classification

```text
seqtype=assembly
database=kraken2
2026-SEQ-0001
```

See [issue 29355](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/29355) for an example Kraken2 request. Temporary result-download links associated with the issue may expire.

## Interpreting results

When Kraken2/Bracken finishes, it uploads an archive named using the Redmine issue identifier:

```text
kraken2_output_redmineID.zip
```

The archive contains Kraken2 and Bracken report files.

Use Kraken2 output to review taxonomic classifications and Bracken output to review refined abundance estimates. Interpret low-abundance or closely related taxa cautiously because classification accuracy depends on sequence quality, database composition, and similarity among represented organisms.

Results produced with a 16S-oriented database are not directly equivalent to results produced with a metagenomic database. Record the selected database when reporting or comparing results.

## How long does it take?

Runtime depends on the selected input type, database, amount of sequence data, and number of requested samples. Kraken2 is faster than the original Kraken workflow, but large requests can require substantial memory.

Limit metagenomic requests to approximately 10 samples at a time to reduce the risk of memory exhaustion.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** The automator cannot locate the data required for the requested `SEQID` and `seqtype`.

**What to do:** Verify each `SEQID`, confirm that the selected input type exists, and submit a corrected request.

### The request runs out of memory

**Symptom:** The job fails while processing a large metagenomic batch.

**Likely cause:** Too many samples or a memory-intensive database was requested at once.

**What to do:** Split the request into batches of no more than approximately 10 metagenomes and resubmit them separately.

### The selected database is inappropriate

**Symptom:** Results are sparse, misleading, or difficult to compare with prior analyses.

**Likely cause:** A 16S-oriented database was selected for metagenomic data, or a metagenomic database was selected for an analysis intended to use a 16S reference collection.

**What to do:** Choose `kraken2` or `plusPF` for metagenomic classification and use `greengenes`, `rdp`, or `silva` only for an appropriate 16S-based analysis.

### The selected input type is incorrect

**Symptom:** The automator cannot locate or process the expected sequence files.

**Likely cause:** `seqtype` does not match the available paired-end reads, Nanopore reads, or assembly data.

**What to do:** Correct `seqtype` and resubmit the request.

## Related automators

- [MetaPhlAn4](metaphlan.md) — profiles microbial communities using marker genes and supports raw reads and assemblies.
- [AutoCLARK](autoclark.md) — reports species represented in raw reads or draft assemblies using CLARK.
- [StrainMash](strainmash.md) — compares an assembly with RefSeq type strains to identify its closest reference.

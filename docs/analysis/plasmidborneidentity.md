# Plasmid-Borne Identity

![Plasmid-Borne Identity workflow](../img/plasmid_borne_identity.png)

## What does it do?

Use **Plasmid-Borne Identity** to search FASTA-formatted draft genome assemblies for user-supplied gene targets and predict whether each detected target is located on a plasmid or chromosome.

Plasmid-Borne Identity combines:

- **GeneSeekr** — searches assemblies for the attached target sequences;
- **MOB-suite `mob_recon`** — reconstructs and types predicted plasmids.

The combined report maps target-containing contigs to MOB-suite plasmid predictions. For additional MOB-suite output details, see the [MOB-suite repository](https://github.com/phac-nml/mob-suite).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
plasmid_borne_identity
```

Spelling matters, but matching is not case-sensitive.

### Description

In the **Description** field, enter one `SEQID` per line. Optional GeneSeekr parameters can be placed on separate lines before the `SEQID`s.

```text
cutoff=80
2026-SEQ-0001
2026-SEQ-0002
```

Each requested `SEQID` must have a FASTA-formatted draft genome assembly available.

### Attachments

Attach a FASTA-formatted file containing the gene targets to search for. The supplied documentation does not specify a required attachment filename or a Description parameter that refers to that filename.

### Optional parameters

#### `blast`

Selects the BLAST program used by GeneSeekr.

- Default: `blastn`
- Accepted values: `blastn`, `blastp`, `blastx`, `tblastn`, `tblastx`
- Example: `blast=tblastx`

GeneSeekr and Plasmid-Borne Identity do not verify that the query and database molecule types are appropriate for the selected BLAST program.

#### `cutoff`

Sets the minimum cutoff for matches included in the report.

- Default: `70`
- Example: `cutoff=80`

#### `evalue`

Sets the E-value cutoff.

- Default: `1E-05`
- Examples: `evalue=1E-10` or `evalue=0.01`

### Examples

#### Nucleotide-target request

```text
cutoff=80
2026-SEQ-0001
2026-SEQ-0002
```

Attach the FASTA-formatted target file. The default BLAST program is `blastn`.

#### Alternate BLAST program

```text
blast=tblastx
evalue=1E-10
2026-SEQ-0001
```

Verify that the selected BLAST program is appropriate for both the attached targets and assembly database.

See [issue 15644](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/15644) for an example Plasmid-Borne Identity request.

## Interpreting results

Plasmid-Borne Identity uploads five reports. `{BLAST_PROGRAM}` is replaced with the selected BLAST program, such as `blastn`.

### `geneseekr_{BLAST_PROGRAM}.xlsx`

Reports the strain name and percentage-identity match for each query target.

### `geneseekr_{BLAST_PROGRAM}_detailed.csv`

Reports detailed BLAST evidence for each target, including:

- strain name;
- percentage match;
- alignment length;
- subject length;
- E-value;
- positives;
- mismatches;
- gaps.

### `geneseekr_{BLAST_PROGRAM}.csv`

Provides the GeneSeekr summary in CSV format.

### `mob_recon_summary.csv`

Lists contigs predicted to be plasmid-derived. Contigs predicted to be chromosomal are omitted.

Important fields include:

- `Location` — the predicted plasmid name;
- `Contig` — the contig predicted to contain plasmid sequence.

One predicted plasmid can contain several contigs when it could not be circularized.

### `plasmid_borne_summary.csv`

Combines detailed GeneSeekr target hits with MOB-suite plasmid predictions. It maps target-containing contigs to predicted plasmids and includes plasmid incompatibility types when available.

The `Location` field reports either:

- `chromosome`; or
- the name of a predicted plasmid.

This location is a computational prediction. Review the detailed BLAST evidence, assembly context, and MOB-suite reconstruction before drawing conclusions.

## How long does it take?

GeneSeekr is fast, while MOB-suite is comparatively slower. Expect the combined analysis to take a few minutes per requested `SEQID`, depending on assembly size, target count, sample count, and service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** The automator cannot locate a draft genome assembly for the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its FASTA assembly is available, and submit a corrected request.

### The target attachment is missing or invalid

**Symptom:** The automator cannot read or search the target sequences.

**Likely cause:** The target file was not attached or is not valid FASTA.

**What to do:** Attach a correctly formatted FASTA file containing the target sequences.

### The BLAST program does not match the sequence types

**Symptom:** The analysis returns no useful hits or fails unexpectedly.

**Likely cause:** The selected BLAST program is incompatible with the attached query targets or nucleotide assembly database, for example `blastp` with nucleotide query and database sequences.

**What to do:** Verify the molecule type of the query targets and select the appropriate BLAST program. Use the default `blastn` for nucleotide targets searched against nucleotide assemblies.

### A target has an uncertain predicted location

**Symptom:** A detected target cannot be confidently assigned to a predicted plasmid or chromosome.

**Likely cause:** Draft assemblies can fragment plasmids across contigs, and plasmid reconstruction is predictive.

**What to do:** Review `geneseekr_{BLAST_PROGRAM}_detailed.csv`, `mob_recon_summary.csv`, and the assembly context together.

## Related automators

- [GeneSeekr](geneseekr.md) — searches FASTA-formatted files for predefined or custom targets without plasmid-location summarization.
- [MobSuite](mobsuite.md) — detects, reconstructs, and types predicted plasmids without requiring a user-supplied target set.
- [AMRsummary](amrsummary.md) — combines ResFinder acquired-AMR detection with MOB-suite plasmid-location predictions.

# GeneSeekr

## What does it do?

Use **GeneSeekr** to detect predefined or custom gene targets in FASTA-formatted files. GeneSeekr operates on assembled sequence data; use [Sipprverse](sipprverse.md) when the targets must be detected directly in raw, paired-end FASTQ reads.

GeneSeekr supports gene detection, selected typing analyses, and custom target databases. The supported analysis name must be supplied in the Redmine issue Description.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
geneseekr
```

Spelling matters, but matching is not case-sensitive.

### Description

The **Description** field must contain:

1. an analysis declaration in the form `analysis=requested_analysis`;
2. any additional field required by that analysis; and
3. one `SEQID` per line.

Example structure:

```text
analysis=resfinder
2026-SEQ-0001
2026-SEQ-0002
```

#### Supported analyses

- `gdcs` — detects genomically dispersed conserved sequences in *Escherichia*, *Listeria*, *Salmonella*, and *Vibrio*. Also include `organism=ORGANISM`.
- `genesippr` — uses a custom suite of genes derived from *Bacillus*, *Campylobacter*, *Escherichia*, *Listeria*, *Salmonella*, *Staphylococcus*, and *Vibrio*.
- `mlst` — determines multilocus sequence type for *Bacillus*, *Campylobacter*, *Escherichia*, *Listeria*, *Salmonella*, *Staphylococcus*, and *Vibrio*. Also include `organism=ORGANISM`.
- `cgmlst` — determines core-genome multilocus sequence type for *Escherichia* and *Yersinia*. Also include `organism=ORGANISM`.
- `resfinder` — identifies acquired antimicrobial-resistance genes.
- `rmlst` — determines ribosomal multilocus sequence type.
- `serosippr` — calculates the serotype for *Escherichia*.
- `sixteens` — determines the closest 16S match.
- `virulence` — detects virulence genes.
- `custom` — detects targets from a user-supplied FASTA file. This analysis requires an attachment.

GeneSeekr also provides analyses based on ICEberg databases from the [ICEfinder publication](https://academic.oup.com/nar/article/47/D1/D660/5165266):

- `all_ices` — detects all included integrative and conjugative element targets.
- `aice` — detects actinomycete integrative and conjugative element targets.
- `cime` — detects cis-mobilizable element targets.
- `ime` — detects integrative and mobilizable element targets.
- `t4ss` — detects Type IV Secretion System targets.

The ICEberg databases are available from the [ICEberg download page](https://bioinfo-mml.sjtu.edu.cn/ICEberg2/download.html).

### Attachments

Most standard analyses do not require an attachment.

For `analysis=custom`, attach a FASTA-formatted file containing the target sequences. The source documentation does not specify a required attachment filename or a Description parameter that refers to it; verify the attachment requirements with the current implementation if a custom request fails.

### Optional parameters

#### `blast`

Selects the BLAST program.

- Default: `blastn`
- Accepted values: `blastn`, `blastp`, `blastx`, `tblastn`, `tblastx`
- Example: `blast=tblastx`

GeneSeekr does not verify that the query and database molecule types are appropriate for the selected BLAST program. None of the standard analyses currently uses a protein database.

#### `cutoff`

Sets the minimum cutoff for matches included in the report.

- Default: `70`
- Example: `cutoff=80`

#### `evalue`

Sets the E-value cutoff.

- Default: `1E-05`
- Example: `evalue=1E-10`

#### `align`

Controls whether reports include alignments.

- Default: `False`
- Example: `align=True`

#### `unique`

Controls whether only the best hit is reported when multiple hits occur at the same location in a contig.

- Default: `False`
- Example: `unique=True`

#### `fasta`

Controls whether the output includes FASTA files containing strain-specific target-sequence matches.

- Default: `False`
- Example: `fasta=True`

### Examples

#### ResFinder request

```text
analysis=resfinder
2026-SEQ-0001
2026-SEQ-0002
```

See [issue 14470](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/14470) for an example ResFinder request.

#### cgMLST request

```text
analysis=cgmlst
organism=Escherichia
2026-SEQ-0001
```

See [issue 27867](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/27867) for an example cgMLST request.

#### Custom request

```text
analysis=custom
2026-SEQ-0001
```

Attach the FASTA-formatted target file to the issue. See [issue 14471](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/14471) for an example custom request.

## Interpreting results

When GeneSeekr finishes, it uploads:

```text
geneseekr_output.zip
```

The archive contains the reports generated for the selected analysis. The supplied documentation does not define a single common report schema because the contents depend on the requested analysis. Interpret each report according to the selected analysis and its reported fields.

## How long does it take?

Runtime depends on the selected analysis and the number of requested `SEQID`s. As a general estimate, GeneSeekr takes approximately one minute per `SEQID`.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** GeneSeekr cannot locate the requested assembled sequence data.

**What to do:** Verify each `SEQID` and confirm that its FASTA-formatted assembly is available before submitting a corrected request.

### The analysis is missing, misspelled, or unsupported

**Symptom:** The Redmine issue receives an error describing the requested analysis.

**Likely cause:** The Description does not include `analysis=...`, contains a spelling error, or requests an analysis that GeneSeekr does not support.

**What to do:** Choose one of the documented analysis names and submit a corrected request.

### A custom target file is missing or unreadable

**Symptom:** The custom analysis cannot read or use its target database.

**Likely cause:** The FASTA-formatted target file was not attached or could not be read.

**What to do:** Attach a valid FASTA-formatted target file and submit a corrected `analysis=custom` request.

## Related automators

- [Sipprverse](sipprverse.md) — use for predefined or custom target detection directly in raw, paired-end FASTQ reads.
- [KMA](kma.md) — use for its supported resistance and toxin analyses, or custom targets, in assemblies or raw reads.

# KMA — Gene Detection

## What does it do?

Use **KMA** to align supported gene targets against sequence assemblies or raw reads. The Redmine KMA automator supports antimicrobial-resistance, biocide-resistance, metal-resistance, verotoxin, and custom target analyses.

KMA uses different processing modes for FASTA assemblies, paired-end Illumina reads, and single-end Nanopore reads. Specify the intended input with `seqtype` when the default assembly mode is not appropriate.

For background, see the [KMA publication](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-018-2336-6) and the [CGE KmerResistance website](https://cge.food.dtu.dk/services/KmerResistance/). Cite Clausen et al. (2018) when publishing results produced with KMA.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
kma
```

Spelling matters, but matching is not case-sensitive.

### Description

The **Description** field must contain:

1. an analysis declaration as the first line; and
2. one `SEQID` per line.

Example:

```text
analysis=amr
2026-SEQ-0001
2026-SEQ-0002
```

#### Supported analyses

- `amr` — detects antimicrobial-resistance gene targets.
- `biocide` — detects biocide-resistance gene targets.
- `metal` — detects metal-resistance gene targets.
- `verotoxin` — detects toxin gene targets in Shiga toxin-producing *Escherichia coli*. The database was curated for poresippr.
- `custom` — detects targets from a user-supplied FASTA database. Also provide `targetsfile=filename.fasta` and attach that file.

### Attachments

Standard analyses use databases provided with the automator and do not require a target attachment.

For `analysis=custom`:

1. attach a multifasta target database;
2. include its exact filename and suffix with `targetsfile`; and
3. ensure the FASTA headers contain the target names you want used in the output CSV.

Example:

```text
analysis=custom
targetsfile=targets.fasta
2026-SEQ-0001
```

### Optional parameters

#### `seqtype`

Selects the input type.

- Default: `fasta`
- Paired-end raw reads: `seqtype=fastq`
- Single-end Nanopore raw reads: `seqtype=minionfastq`

KMA uses different commands for paired-end Illumina and single-end Nanopore data.

#### `targetsfile`

Identifies the attached target database for `analysis=custom`.

- Default: `False`
- Example: `targetsfile=genes.fasta`

The filename may vary, but it must include the file suffix and match the attachment name.

#### `nanopore`

Controls the documented Nanopore assembly mode.

- Default: `False`
- Example: `nanopore=TRUE`

The supplied documentation describes this option as applying to assemblies. Use `seqtype=minionfastq` for single-end Nanopore raw reads.

#### `min_ID`

Sets the minimum template identity, as a percentage, required for a target hit to be reported.

- Default: `False`
- Example: `min_ID=90`

The supplied documentation associates this example with Nanopore analysis.

#### `readcount`

Adds read-mapping information for each database target.

- Default: `False`
- Example: `readcount=TRUE`

This option is most useful with raw reads. Assembly analysis may report only one read mapping to each target.

#### `align`

Creates consensus alignment files with `.fsa` and `.aln` extensions.

- Default: `False`
- Example: `align=TRUE`

#### `vcf`

Creates VCF files containing positions that differ from the template. The automator uses KMA's `-vcf 2` base-calling filter.

- Default: `False`
- Example: `vcf=TRUE`

Generating VCF files increases runtime.

#### `hmm`

Uses a hidden Markov model to identify high-scoring subsequences in the query.

- Default: `False`
- Example: `hmm=TRUE`

### Examples

#### Assembly analysis

```text
analysis=amr
2026-SEQ-0001
2026-SEQ-0002
```

#### Paired-end raw-read analysis

```text
analysis=amr
seqtype=fastq
readcount=TRUE
2026-SEQ-0001
```

#### Custom analysis

```text
analysis=custom
targetsfile=targets.fasta
2026-SEQ-0001
```

Attach `targets.fasta` to the issue. See [issue 28117](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/28117) for an example KMA request. Any temporary result-download links in the issue may expire.

## Interpreting results

When KMA finishes, it uploads an archive named using the Redmine issue identifier:

```text
kma_output_redmineID.zip
```

The archive contains:

```text
kma_output.csv
```

This CSV summarizes target hits for all requested sequences. A listed resistance or gene target does not by itself prove that the strain carries the associated phenotype. Review at least:

- `Template_Identity` — identity between the detected sequence and the database template;
- `Template_Coverage` — coverage of the database template.

A hit with `100` for both identity and coverage provides stronger evidence that the complete target is present. Hits with lower identity or coverage require further review.

Depending on selected options, the archive may also contain:

- read-count output;
- `.fsa` and `.aln` consensus alignments;
- VCF files.

## Databases provided with the automator

The AMR, biocide, and metal-resistance databases were derived from NCBI AMRFinderPlus version 3.11, downloaded on 2023-10-27, then manually curated and separated by target category.

The following genes were added to the biocide resistance database, as they were of interest for some research projects:

    -sugE_sugE_quaternary_ammonium_compound_efflux_NC_011514.1:c6661-6344
    -sugE_sugE_quaternary_ammonium_compound_efflux_NC_003197.2:4581504-4581833
    -sugE_sugE_quaternary_ammonium_compound_efflux_NC_017731.1:2886005-2886319
    -sugE_sugE_quaternary_ammonium_compound_efflux_NC_020418.1:c2409693-2409373
    -sugE_sugE_quaternary_ammonium_compound_efflux_NC_016830.1:5098537-5098851
    -sugE(p)_sugESalmonella_quaternary_ammonium_compound_efflux_NC_010259.1:c4461-4144
    -sugE(c)_sugESalmonella_quaternary_ammonium_compound_efflux_NC_003198.1:4557993-4558310
    -sugE(p)_sugEEcoli_quaternary_ammonium_compound_efflux_NC_019069.1:c60194-59877
    -sugE(p)_sugEEcoli_quaternary_ammonium_compound_efflux_KC285365.1
    -sugE(c)_sugEEcoli_quaternary_ammonium_compound_efflux_LT903847.1:c240852-240535
    -bcrA_bcrABC_Listeria_benzalkonium_chloride_efflux_JX023276.1:106-645
    -bcrB_bcrABC_Listeria_benzalkonium_chloride_efflux_JX023276.1:657-974
    -bcrC_bcrABC_Listeria_benzalkonium_chloride_efflux_JX023276.1:992-1336
    -qacH_qacH_Listeria_quaternary_ammonium_compound_efflux_HG329628.1:983-1354
    -emrE_emrE_Listeria_Listeria_quaternary_ammonium_compound_resistance_NC_013768.1:1817073-1817396
    -qacA_qacA_Listeria_quaternary_ammonium_compound_efflux_KC980922.1
    -qacC_qacC_Listeria_quaternary_ammonium_compound_efflux_RJZ34303
    -qacED1_qacED1_Acinetobacter_quaternary_ammonium_compound_efflux_KM972592.1
    -mdfA_multidrug_efflux_pump_NC_00913.3:883673-884905

A laboratory-curated verotoxin database is also provided. Contact Catherine Carrillo for additional information about that database.

## How long does it take?

KMA is generally fast, but runtime depends on the selected analysis, input type, enabled output options, and number of requested sequences. Options such as `vcf=TRUE` increase runtime.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** The requested assembly or raw-read files cannot be found.

**What to do:** Verify the `SEQID`s and confirm that the required input type is available.

### A custom target file is missing or not recognized

**Symptom:** The issue receives a warning that the custom target file is missing.

**Likely cause:** The multifasta file was not attached, or `targetsfile` does not exactly match the attachment filename.

**What to do:** Attach the target file and include its complete filename and suffix, for example `targetsfile=targets.fasta`.

### The selected input type is incorrect

**Symptom:** KMA cannot find or correctly process the expected sequence files.

**Likely cause:** The request uses the default `fasta` mode for raw reads, or specifies the wrong raw-read mode.

**What to do:** Use `seqtype=fastq` for paired-end raw reads or `seqtype=minionfastq` for single-end Nanopore raw reads.

## Related automators

- [GeneSeekr](geneseekr.md) — use for its supported predefined or custom analyses on FASTA-formatted assemblies.
- [Sipprverse](sipprverse.md) — use for its supported predefined or custom analyses directly on raw, paired-end FASTQ reads.

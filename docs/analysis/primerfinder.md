# PrimerFinder

![PrimerFinder Legacy workflow](../img/primer_finder_legacy.png)

## What does it do?

Use **PrimerFinder** to perform *in silico* PCR on FASTA-formatted sequence data.

PrimerFinder supports:

- `vtyper` — uses the primer set supplied by the VTyper workflow;
- `custom` — uses a FASTA-formatted primer file attached to the Redmine issue.

The legacy workflow computes primer-binding and amplicon statistics with the retired NCBI e-PCR suite. The current documented analysis also refers to `ipcress` for *in silico* PCR, particularly when discussing degenerate bases and contig-break behavior. Confirm the deployed backend and output format before removing this implementation note.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
primer_finder
```

Spelling matters, but matching is not case-sensitive.

### Description

The Description must contain an analysis declaration:

```text
analysis=vtyper
```

or:

```text
analysis=custom
```

It must also specify at least one sequence source:

- one or more `SEQID`s; or
- a NAS path to a multi-FASTA database using `database=/path/to/database.fasta`.

Enter all parameters before the `SEQID` list.

### Inclusivity and exclusivity panels

Use `inclusivity` and `exclusivity` headings to assign requested `SEQID`s to panels:

```text
analysis=vtyper
inclusivity
2014-SEQ-0276
exclusivity
2025-SEQ-0791
2025-SEQ-0799
```

If no panel heading is supplied, requested `SEQID`s default to the inclusivity panel:

```text
analysis=vtyper
2014-SEQ-0276
2025-SEQ-0791
2025-SEQ-0799
```

### Use a FASTA database from the NAS

Instead of listing `SEQID`s, provide a path to an existing multi-FASTA database:

```text
analysis=vtyper
database=/path/to/database.fasta
```

The database file must already exist on the NAS. PrimerFinder extracts each FASTA record into an individual file and derives its filename from the FASTA header.

The following header characters are replaced with underscores:

```text
/ . | space [ ] ( ) : '
```

For example, a header resembling:

```text
gbCAJ32491.1ARO:3002622aadA6/aadA10 [Pseudomonas aeruginosa]
```

is converted to a sanitized filename resembling:

```text
gb_CAJ32491_1_ARO_3002622_aadA6_aadA10__Pseudomonas_aeruginosa_
```

### Attachments

For `analysis=custom`, attach a FASTA-formatted file containing the primer pairs to test.

The supplied documentation says the primer file must follow a required format but does not show that format. Consult a known working custom request or the current implementation before publishing a complete primer-file example.

IUPAC degenerate bases are accepted, but the documented `ipcress` behavior may count the presence of degenerate bases as a mismatch even when a degenerate symbol should match the query base. If a primer pair contains one or more degenerate bases and the intended limit is two true mismatches, the existing guidance recommends setting:

```text
mismatches=3
```

Verify this behavior against the deployed implementation because it materially affects assay interpretation.

### Optional parameters

#### `mismatches`

Sets the allowed number of mismatches.

- Default: `2`
- Accepted values: `0`, `1`, `2`, `3`
- Example: `mismatches=1`

Any other value produces an error.

#### `minimum_amplicon_size`

Sets the minimum accepted amplicon length.

- Default: `0` (no minimum)
- Maximum accepted value: `10000`
- Must be less than `maximum_amplicon_size`
- Example: `minimum_amplicon_size=150`

#### `maximum_amplicon_size`

Sets the maximum accepted amplicon length.

- Default: `1500`
- Maximum accepted value: `10000`
- Must be greater than `minimum_amplicon_size`
- Example: `maximum_amplicon_size=1750`

#### `contig_breaks`

When no complete amplicon is found, controls whether PrimerFinder searches for the two primers on separate contigs and reports a positive result when both are found.

- Default: `False`
- Example: `contig_breaks=True`

A positive result produced across separate contigs does not demonstrate that the primers bound within one contiguous amplicon. Interpret these results separately from complete amplicons.

#### `probe`

Supplies a probe sequence to search within predicted amplicons. This option can be provided more than once.

```text
probe=CGCGTTATCATCACTGTTACCGATAGCG
```

### Examples

#### VTyper analysis with default inclusivity panel

```text
analysis=vtyper
2014-SEQ-0276
2025-SEQ-0791
```

See [issue 37408](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/37408) for a VTyper request with no explicit panel headings and a custom mismatch setting.

#### VTyper analysis with inclusivity and exclusivity panels

```text
analysis=vtyper
inclusivity
2014-SEQ-0276
exclusivity
2025-SEQ-0791
2025-SEQ-0799
```

See [issue 37405](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/37405).

#### Custom-primer analysis

```text
analysis=custom
mismatches=2
minimum_amplicon_size=150
maximum_amplicon_size=1750
2014-SEQ-0276
```

Attach the required FASTA-formatted primer file. See [issue 37407](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/37407).

#### Custom analysis with a probe

```text
analysis=custom
probe=CGCGTTATCATCACTGTTACCGATAGCG
2014-SEQ-0276
```

See [issue 37409](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/37409).

#### Analysis using an existing NAS database

```text
analysis=custom
database=/path/to/database.fasta
```

Attach the custom primer file. See [issue 37415](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/37415).

## Interpreting results

The supplied documentation does not identify the exact output archive or report filenames for the current PrimerFinder workflow.

Interpret results according to:

- inclusivity or exclusivity panel membership;
- selected mismatch allowance;
- minimum and maximum amplicon sizes;
- whether a complete amplicon was found on one contig;
- whether `contig_breaks=True` produced a split-contig result;
- whether each supplied probe was found in the predicted amplicon.

Do not interpret a split-contig primer result as equivalent to a complete contiguous amplicon. Review degenerate-primer results carefully because of the documented mismatch-counting limitation.

## How long does it take?

PrimerFinder is generally fast and commonly requires only seconds per sample. Runtime increases with sample count, database size, primer-pair count, probes, and broader amplicon or mismatch settings.

## What can go wrong?

### Required analysis is missing or unsupported

**Symptom:** PrimerFinder rejects the request and reports an analysis error.

**Likely cause:** `analysis=...` is absent, misspelled, or not one of `custom` and `vtyper`.

**What to do:** Add `analysis=custom` or `analysis=vtyper`.

### A required program setting is reported missing

**Symptom:** The issue reports a missing `program=requested_program` component.

**Likely cause:** The legacy error list refers to a required program parameter that is not otherwise documented on the page.

**What to do:** Check a current known-good issue or the deployed implementation before adding a `program` value. This requirement needs technical clarification.

### A custom primer file is missing or malformed

**Symptom:** A custom analysis fails before *in silico* PCR begins.

**Likely cause:** The FASTA-formatted primer file was not attached or does not follow the required primer-pair format.

**What to do:** Use the format from a current known-good custom request and validate the FASTA file before resubmitting.

### The mismatch value is unsupported

**Symptom:** The request returns an error for `mismatches`.

**Likely cause:** The value is outside `0`–`3` or is not an integer.

**What to do:** Select `0`, `1`, `2`, or `3`, accounting for the documented degenerate-base behavior.

### Amplicon-size limits are invalid

**Symptom:** PrimerFinder reports that a minimum or maximum amplicon size is invalid.

**Likely cause:** A value exceeds `10000`, is not numeric, or the minimum is not smaller than the maximum.

**What to do:** Use valid numeric limits with `minimum_amplicon_size < maximum_amplicon_size`.

### A requested `SEQID` is unavailable

**Symptom:** The issue reports that one or more requested sequences cannot be found.

**Likely cause:** The `SEQID` is incorrect or its FASTA assembly is unavailable.

**What to do:** Verify each `SEQID` and confirm that its assembly exists.

### A database path is unavailable

**Symptom:** PrimerFinder cannot load the supplied multi-FASTA database.

**Likely cause:** The path is incorrect or the file is not present on the NAS.

**What to do:** Verify the complete path and file permissions before resubmitting.

## Related automators

- [GeneSeekr](geneseekr.md) — searches assemblies for attached custom gene targets using BLAST-based analysis.
- [IntiminTyper](intimintyper.md) — subtypes detected `eae` genes in *E. coli* assemblies.
- [ECTyper](ectyper.md) — predicts *E. coli* O- and H-antigen serotypes.

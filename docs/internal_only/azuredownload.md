# Azuredownload

> **Internal-only workflow:** This page documents an operational data-ingestion automator. It must remain under `docs/internal_only/` and must not be linked from the standard user index.

## What does it do?

Use **Azuredownload** to retrieve raw reads, assemblies, and reports produced by the FoodPort assembly pipeline and place them in the appropriate NAS locations for use by other Redmine automators.

The workflow is intended for authorized internal data management. It uses the FoodPort sequence-folder name, sequencing-machine category, and destination category to locate and organize a completed assembly run.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
azuredownload
```

Spelling matters, but matching is not case-sensitive.

### Description

Provide exactly one value for each required field:

```text
machine=MACHINE_TYPE
location=DESTINATION
sequence_folder=YYMMDD-lab
```

#### `machine`

Select the source data category:

- `machine=miseq` — MiSeq runs assembled through FoodPort;
- `machine=nextseq` — NextSeq runs assembled through FoodPort;
- `machine=other` — other data assembled through FoodPort, including research assemblies or data obtained from NCBI and processed through the pipeline.

#### `location`

Select the NAS destination category:

- `location=atcc` — ATCC assemblies;
- `location=enterobase` — EnteroBase sequences;
- `location=ncbi` — raw data obtained from SRA or NCBI;
- `location=merged` — merged sequence data;
- `location=refseq` — RefSeq sequences;
- `location=other` — research or other data that do not fit the listed categories.

#### `sequence_folder`

Use the sequence-folder name associated with the completed FoodPort assembly run.

Use dashes, not underscores:

```text
sequence_folder=YYMMDD-lab
```

Even when the original FoodPort upload name contained an underscore, the Redmine request must use the corresponding dashed form.

### Attachments

No attachment is required. The automator locates the completed FoodPort output using `sequence_folder`.

### Optional parameters

The supplied documentation does not identify optional parameters.

### Example

```text
machine=other
location=ncbi
sequence_folder=260717-research
```

See [issue 34137](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/34137) for an example Azuredownload request.

## Interpreting results

Azuredownload returns:

```text
legacy_combinedMetadata.csv
```

This CSV contains metadata for the sequences found in the assembly folder. The workflow also moves the retrieved raw reads, assemblies, and reports into the configured NAS locations so downstream automators can resolve the resulting sequence records.

Verify that the expected sequences appear in the metadata report and that the selected `machine` and `location` categories match the source and intended destination.

## How long does it take?

Runtime depends on sample count, file sizes, and storage-transfer performance. The supplied documentation estimates approximately 30 minutes per sample.

## What can go wrong?

### The sequence folder cannot be found

**Symptom:** The issue reports that the assembly folder is unavailable.

**Likely cause:** `sequence_folder` is misspelled, uses underscores instead of dashes, or does not correspond to a completed FoodPort run.

**What to do:** Verify the exact folder identifier and use the dashed form.

### The FoodPort assembly folder was downloaded manually

**Symptom:** The workflow cannot find the expected pipeline output or encounters duplicate/replication errors.

**Likely cause:** The assembly folder was manually downloaded instead of being left for the completed COWBAT/FoodPort process to place in the expected location.

**What to do:** Do not manually download or relocate the assembly folder. Allow the completed pipeline to place it in the location used by Azuredownload.

### The machine or destination category is invalid

**Symptom:** The automator cannot route or store the retrieved data correctly.

**Likely cause:** `machine` or `location` is missing, misspelled, or unsupported.

**What to do:** Use one documented value for each required field.

## Access and navigation

Keep this page out of standard-access navigation and retrieval. Link it only from explicitly internal operational documentation.

# SRA Download

## What does it do?

Use **SRA Download** to import FASTQ data from the NCBI Sequence Read Archive (SRA), save the reads on the NAS, upload them to Azure storage, and submit them to the COWBAT assembly workflow through FoodPort.

This workflow imports external sequence runs into the internal processing environment. It is not the same as External Retrieve, which exports existing internal sequence data for download.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
sradownload
```

Spelling matters, but matching is not case-sensitive.

### Description

The first line must specify the run name:

```text
run_name=YYMMDD-sra
```

Use this value as the Azure storage container name and FoodPort run name.

Enter optional commands next, followed by one SRA run accession per line:

```text
run_name=260717-sra
SRR00000001
SRR00000002
```

### Attachments

No attachment is required. The automator downloads the requested runs from SRA.

### Optional parameters

#### `email`

Provides an email address to FoodPort for assembly-completion notification.

```text
email=user@example.gc.ca
```

#### `basic_assembly`

Runs COWBAT assembly without typing modules.

```text
basic_assembly
```

#### `preprocess`

Runs quality assessment and read trimming or error correction without assembly or typing.

```text
preprocess
```

The supplied documentation does not say whether `basic_assembly` and `preprocess` are mutually exclusive. Do not combine them unless the current implementation has been verified to support that combination.

### Examples

#### Full COWBAT workflow

```text
run_name=260717-sra
email=user@example.gc.ca
SRR00000001
SRR00000002
```

#### Assembly without typing modules

```text
run_name=260717-sra
basic_assembly
SRR00000001
```

#### Preprocessing only

```text
run_name=260717-sra
preprocess
SRR00000001
```

See [issue 34216](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/34216) for an example SRA Download request.

## Interpreting results

The workflow downloads the requested FASTQ files, stores them on the NAS, stages them in Azure storage, and submits the selected processing mode to FoodPort.

Monitor the Redmine issue and, when supplied, the FoodPort notification email for completion status. Confirm that every requested SRA accession was imported and that the resulting workflow mode matches the request:

- default — assembly and configured typing modules;
- `basic_assembly` — assembly without typing modules;
- `preprocess` — quality assessment and preprocessing without assembly.

The supplied documentation does not identify final archive names or the identifiers assigned to imported runs. Confirm those details from the completed issue and FoodPort output.

## How long does it take?

The supplied documentation estimates approximately 15–20 minutes per SRA accession. Total time also depends on SRA transfer speed, read volume, Azure staging, FoodPort queueing, and selected COWBAT processing mode.

## What can go wrong?

### An SRA accession is unavailable

**Symptom:** The issue warns that one or more requested SRA runs cannot be downloaded.

**Likely cause:** The accession is incorrect, unavailable, withdrawn, or temporarily inaccessible.

**What to do:** Verify each accession in SRA and submit a corrected request.

### The run name is missing or invalid

**Symptom:** The workflow cannot create the Azure container or FoodPort run.

**Likely cause:** `run_name` is absent, malformed, or unsuitable as a container/run identifier.

**What to do:** Put `run_name=YYMMDD-sra` on the first line and use a unique value appropriate for the request.

### The requested processing mode is unclear

**Symptom:** The workflow cannot determine whether to preprocess, assemble only, or run the default pipeline.

**Likely cause:** Conflicting commands were supplied or an option was misspelled.

**What to do:** Use no mode command for the default workflow, `basic_assembly` for assembly without typing, or `preprocess` for preprocessing only.

### External transfer or FoodPort submission fails

**Symptom:** Downloads, Azure staging, or FoodPort processing do not complete.

**Likely cause:** A remote transfer, storage, network, or downstream pipeline error occurred.

**What to do:** Review the Redmine issue for the failed stage and retry or escalate persistent infrastructure failures.

## Related data workflows

- [External Retrieve](external_retrieve.md) — exports existing internal reads or assemblies for local download.
- [Report Retrieve](report_retrieve.md) — retrieves COWBAT report files for existing internal sequences.

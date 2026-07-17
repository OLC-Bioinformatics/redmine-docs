# Merge

> **Internal-only workflow:** This page documents an operational sequence-processing automator. It must remain under `docs/internal_only/` and must not be linked from the standard user index.

## What does it do?

Use **Merge** to combine raw FASTQ data from multiple MiSeq runs of the same biological strain and generate a new merged assembly workflow record.

Merge is intended for authorized internal processing when repeated sequencing runs belong to the same isolate. The source runs must be verified as the same biological sample before submission.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
Merge
```

Spelling matters, but matching is not case-sensitive.

### Description

Leave the Description empty. Supply the complete request in one attached Excel workbook with the `.xlsx` extension.

### Required attachment

The workbook must contain these exact headers:

- `Location` — enter `MER`;
- `SEQID` — new merged identifier in the form `YYYY-MER-####`;
- `OLNID` — may be blank;
- `LabID` — may be blank;
- `LabIDIsolate` — may be blank;
- `OtherName` — source `SEQID`s separated by semicolons;
- `Genus` — genus of the isolate; may be blank;
- `Species` — may be blank;
- `Subspecies` — may be blank;
- `Serotype` — may be blank.

Example row concept:

```text
Location: MER
SEQID: 2026-MER-0001
OtherName: 2026-SEQ-0001;2026-SEQ-0002
```

Use a genuine `.xlsx` workbook rather than a renamed CSV file.

### Optional parameters

The supplied documentation does not identify optional Description parameters. Optional metadata columns may be left blank as noted above.

### Example

See [issue 14285](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/14285) for an example Merge request and workbook.

## Interpreting results

Merge returns the reports and assemblies normally produced by the WGS assembly workflow for the new merged record.

Verify that:

- the new `YYYY-MER-####` identifier is correct;
- every source `SEQID` belongs to the same biological strain;
- the resulting assembly and reports correspond to the merged input runs;
- expected quality-control metrics improve or remain acceptable.

Merging additional sequencing data does not guarantee a better assembly if the runs are contaminated, mismatched, or poor quality.

## How long does it take?

Runtime depends on sample count, read volume, and assembly workload. The supplied documentation estimates approximately 30 minutes per merged sample.

## What can go wrong?

### A source `SEQID` is unavailable

**Symptom:** The issue warns that one or more source runs cannot be found.

**Likely cause:** A source identifier in `OtherName` is incorrect or its FASTQ data are unavailable.

**What to do:** Verify each semicolon-separated source `SEQID`.

### The workbook is missing or malformed

**Symptom:** The automator cannot parse the request.

**Likely cause:** No `.xlsx` file was attached, a required header is missing or misspelled, or the file is not a valid Excel workbook.

**What to do:** Use the required headers exactly and attach a valid `.xlsx` file.

### The merged output identifier is invalid

**Symptom:** The workflow cannot create the new merged record.

**Likely cause:** `Location` is not `MER`, or `SEQID` does not follow the required `YYYY-MER-####` format.

**What to do:** Correct both fields in the workbook.

### Source runs belong to different isolates

**Symptom:** The merged assembly has poor quality, excessive fragmentation, conflicting taxonomic signals, or other unexpected results.

**Likely cause:** FASTQ data from different biological strains were combined.

**What to do:** Stop downstream use, verify sample identity, and repeat the workflow only with runs from the same strain.

## Access and navigation

Keep this page out of standard-access navigation and retrieval. Link it only from explicitly internal operational documentation.

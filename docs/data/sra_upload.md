# SRA Upload

## What does it do?

Use **SRA Upload** to transfer raw sequence files associated with internal `SEQID`s into the preload area of an NCBI Sequence Read Archive submission using SRA-provided FTP credentials.

SRA Upload handles file transfer only. You must first create or begin an SRA submission and obtain the temporary FTP username, password, and account-folder path from the SRA submission portal.

> **Credential warning:** The Redmine Description contains a temporary FTP password. Restrict the issue to the appropriate project and users, do not reuse the password elsewhere, and remove or rotate credentials when the transfer is complete if the SRA workflow permits it.

## Before creating the request

1. Sign in to the NCBI SRA submission portal.
2. Open **My Submissions**.
3. Start or open a **Sequence Read Archive** submission.
4. Under the data-preload options, select **FTP upload**.
5. Record the temporary:
   - username;
   - password;
   - account-folder path shown under **Navigate to your account folder**.

Portal labels may change over time. Use the FTP credentials and destination shown by the active SRA submission.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
SRA Upload
```

Spelling matters, but matching is not case-sensitive.

### Description

Use this exact line order:

1. SRA FTP username;
2. SRA FTP password;
3. SRA account-folder path;
4. one `SEQID` per subsequent line.

Example structure:

```text
FTP_USERNAME
FTP_PASSWORD
uploads/account_folder
2026-SEQ-0001
2026-SEQ-0002
```

Do not include the placeholder values literally. Copy the current temporary credentials and folder path from the SRA submission portal.

### Attachments

No attachment is required. SRA Upload locates the raw sequence files associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for SRA Upload.

### Example

See [issue 14963](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/14963) for an example SRA Upload request.

Do not copy expired credentials from an old issue.

## Interpreting results

When the transfer finishes, return to the SRA submission and select the preload folder. The uploaded files should appear in a directory named with the Redmine issue identifier.

Verify that:

- the expected issue-number folder exists;
- every requested `SEQID` has the expected FASTQ file or paired files;
- filenames are appropriate for the SRA submission;
- file sizes are plausible and the transfer completed before continuing the submission.

A completed Redmine issue confirms the transfer workflow finished; it does not by itself confirm that the SRA submission metadata or final NCBI submission is complete.

## How long does it take?

Small requests generally finish within a few minutes. Runtime increases with the number and size of the files and current network performance.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that one or more requested sequences cannot be found.

**Likely cause:** The identifier is incorrect or its raw FASTQ data are unavailable.

**What to do:** Verify each `SEQID` and confirm that the raw reads exist.

### The FTP transfer times out

**Symptom:** The issue reports an error such as `Connection reset by peer`.

**Likely cause:** The FTP connection failed, often during a large transfer.

**What to do:** Retry later or divide a large upload into smaller requests. Escalate persistent failures to the bioinformatics team.

### The FTP credentials are rejected

**Symptom:** The issue reports an authentication failure.

**Likely cause:** The username or password is incorrect, expired, or placed on the wrong line.

**What to do:** Obtain current credentials from the active SRA submission and ensure the username is first and password second.

### The FTP folder does not exist

**Symptom:** The issue reports that the destination folder cannot be found.

**Likely cause:** The third line does not exactly match the account-folder path supplied by SRA.

**What to do:** Copy the path from **Navigate to your account folder** without modifying it.

## Related data workflows

- [External Retrieve](external_retrieve.md) — exports raw reads or assemblies for local download.
- [SRA Download](sra_download.md) — imports public SRA runs into the internal FoodPort/COWBAT workflow.

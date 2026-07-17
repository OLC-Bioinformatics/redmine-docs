# Troubleshooting

Use this page for general Redmine Automator problems. For workflow-specific failures, first review the **What can go wrong?** section on the relevant analysis or data page.

## The issue does not start

### Check the Subject

The Subject selects the automator. Verify its spelling, spaces, underscores, and punctuation against the documentation page. Although most Subjects are case-insensitive, use the documented form.

### Check project access and issue placement

Confirm that the issue was created in the correct project and that your account has permission to submit requests.

### Check the Description structure

Some automators require a parameter on the first line, a specific `reference`/`compare` structure, or options before the `SEQID` list. Correct the structure and create a new request when required.

## The issue reports unavailable `SEQID`s

Verify that:

- every identifier is spelled correctly;
- the required data type exists;
- paired-end workflows have both R1 and R2 FASTQ files;
- assembly workflows have a FASTA assembly;
- older or external data have been imported to the expected storage location.

Some automators process available samples and warn about missing ones; others stop. Read the workflow page and final issue comments.

## The job is taking longer than expected

Runtime estimates apply after accounting for input size and normal compute availability. Jobs can wait when cluster resources are fully used.

Longer runtime can also result from:

- large sample counts;
- large FASTQ files or assemblies;
- database installation or update;
- high bootstrap counts;
- deep annotation modes;
- expensive secondary comparisons;
- large uploads to Dropbox or another transfer service.

Do not submit duplicate issues solely because a valid job is queued. Check recent issue comments and the workflow's expected runtime before escalating.

## The analysis completed but expected samples are missing

Review the issue for warnings about:

- missing `SEQID`s;
- unsupported genera or organisms;
- absent paired-read mates;
- divergence from the selected reference;
- no detected target or resistance result;
- malformed attachments;
- samples removed during filtering or tree cleanup.

A sample absent from an output can mean “not processed,” “unsupported,” or “no result detected,” depending on the automator.

## A result link does not work

Dropbox, FTP, or other transfer links can fail because of temporary connectivity, timeouts, or expiration.

Try the following:

1. retry after a short delay;
2. check whether the link expired;
3. split very large retrieval requests into smaller batches;
4. preserve the issue number and exact upload error;
5. contact the bioinformatics team if the problem persists.

## An attachment is rejected or ignored

Check:

- filename and extension;
- file size;
- FASTA, CSV, TSV, Newick, or Excel formatting;
- exact required headers;
- whether the automator expects a specific filename;
- whether only one attachment is allowed;
- whether an attachment takes precedence over Description entries.

Do not rename a CSV file to `.xlsx`; create a valid Excel workbook when `.xlsx` is required.

## Results look biologically unexpected

Before treating the result as a workflow error, review:

- organism identity and contamination;
- read and assembly quality;
- reference choice;
- database name and version;
- identity, coverage, and core-genome metrics;
- whether the method reports prediction, association, or direct evidence;
- known limitations documented on the workflow page.

Computational predictions and associations require biological interpretation and may need an independent method.

## Information to include when requesting help

Provide:

- Redmine issue number;
- automator Subject;
- relevant `SEQID`s;
- exact error text;
- submission and failure time;
- expected output;
- observed output;
- whether a retry produced the same result.

Do not send corporate passwords, SRA passwords, or reusable credentials by email.

## Requesting a new tool or feature

Contact the bioinformatics team with:

- the problem the tool should solve;
- expected input and output formats;
- source-code and publication links;
- approximate sample volume;
- required parameters;
- validation or regulatory considerations;
- whether the workflow should be standard-access or internal-only.

Named contacts can become outdated, so this documentation refers to the responsible bioinformatics team rather than individual employees.

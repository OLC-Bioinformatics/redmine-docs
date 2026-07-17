# rMLST

## What does it do?

Use **rMLST** to perform ribosomal multilocus sequence typing on requested sequence assemblies.

The automator prepares the required rMLST database when it is not already present on the NAS or when an update is requested. It then runs the GeneSeekr rMLST analysis and attaches the reports to the Redmine issue.

rMLST uses ribosomal protein gene loci and can support organism identification and comparison across a broad range of bacteria. It is different from conventional MLST, which uses an organism- or scheme-specific set of housekeeping loci.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
rmlst
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter one `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

### Database update

To request a database update, place `update` before the `SEQID` list:

```text
update
2026-SEQ-0001
2026-SEQ-0002
```

Updating or installing the database increases runtime.

### Attachments

The supplied documentation does not identify support for attached FASTA files in the dedicated Redmine rMLST automator. Use requested `SEQID`s unless the current implementation has been verified to support attachments.

### Optional parameters

The supplied documentation does not identify optional parameters other than the `update` command.

### Example

See [issue 34424](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/34424) for an example rMLST request.

## Interpreting results

When rMLST finishes, it uploads an archive named using the Redmine issue number:

```text
rmlst_output_<redmine issue number>.zip
```

The archive contains the GeneSeekr rMLST reports.

If `update` was included, a Redmine comment may report the database version in `YYMMDD` form:

```text
Using database version: YYMMDD
```

Record the database version when reproducibility is important. Interpret the rMLST match and support in the context of database coverage and sequence quality; a weak or conflicting match may require [Unknown Isolate](unknownisolate.md) or another follow-up workflow.

## How long does it take?

rMLST generally takes a few minutes. Runtime depends on whether the database must be installed or updated and increases with the number of requested sequences.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable assemblies.

**Likely cause:** The automator cannot locate the assembly associated with the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its assembly is available, and submit a corrected request.

### Database preparation increases runtime or fails

**Symptom:** The request takes longer than expected or reports a database installation or update error.

**Likely cause:** The required rMLST database was absent, an update was requested, or database preparation failed.

**What to do:** Review the issue messages for the database version or error. Escalate persistent database-preparation failures to the bioinformatics team.

## Related automators

- [MLST](mlst.md) — performs organism- and scheme-specific multilocus sequence typing.
- [Unknown Isolate](unknownisolate.md) — combines rMLST with MASH, ANIb, and ANIm evidence in a GROBI report.
- [GeneSeekr](geneseekr.md) — provides `analysis=rmlst` among its supported analyses.

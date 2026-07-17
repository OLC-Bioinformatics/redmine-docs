# MobSuite

## What does it do?

Use **MobSuite** to detect, reconstruct, and type predicted plasmids in draft genome assemblies.

The Redmine automator runs the `mob_recon` component of MOB-suite. `mob_recon` assigns assembly contigs to predicted chromosome or plasmid groups and provides typing information for reconstructed plasmids.

MOB-suite was developed by the Public Health Agency of Canada. For method details and complete output definitions, see the [MOB-suite repository](https://github.com/phac-nml/mob-suite).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
MobSuite
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter one draft-assembly `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

No attachment is required. MobSuite retrieves the draft genome assembly associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine MobSuite automator.

### Example

See [issue 37387](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/37387) for an example MobSuite request.

## Interpreting results

MobSuite uploads a ZIP archive containing one directory for each requested `SEQID`. Each directory contains the MOB-suite output for that assembly.

An important output is:

```text
contig_report.txt
```

This report assigns each assembly contig to a predicted chromosome or plasmid group and includes typing information for plasmid-derived contigs.

Plasmid reconstruction from a draft assembly is predictive. One plasmid may be represented by several contigs, repeated regions can complicate reconstruction, and chromosomal or plasmid assignments should be interpreted with assembly quality and supporting evidence.

See the [MOB-suite repository](https://github.com/phac-nml/mob-suite) for definitions of all other output files and columns.

## How long does it take?

MobSuite generally takes approximately one minute per requested `SEQID`. Total runtime depends on assembly size, sample count, compute availability, and result-upload time.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable assemblies.

**Likely cause:** MobSuite cannot locate the draft assembly associated with the requested `SEQID`.

**What to do:** Verify each `SEQID`, confirm that its assembly exists, and submit a corrected request.

### The Dropbox upload times out

**Symptom:** The analysis may complete, but the issue reports an upload error or does not provide a usable result link.

**Likely cause:** The Dropbox upload timed out or encountered a temporary connection problem.

**What to do:** Retry later if the problem is transient. If it persists, contact the bioinformatics team with the Redmine issue number.

### A plasmid prediction is uncertain

**Symptom:** A predicted plasmid is fragmented across several contigs or a contig assignment is difficult to interpret.

**Likely cause:** The draft assembly is fragmented, contains repeated sequence, or lacks enough context for unambiguous reconstruction.

**What to do:** Review `contig_report.txt`, assembly quality, and other MOB-suite outputs together. Use long-read or hybrid assembly evidence when available.

## Related automators

- [AMRsummary](amrsummary.md) — combines ResFinder acquired-AMR results with MOB-suite plasmid-location predictions.
- [Plasmid-Borne Identity](plasmidborneidentity.md) — searches assemblies for user-supplied targets and predicts whether matching targets are plasmid-borne.
- [PHASTEST](phastest.md) — detects prophage regions in bacterial genomes and plasmids rather than reconstructing plasmids.

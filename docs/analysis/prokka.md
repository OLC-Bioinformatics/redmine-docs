# Prokka

## What does it do?

Use **Prokka** to annotate genome assemblies and identify genes and other genomic features.

Prokka produces standardized annotation files that can be inspected directly or used by downstream workflows such as Roary, bcgTree, and pyseer.

For background, see the [Prokka publication](https://www.ncbi.nlm.nih.gov/pubmed/24642063) and [Prokka output documentation](https://github.com/tseemann/prokka#output-files).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
Prokka
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter one assembly `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

No attachment is required. Prokka retrieves the genome assembly associated with each requested `SEQID`.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine Prokka automator.

### Example

See [issue 15195](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/15195) for an example Prokka request.

## Interpreting results

Prokka creates multiple annotation files for each genome. See the [Prokka output documentation](https://github.com/tseemann/prokka#output-files) for the complete output list.

Important files include:

### `.gff`

Contains genomic features and annotations in GFF format, together with sequence information in a format used by workflows such as Roary.

### `.gbk`

Contains the annotated genome in GenBank format for inspection or downstream analysis.

Other common Prokka outputs can include nucleotide and protein FASTA files, feature tables, and summary files. Record the Prokka version and relevant database information when annotations must be reproducible.

Automated annotation is predictive. Product names, hypothetical proteins, gene boundaries, and feature calls may require review before use in publication or biological conclusions.

## How long does it take?

Prokka generally takes approximately two to three minutes per genome. Total runtime depends on assembly size, sample count, service workload, and result-upload time.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable assemblies.

**Likely cause:** Prokka cannot locate the assembly associated with the requested `SEQID`.

**What to do:** Verify every `SEQID`, confirm that its assembly exists, and submit a corrected request.

### The Dropbox upload fails

**Symptom:** Annotation completes, but the issue reports that the large result package could not be uploaded.

**Likely cause:** A temporary Dropbox connection or upload problem occurred.

**What to do:** Retry later. These connection problems are usually temporary; contact the bioinformatics team if the failure persists.

### An annotation is overinterpreted

**Symptom:** A predicted gene name or product is treated as experimentally confirmed.

**Likely cause:** Automated annotation assigns features from sequence evidence and reference databases and can contain uncertain or generic calls.

**What to do:** Review important annotations with supporting sequence evidence and appropriate specialized tools.

## Related automators

- [Roary/Scoary](roary.md) — uses Prokka `.gff` annotations for pan-genome analysis.
- [bcgTree](bcgtree.md) — uses Prokka `.faa` protein files for core-gene phylogeny.
- [GWAS-pyseer](pyseer.md) — uses Prokka annotations to interpret significant k-mers.
- [PHASTEST](phastest.md) — provides prophage-focused detection, annotation, and visualization.

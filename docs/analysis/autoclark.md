# AutoCLARK

## What does it do?

Use **AutoCLARK** to identify species represented in raw reads or draft genome assemblies. AutoCLARK runs CLARK, a metagenomic classification tool, and reports the species detected in each requested sample.

AutoCLARK is useful when the expected species is uncertain or when you want a taxonomic profile of a sample. It is not a dedicated contamination-detection workflow: use [ConFindr](confindr.md) when the primary question is whether raw sequencing reads are contaminated.

For background, see the [CLARK website](http://clark.cs.ucr.edu/).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
AutoCLARK
```

Spelling matters, but matching is not case-sensitive.

### Description

The first line must identify the input type:

- `fastq` — analyze raw reads;
- `fasta` — analyze draft genome assemblies.

Enter one `SEQID` per subsequent line.

#### Raw-read request

```text
fastq
2026-SEQ-0001
2026-SEQ-0002
```

#### Assembly request

```text
fasta
2026-SEQ-0001
2026-SEQ-0002
```

### Attachments

No attachment is required. AutoCLARK retrieves the sequence data associated with each requested `SEQID` according to the selected input type.

### Optional parameters

The supplied documentation does not identify optional parameters for the Redmine AutoCLARK automator.

### Example

See [issue 12819](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/12819) for an example AutoCLARK request.

## Interpreting results

When AutoCLARK finishes, it uploads:

```text
abundance.xlsx
```

The workbook reports the species detected in each requested sample and their estimated proportions.

Interpret low-abundance classifications cautiously. The existing workflow guidance notes that species reported below approximately 1–2% are often classification artifacts rather than organisms truly present in the sample. This is a practical interpretation guideline, not a universal biological threshold; review the result in the context of input quality, expected organisms, database composition, and supporting analyses.

AutoCLARK reports taxonomic classifications. A secondary species classification does not by itself establish that a sample is contaminated.

## How long does it take?

AutoCLARK usually takes approximately 10–15 minutes per request. Requests containing many `SEQID`s can take substantially longer.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable sequences.

**Likely cause:** AutoCLARK cannot locate the raw reads or draft assembly requested for that `SEQID`.

**What to do:** Verify the `SEQID`, confirm that the selected input type is available, and submit a corrected request.

### The input type is missing or incorrect

**Symptom:** AutoCLARK cannot determine which sequence files to retrieve or analyze.

**Likely cause:** The first Description line is missing, misspelled, or inconsistent with the available data.

**What to do:** Use `fastq` for raw reads or `fasta` for draft assemblies.

### A low-proportion species is overinterpreted

**Symptom:** A species reported at a very low proportion is treated as definitively present.

**Likely cause:** Low-abundance CLARK assignments can be classification artifacts.

**What to do:** Review the proportion, expected sample composition, sequence quality, and supporting evidence. Use ConFindr when contamination in raw reads is the specific question.

## Related automators

- [ConFindr](confindr.md) — detects intra-species and inter-species contamination in raw sequencing reads.
- [Unknown Isolate](unknownisolate.md) — identifies an uncertain isolate from a draft genome assembly using rMLST, MASH, ANIb, and ANIm evidence.
- [Kraken2/Bracken](kraken2.md) and [MetaPhlAn](metaphlan.md) — provide metagenomic taxonomic analysis with different methods and trade-offs.

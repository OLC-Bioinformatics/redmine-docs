# SequenceExtractor

## What does it do?

Use **SequenceExtractor** to extract a requested nucleotide interval—or an entire contig—from a specific contig in a sequence assembly.

Each extraction identifies:

- the assembly `SEQID`;
- the exact contig name;
- the start coordinate; and
- the stop coordinate.

The source code is available in the [SequenceExtractor directory of the genemethods repository](https://github.com/OLC-LOC-Bioinformatics/genemethods/tree/main/genemethods/SequenceExtractor).

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
sequence_extractor
```

Spelling matters, but matching is not case-sensitive.

### Description

Provide one extraction request per line using four semicolon-separated fields:

```text
SEQID;contig name;start coordinate;stop coordinate
```

Example:

```text
2019-SEQ-0848;Contig_1_149.079_Circ;1;50
2019-SEQ-1019;Contig_1_388.862_Circ;14;77
2019-SEQ-0848;Contig_2_392.879_Circ;2;39
2019-SEQ-1019;Contig_3_52.4575;5;22
```

This example requests:

- bases 1–50 from `Contig_1_149.079_Circ` in `2019-SEQ-0848`;
- bases 14–77 from `Contig_1_388.862_Circ` in `2019-SEQ-1019`;
- bases 2–39 from `Contig_2_392.879_Circ` in `2019-SEQ-0848`;
- bases 5–22 from `Contig_3_52.4575` in `2019-SEQ-1019`.

#### Extract an entire contig

Set both coordinates to `0`:

```text
2019-SEQ-0848;Contig_1_149.079_Circ;0;0
```

#### Reversed coordinates

The stop coordinate may be lower than the start coordinate. SequenceExtractor reorders the two values so the lower coordinate is used as the start.

### Attachments

You can supply the extraction list either:

- directly in the Description field; or
- in one attached text file.

Do not use both methods. If an attachment is present, entries in the Description are ignored.

The attachment filename does not matter. Avoid empty lines and use the same semicolon-separated format as the Description.

### Optional parameters

The supplied documentation does not identify optional parameters beyond the coordinate conventions described above.

### Examples

See [issue 28104](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/28104) for an example that supplies extraction details in an attachment. `SEQID`s appearing in that issue's Description are not required when the attachment contains the extraction list.

See [issue 28181](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/28181) for an example that supplies extraction details directly in the Description.

## Interpreting results

SequenceExtractor uploads:

```text
extracted_sequences.fasta
```

Each FASTA header identifies the source `SEQID`, contig, and requested coordinates. For example:

```fasta
>2019-SEQ-0848_Contig_1_149.079_Circ_1_50
AAAAAAAACAAATATATACTTTGATGATAACTTTCTAAATATCTACAAAA
>2019-SEQ-0848_Contig_2_392.879_Circ_2_39
AAAAAAACAATAAAAAACACCGCAAAAATGGATTGTTA
>2019-SEQ-1019_Contig_1_388.862_Circ_14_77
CAATAGTCTTATTTCCATTCAGGTATTCAATATTAATATTCATACGTTAATCCGATTTAT
CCTT
>2019-SEQ-1019_Contig_3_52.4575_5_22
ATATCATCAGATGGCTGC
```

Confirm that each expected extraction has a FASTA record and that the header contains the intended assembly, contig, and coordinates.

## How long does it take?

SequenceExtractor is generally fast. Expect each extraction entry to take a few seconds, with total runtime depending on the number of entries and current service workload.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable assemblies.

**Likely cause:** SequenceExtractor cannot locate the assembly associated with the requested `SEQID`.

**What to do:** Verify the `SEQID` and confirm that its assembly is available.

### The contig name does not match

**Symptom:** No sequence is returned for an otherwise valid assembly and coordinate range.

**Likely cause:** The contig name does not exactly match the name in the assembly.

**What to do:** Copy the complete contig name exactly, including capitalization, punctuation, underscores, and any `_Circ` suffix.

### A coordinate is outside the contig

**Symptom:** No sequence is returned for the requested interval.

**Likely cause:** The start or stop coordinate exceeds the contig length, for example requesting bases 789–882 from a 500-base contig.

**What to do:** Check the contig length and submit coordinates within its valid range. Use `0;0` when the entire contig is required.

### Description entries are unexpectedly ignored

**Symptom:** The automator processes the attachment but not the extraction lines in the Description.

**Likely cause:** SequenceExtractor gives the attachment precedence whenever an attachment is present.

**What to do:** Supply the extraction list in only one place: either the Description or one attachment.

## Related automators

- [GFA Retrieve](gfa_retrieve.md) — retrieves assembly graph files for supported hybrid assemblies rather than extracting a nucleotide interval.
- [External Retrieve](../data/external_retrieve.md) — exports complete sequence data when a larger data package is required.

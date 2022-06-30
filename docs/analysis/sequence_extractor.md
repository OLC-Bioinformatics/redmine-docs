# SequenceExtractor

### What does it do?

SequenceExtractor extracts the sequence data from a supplied contig at designated start and stop positions

Check out the source code: [github](https://github.com/OLC-LOC-Bioinformatics/genemethods/tree/main/genemethods/SequenceExtractor)

### How do I use it?

#### Subject

In the `Subject` field, put `sequence_extractor`. Spelling counts, but case sensitivity does not.

#### Description

There are two ways to use SequenceExtractor

You must provide a list of (one per line):

`SEQID;contig name;start coordinates; stop coordinates`

e.g. 

```
2019-SEQ-0848;Contig_1_149.079_Circ;1;50
2019-SEQ-1019;Contig_1_388.862_Circ;14;77
2019-SEQ-0848;Contig_2_392.879_Circ;2;39
2019-SEQ-1019;Contig_3_52.4575;5;22
```

Notes: 

- If you want an entire contig, supply the start and stop positions as 0
- The stop base can be lower than the start base. The script will change them, so that the start base is always lower

The above example performs four extractions:

1. bases `1-50` from `Contig_1_149.079_Circ` in the assembly for SEQID `2019-SEQ-0848`
2. bases `14-77` from `Contig_1_388.862_Circ` in the assembly for SEQID `2019-SEQ-1019`
3. bases `2-39` from `Contig_2_392.879_Circ` in the assembly for SEQID `2019-SEQ-0848`
4. bases `5-22` from `Contig_3_52.4575` in the assembly for SEQID `2019-SEQ-1019`

This list can be supplied in two possible ways:

1. In the Description
2. In an attached file (the name of the file doesn't matter, but please try to avoid empty lines)

You cannot supply the list both ways; if you provide an attachment, any entries in the Description will be ignored.

#### Example

For an example SequenceExtractor analysis, see [issue 28104](https://redmine.biodiversity.agr.gc.ca/issues/28104).

This example has SEQIDs in the Description, but it's not necessary.

#### Interpreting Results

SequenceExtractor will output a FASTA-formatted file, `extracted_sequences.fasta` ,that should look like the following if you used the example list above:

```
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

### How long does it take?

SequenceExtractor is pretty fast - expect it to take a few seconds per entry you give it.

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, 
you will get a warning message informing you of it.
2. The contig name must be provided exactly as it is written in the assembly
3. The start or stop position provided is invalid e.g. if you ask for positions 789-882 in a contig that is only 500bp long, nothing will be returned


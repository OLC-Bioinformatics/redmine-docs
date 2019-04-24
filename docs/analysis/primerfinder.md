# PrimerFinder

### What does it do?

The PrimerFinder performs _in silico_ PCR analyses on FASTA and FASTQ formatted files.

There are two different modules:

![PrimerFinder Legacy](../img/primer_finder_legacy.png) ![PrimerFinder Supremacy](../img/primer_finder_supremacy.png)

__PrimerFinder Legacy__ computes primer binding and amplicon statistics on FASTA formatted files using the now retired ePCR 
suite of tools from NCBI. 

__PrimerFinder Supremacy__ is able to process FASTA- and FASTQ-formatted files. If using FASTQ files, 
[BBMap](https://sourceforge.net/projects/bbmap/) is employed to bait out reads containing primer sequences. A second 
round of baiting is performed using the previously-baited reads. Contigs are assembled by [SPAdes](http://cab.spbu.ru/software/spades/) with only the baited reads. 

For both FASTA-formatted files and contigs assembled from FASTQ-formatted files, primers are 
BLASTed against contigs, and outputs are parsed to determine primer binding, and amplicon statistics

### How do I use it?

#### Subject

In the `Subject` field, put `primer_finder`. Spelling counts, but case sensitivity does not.

#### Description

In the `Description` field, you must provide:

**Required Components**

1. `program=requested_program`

    - acceptable programs are `legacy` and `supremacy`

2. `analysis=requested_analysis`

    - acceptable analyses are `custom` and `vtyper`
        - `custom` analyses require a FASTA-formatted file of primers you wish to use (see Attachments section for 
        additional details)
        
        - `vtyper` analyses will use the primer set from the vtyper tool

3. a list of SEQIDs (one per line)

**Optional Components**

In order to customise your PrimerFinder analyses, several settings can be optionally modified

- Number of mismatches allowed.
    - default is `2`
    - options are `0`, `1`, `2`, `3`. Anything else will return an error
    - modify as follows:
        - `mismatches=1`
- Format of sequence files to use (note that PrimerFinder legacy will still use FASTA-formatted files even if you try 
to specify FASTQ)
    - default is `fasta`
    - options are: `fasta` and `fastq`
    - modify as follows:
        - `format=fastq`
- K-mer size to use for SPAdes assembly
    - default is `55,77,99,127`
    - either provide and integer, e.g. `33`, or a comma-separated list, e.g. `21,33,55`
    - modify as follows:
        - `kmersize=33` *OR*
        - `kmersize=21,33,55`
- Export amplicons in PrimerFinder Legacy (amplicons are created by default by PrimerFinder Supremacy)
    - default is `False`
    - modify as follows:
        - `exportamplicons`
#### Attachments

For `custom` analyses, you are required to attach a FASTA-formatted file containing the primer set(s) you wish analysed. 
The file must have the following format:

    >gene1-F
    seq
    >gene1-R
    seq
    >gene2-F1
    seq
    >gene2-R1
    seq
    >gene2-F2
    seq
    >gene2-R2
    seq
    .....

You are allowed to use IUPAC degenerate bases in this file. 
__Please don't add too many degenerate bases, as the number of primers combinations can increase quickly.__

    # Dictionary of degenerate IUPAC codes
    iupac = {
        'R': ['A', 'G'],
        'Y': ['C', 'T'],
        'S': ['C', 'G'],
        'W': ['A', 'T'],
        'K': ['G', 'T'],
        'M': ['A', 'C'],
        'B': ['C', 'G', 'T'],
        'D': ['A', 'G', 'T'],
        'H': ['A', 'C', 'T'],
        'V': ['A', 'C', 'G'],
        'N': ['A', 'C', 'G', 'T'],
        '-': ['-']
    } 

#### Examples

Example PrimerFinder analyses:
 
PrimerFinder Legacy, custom analyses [issue 15776](https://redmine.biodiversity.agr.gc.ca/issues/15776)

PrimerFinder Legacy, vtyper analyses [issue 15777](https://redmine.biodiversity.agr.gc.ca/issues/15777)

PrimerFinder Supremacy, custom analyses, FASTA format [issue 15778](https://redmine.biodiversity.agr.gc.ca/issues/15778)

PrimerFinder Supremacy, custom analyses, FASTQ format [issue 15780](https://redmine.biodiversity.agr.gc.ca/issues/15780)


#### Interpreting Results

__PrimerFinder Legacy__

PrimerFinder Legacy will upload `ePCR_report.csv`, which contains the strain name, gene name as parsed from the primer file, location of the calculated amplicon, the size of the amplicon,
the name of the contig on which the amplicon was found, the total number of mismatches between the primer set and the target sequence, and the primers used to create the amplicon.

Here's an example created the example primer file from the `Attachments` section of this document:


| Sample        | Gene  | GenomeLocation | AmpliconSize | Contig                                            | TotalMismatches | PrimerSet |
|---------------|-------|----------------|--------------|---------------------------------------------------|-----------------|-----------|
| 2014-SEQ-1390 | gene1 | 30506-30699    | 194          | 2014-SEQ-1390_1_length_680177_cov_29.8782_ID_2219   | 0               | gene1_0_0 |
| 2014-SEQ-1390 | gene2 | 476139-476423  | 285          | 2014-SEQ-1390_32_length_32477_cov_33.4201_ID_2281 | 1               | gene2_1_1 |


Note that the way the primer set is numbered is based on the number of primer sets with the same gene name: _gene1_ had one primer set, `gene1-F` and `gene1-R`, and these are represented as `gene1_0_0`. There were two
primer sets for _gene2_, and in this example `gene2-F2` and `gene2-R2` annealed (with one mismatch)

If requested, amplicon sequences for each match will be created. Using the same example as above `2014-SEQ-1390_amplicons.fa` will contain:

    >2014-SEQ-1390_2014-SEQ-1390_1_length_680177_cov_29.8782_ID_2219_476139_476423_gene1_0_0
    amplicon..... 
    >2014-SEQ-1390_2014-SEQ-1390_32_length_32477_cov_33.4201_ID_2281_27409_27668_gene2_1_1
    amplicon..... 

__PrimerFinder Supremacy__

PrimerFinder Supremacy will generate `ePCR_report.csv`, with the following fields: strain name, gene name parsed from the primer file, location of the amplicon, size of the amplicon, 
name of contig on which the amplicon was found, the forward and reverse primers used to create the amplicon, the number of mismatches for the forward and reverse primers.

Here's an example created from the example primer file from the `Attachments` section of this document:

| Sample        | Gene  | GenomeLocation | AmpliconSize | Contig                                            | ForwardPrimers | ReversePrimers | ForwardMismatches | ReverseMismatches |
|---------------|-------|----------------|--------------|---------------------------------------------------|----------------|----------------|-------------------|-------------------|
| 2014-SEQ-1390 | gene1 | 30506-30699    | 194          | 2014-SEQ-1390_1_length_680177_cov_29.8782_ID_2219    | gene1-F_0      | gene1-R_0      | 0                 | 0                 |
| 2014-SEQ-1390 | gene2 | 476139-476423  | 285          | 2014-SEQ-1390_32_length_32477_cov_33.4201_ID_2281 | gene2-F2_0     | gene2-R2_0     | 1                 | 0                 |


Note that the primer names have `_0` appended to the end. In the case of IUPAC bases being present in the primer sequence, this number will be incremented for each primer created to satisfy all possible 
combinations. 

Amplicon sequences are always included. Using the same example as above, `2014-SEQ-1390_amplicons.fa` will contain:

    >2014-SEQ-1390_2014-SEQ-1390_1_length_680177_cov_29.8782_ID_2219_476139_476423_gene1-F_0_gene1-R_0
    amplicon
    >2014-SEQ-1390_2014-SEQ-1390_32_length_32477_cov_33.4201_ID_2281_27409_27668_gene2-F2_0_gene2-R2_0
    amplicon
    
Raw BLAST results are also uploaded. In the example above, `2014-SEQ-1390_rawresults.csv` will contain:

| qseqid                                            | sseqid     | positive | mismatch | gaps | evalue   | bitscore | slen | length | qstart | qend   | qseq                      | sstart | send | sseq                      |
|---------------------------------------------------|------------|----------|----------|------|----------|----------|------|--------|--------|--------|---------------------------|--------|------|---------------------------|
| 2014-SEQ-1390_1_length_680177_cov_29.8782_ID_2219 | gene1-R_0  | 25       | 0        | 0    | 3.07E-07 | 50.1     | 25   | 25     | 476399 | 476423 | TACGGTTCCTTTGACGGTGCGATGA | 25     | 1    | TACGGTTCCTTTGACGGTGCGATGA |
| 2014-SEQ-1390_1_length_680177_cov_29.8782_ID_2219 | gene1-F_0  | 25       | 0        | 0    | 3.07E-07 | 50.1     | 25   | 25     | 476139 | 476163 | GTGAAATTATCGCCACGTTCGGGCA | 1      | 25   | GTGAAATTATCGCCACGTTCGGGCA |
| 2014-SEQ-1390_32_length_32477_cov_33.4201_ID_2281 | gene2-F2_0 | 20       | 1        | 0    | 4.41E-06 | 42.1     | 21   | 21     | 27648  | 27668  | CGCCTTATTATACGACCAAAG     | 21     | 1    | CGCCTTATTTTACGACCAAAG     |
| 2014-SEQ-1390_32_length_32477_cov_33.4201_ID_2281 | gene2-R2_0 | 20       | 0        | 0    | 1.74E-05 | 40.1     | 20   | 20     | 27409  | 27428  | TGCCCAAAGCAGAGAGATTC      | 1      | 20   | TGCCCAAAGCAGAGAGATTC      |

### How long does it take?

PrimerFinder Legacy and PrimerFinder Supremacy on FASTA mode are very fast (seconds per sample), while PrimerFinder Supremacy FASTQ mode is relatively slow (a few minutes per sample)

### What can go wrong?

1. Requested SEQIDs are not available. 
1. Not including the `program=requested_program` component, or requesting an unsupported program
1. Not including the `analysis=requested_analysis` component, or requesting an unsupported analysis
1. Specifying an unsupported number of `mismatches`
1. Incorrectly formatted `kmersize`
1. Attaching an incorrectly formatted primer file, or not including a primer file for `custom` analyses
1. ?

If anything goes wrong, an error message explaining the error should be returned.
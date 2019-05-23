# eCGF

### What does it do?

The eCGF automator performs _in silico_ CGF subtyping of _Campylobacter_ using whole-genome sequence data in FASTA format 

Check out the source code: [github](https://github.com/dorbarker/cgfprediction)

### How do I use it?

#### Subject

In the `Subject` field, put `eCGF`. Spelling counts, but case sensitivity does not.

#### Description

All you need to put in the description is a list of SEQIDs you want to process, one per line.

#### Example

For an example eCGF analysis, see [issue 16196](https://redmine.biodiversity.agr.gc.ca/issues/16196).

#### Interpreting Results

ECTyper will output a summary report, `ISSUE_NUMBER_summary_report.csv` that should look something like 
the following excerpt:

| Genes          | 11168_cj0008 | 11168_cj0033 | 11168_cj0035 | ... | Nearest Match | Number of Similarities (/40) | ... |
|----------------|--------------|--------------|--------------|-----|---------------|------------------------------|-----|
| 2018-LET-0106  | 0            | 0            | 1            | ... | 27_3_4        | 40                           | ... |
| 2018-LET-0012  | 0            | 0            | 1            | ... | 926_2_1       | 40                           | ... |

### How long does it take?

eCGF is pretty fast - expect it to take a few seconds per genome you give it.

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, 
you will get a warning message informing you of it.
1. ?


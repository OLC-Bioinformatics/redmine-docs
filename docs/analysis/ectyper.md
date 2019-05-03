# ECTyper

### What does it do?

The ECTyper automator performs serotyping on FASTA files. 

Check out the source code: [github](https://github.com/phac-nml/ecoli_serotyping)

### How do I use it?

#### Subject

In the `Subject` field, put `ec_typer`. Spelling counts, but case sensitivity does not.

#### Description

All you need to put in the description is a list of SEQIDs you want to process, one per line.

#### Example

For an example ECTyper analysis, see [issue 15946](https://redmine.biodiversity.agr.gc.ca/issues/15946).

#### Interpreting Results

ECTyper will output a summary report, `ec_typer_report.tsv` ,that should look something like 
the following:

| Name          | O-type | H-type | Alleles    |           |            |
|---------------|--------|--------|------------|-----------|------------|
| 2014-SEQ-0276 | O157   | H7     | wzx: 1.00  | wzy: 0.58 | fliC: 1.00 |
| 2019-SEQ-0137 | O146   | H8     | wzx: 1.00  | wzy: 0.56 | fliC: 1.00 |
| 2019-SEQ-0145 | -      | H2     | fliC: 1.00 |           |            |

### How long does it take?

ECTyper is pretty fast - expect it to take a minute per genome you give it.

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, 
you will get a warning message informing you of it.


# GeneSeekr

### What does it do?

The GeneSeekr is a suite of analyses that detect gene targets in FASTA-formatted files.

### How do I use it?

#### Subject

In the `Subject` field, put `geneseekr`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

In the `Description` field, you must provide the requested analysis type as follows:

`analysis=requested_analysis`

The GeneSeekr pipeline supports the following analyses (again, spelling counts, but case sensitivity doesn't):

- gdcs - determines the presence of genomically-dispersed conserved sequences in the following genera: *Escherichia, Listeria, Salmonella, Vibrio*
- genesippr - custom suite of genes derived from the following genera: *Bacillus, Campylobacter, Escherichia, Listeria, Salmonella, Staphylococcus, Vibrio*
- mlst - determines multi-locus sequence type for the following genera: *Bacillus, Campylobacter, Escherichia, Listeria, Salmonella, Staphylococcus, Vibrio*
- resfinder - identifies acquired antimicrobial resistance genes
- rmlst - determines ribosomal multi-locus sequence type
- serosippr - calculates the serotype for *Escherichia*
- sixteens - determines closest 16S match
- virulence - finds virulence genes
- custom (**you must attach a FASTA-formatted file of target(s) to the issue**)

You must also include a list of SEQIDs one per line.

**Optional Components**

In order to customise your GeneSeekr analyses, several settings can be optionally modified

- BLAST program. **NOTE:** GeneSeekr does not check to see if your query or database are the appropriate molecule for the requested program. Additionally, none of the standard analyses currently have protein databases.
    - default is `blastn`
    - You can select one of the following BLAST programs to use:
        - blastn - nt query: nt db
        - blastp - protein query: protein db
        - blastx - translated nt query: protein db
        - tblastn - protein query: translated nt db
        - tblastx - translated nt query: translated nt db
    - modify as follows:
        - `blast=tblastx`
- Minimum cutoff for matches to be included in report.
    - default is `70`
    - modify as follows:
        - `cutoff=80`
- E-value cutoff
    - default is `1E-05`
    - modify as follows:
        - `evalue=1E-10`
- Include alignments in reports
    - default is `False`
    - modify as follows:
        - `align=True`
- Report unique hits only - does not report multiple hits at the same location in a contig. Instead, only the best hit is reported, and the rest are discarded
    - default is `False`
    - modify as follows:
        - `unique=True`
- Include FASTA file output of strain-specific target sequence matches 
    - default is `False`
    - modify as follows:
        - `fasta=True`       


#### Example

For an example GeneSeekr issue, see [issue 14470](https://redmine.biodiversity.agr.gc.ca/issues/14470) or [issue 14471](https://redmine.biodiversity.agr.gc.ca/issues/14471).

#### Interpreting Results

The GeneSeekr automator will upload a file called `geneseekr_output.zip` once it has completed. This file will contain all the reports generated for the requested analysis.

### How long does it take?

It depends on the analysis requested. The GeneSeekr pipeline should take about a minute to analyze each SEQID requested.

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.
2. There was an issue with the requested analysis: either one was not supplied, the was a typo, or you requested a currently-unsupported analysis. An error message detailing the problem will be added to the issue.
3. The `custom` analysis requires an attached FASTA-formatted file of gene targets. If the file was not attached, or there was an issue reading the file, an error message detailing the problem will be add to the issue.


# sipprverse

### What does it do?

The sipprverse is a suite of analyses that detect gene targets in raw FASTQ reads.

### How do I use it?

#### Subject

In the `Subject` field, put `sipprverse`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

In the `Description` field, you must provide:

1. `analysis=requested_analysis`

    The sipprverse pipeline supports the following analyses (again, spelling counts, but case sensitivity doesn't):
    
    - gdcs - determines the presence of genomically-dispersed conserved sequences in the following genera: *Escherichia, Listeria, Salmonella, Vibrio*
    - genesippr - custom suite of genes derived from the following genera: *Bacillus, Campylobacter, Escherichia, Listeria, Salmonella, Staphylococcus, Vibrio*
    - mash - finds closest matching RefSeq genome
    - mlst - determines multi-locus sequence type for the following genera: *Bacillus, Campylobacter, Escherichia, Listeria, Salmonella, Staphylococcus, Vibrio*
    - pointfinder - detects chromosomal mutations predictive of drug resistance
    - resfinder - identifies acquired antimicrobial resistance genes
    - rmlst - determines ribosomal multi-locus sequence type
    - serosippr - calculates the serotype for *Escherichia*
    - sixteens - determines closest 16S match
    - virulence - finds virulence genes
    - full (all the above analyses)
    - custom (**you must attach a FASTA-formatted file of targets to the issue**)

2. a list of SEQIDs (one per line)

**Optional Components**

In order to customise your sipprverse analyses, several settings can be optionally modified

- Minimum cutoff for matches to be included in report.
    - default is `0.90`
    - modify as follows:
        - `cutoff=0.85`
- Average pileup depth cutoff
    - default is `2`
    - modify as follows:
        - `averagedepth=3`
- Kmer size to use for baiting. Please don't lower this too much (11 is probably about as low as I would recommend)
    - default is `19`
    - modify as follows:
        - `kmersize=11`
- Do not automatically discard hits if there are internal soft clips present
    - default is `False`
    - modify as follows:
        - `allowsoftclips=True`

#### Example

For an example sipprverse issue, see [issue 15706](https://redmine.biodiversity.agr.gc.ca/issues/15706) or 
[issue 15707](https://redmine.biodiversity.agr.gc.ca/issues/15707).

#### Interpreting Results

The sipprverse automator will upload a file called `sipprverse_output.zip` once it has completed. This file will contain all the reports generated for the requested analysis.

### How long does it take?

It depends on the analysis requested. The sipprverse pipeline deals with raw reads, so expect that it should take a few minutes to analyze each SEQID requested.

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.
2. There was an issue with the requested analysis: either one was not supplied, the was a typo, or you requested a currently-unsupported analysis. An error message detailing the problem will be added to the issue.
3. The `custom` analysis requires an attached FASTA-formatted file of gene targets. If the file was not attached, or there was an issue reading the file, an error message detailing the problem will be add to the issue.


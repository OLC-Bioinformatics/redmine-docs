# Kraken2

### What does it do?

Kraken2 is a program for taxonomic sequence classification.

For more information, see the [Kraken2 github](https://github.com/DerrickWood/kraken2), and/or the protocol paper describing use of [Kraken2](https://www.nature.com/articles/s41596-022-00738-y).

If you publish data using this automator, don't forget to cite the authors of the tool [Wood and Salzberg, 2014](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2014-15-3-r46) and the Kraken2 paper [Wood et al., 2019](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1891-0)

### How do I use it?

#### Subject

In the `Subject` field, put `kraken2`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

You must include a list of SEQIDs one per line.

**Optional Components**

In order to customise your analyses, additional settings can be optionally modified:

- **seqtype** - By default, the Kraken2 will assume you are running an analysis on paired-end raw sequence data.
    - default is `paired` 
    - If you want to analyse long-read nanopore or assembly data, add the following line:
        - `seqtype=nanopore`
        - `seqtype=assembly`

- **database** - the default analysis will utilize the Kraken2 standard database (9/26/2022) from [Kraken2 index zone](https://benlangmead.github.io/aws-indexes/k2).
    - default is `kraken2` 
    - If you want to analyse using a different database, the following options are currently supported:
        - `database=greengenes`
        - `database=plusPF`
        - `database=rdp`
        - `database=silva`

    -**The kraken2 and plusPF databases are meant for metagenomes, all others are 16S based databases**

#### Example

For an example kraken2 analysis, see [issue 29355](https://redmine.biodiversity.agr.gc.ca/issues/29355). The zip file has been attached to this request as an example (the ftp links expire after approx. 2 weeks).

#### Interpreting Results

Kraken2 will upload a zipped folder called `kraken2_output_redmineID.zip` to the ftp once it has completed. This will contain report files for kraken2 and bracken analyses.

### How long does it take?

Kraken2 is much faster than the original kraken, however the time required for analysis will depend on the analysis type and number of sequences requested. If too many sequences are requested at once, the automator may run out of memory and fail. Please try to keep it to 10 metagenomes maximum.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.



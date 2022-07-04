# KMA - Resistance Detection

### What does it do?

KMA (k-mer alignment) is a program developed by the Center for Genomic Epidemiology (CGE) to align gene targets to sequence assemblies or raw-reads. [CARD](https://card.mcmaster.ca/analyze/rgi). This redmine automator tool currently allows analysis for antimicrobial, biocide, or metal resistance in sequence data.

For more information, see the [KMA publication](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-018-2336-6), and/or the [CGE website](https://cge.food.dtu.dk/services/KmerResistance/).

If you publish data using this automator, don't forget to cite the authors of the tool [Clausen et al., 2018](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-018-2336-6)

### How do I use it?

#### Subject

In the `Subject` field, put `kma`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

The first line of the description should be the analysis you would like to run (e.g. `analysis=amr`). This will depend on the type of sequence data your are investigating.

- The following options are currently supported:
    - `amr` - used for antimicrobial resistance gene detection
    - `biocide` - used for biocide resistance gene detection
    - `metal` - used for metal resistance gene detection
    - `custom` - gene detection using a custom target database uploaded by the user. The attached file **MUST** be named `targets.fasta`


You must also include a list of SEQIDs one per line.

**Optional Components**

By default, KMA will run the analysis for isolate assemblies. In order to customise your CARD-RGI analyses, additional settings can be optionally modified:

- seqtype - the default analysis will use genome assemblies.
    - default is `fasta` 
    - If you want to analyse raw-read data, add the following line: **UNDER DEVELOPMENT**
        - `seqtype=fastq`

- nanopore - the default analysis will be for short-read Illumina data.
    - default is `False` 
    - If you want to analyse long-read Nanopore data, add a line to your description:
        - `nanopore=TRUE`

#### Example

For an example KMA analysis, see [issue 28117](https://redmine.biodiversity.agr.gc.ca/issues/28117). The zip file has been attached to this request as an example (the ftp links expire after approx. 2 weeks).

#### Interpreting Results

KMA will upload a zipped folder called `kma_output_redmineID.zip` to the ftp once it has completed. This will contain a  kma_output.csv file with the target-hits/results for all of the sequences requested in the analysis. Just because a gene/resistance is listed here does not necessarily mean the strain carries that resistance - it's important
to look at the __Teplate_Identity__ and __Template_Coverage__ columns. You can be pretty sure that anything with 100 for both
is actually there, but anything else requires further analysis to be sure.

### How long does it take?

KMA is very fast, however the time required for analysis will depend on the analysis type and number of sequences requested.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.

2) A custom analysis was requested but `targets.fasta` file was not attached. You will get a warning message informing you of it.


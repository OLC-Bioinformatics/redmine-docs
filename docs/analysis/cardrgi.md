# CARD-RGI

### What does it do?

CARD-RGI (Comprehensive Antibiotic Resistance Database - Resistance Gene Identifier) is a program developed by the McMaster University to predict resistomes
in draft genome assemblies. [CARD website](https://card.mcmaster.ca/analyze/rgi)

For more information, see the [CARD publication](https://pubmed.ncbi.nlm.nih.gov/31665441/). If you publish data using this automator, don't forget to cite the authors of the tool [Alcock et al., 2020](https://pubmed.ncbi.nlm.nih.gov/31665441/)

### How do I use it?

#### Subject

In the `Subject` field, put `CARDRGI`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

The first line of the description should be the analysis you would like to run (e.g. `analysis=isolate`). This will depend on the type of sequence data your are investigating.

- The following options are currently supported:
    - `isolate` - used for resistome analysis of bacterial isolate sequence assemblies
    - `metagenome` - used for resistome analysis of metagenome fastq-files. (This option can also be used to 'force' the tool to complete resistome analysis of isolate fastq-files). Currently, this metagenome redmine automator uses [KMA](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-018-2336-6) for alignment of gene targets to the raw-read data. If you would like to try the other options (BWA or bowtie2), you can download the stand-alone tool from the [CARD github](https://github.com/arpcard/rgi#rgi-usage-documentation).


You must also include a list of SEQIDs one per line.

**Optional Components**

By default, CARD-RGI will run the analysis for isolate assemblies. In order to customise your CARD-RGI analyses, two additional settings can be optionally modified:

- loosehits - the default analysis will only include strict and perfect resistance gene matches. Setting this to true will also include 'loose' hits, which may or may not be contributing to resistance.
    - default is `False` 
    - If you want to include loose hits, add a line to your description:
        - `loosehits=TRUE`

- partialgenes - the default analysis will only include full gene matches.
    - default is `False` 
    - If you want to include partial gene hits, add a line to your description:
        - `partialgenes=TRUE`

#### Example

For an example using CARDRGI, see [issue 28111](https://redmine.biodiversity.agr.gc.ca/issues/28111). The zip file has been attached to this request as an example (the ftp links expire after approx. 2 weeks). Please note that 2017-SEQ-0054 is an isolate sequence, not actually a metagenome.

#### Interpreting Results

CARD-RGI will upload a zipped folder called `card-rgi_output_redmineID.zip` to the ftp once it has completed. 

**Isolate Analysis**

This folder will contain a `CARDRGI_output.csv` csv file which shows every AMR gene found in each
sample-sequence. Just because a gene/resistance is listed here does not necessarily mean the strain carries that resistance - it's important
to look at the __Best_Identities__ column, which contains the percent identity of the gene/target match to the top hit in CARD. You can be pretty sure that anything with 100
is actually there, but anything else requires further analysis to be sure. 

Also, some efflux and point-mutations may confer resistance in specific genera/species but not others, so it is important to consider this before coming to any conclusions even if they are a 100% match. Information about the output table can be found on the [CARD-RGI github page](https://github.com/arpcard/rgi#rgi-usage-documentation) under `RGI main Tab-Delimited Output Details`.

The isolate analysis will also output individual results files for each sequence, and a RGI-heatmap file including information about all of the isolate sequences in your analysis.

**Metagenome Analysis**

The zip folder will contain individual output files for each sequence analysed. It will also include `CARDRGI_gene_mapping_output.csv` and `CARDRGI_allele_mapping_output.csv` files. These contain the CARD-results of all resistance genes found in the raw fastq data for each sample-sequence. The `CARDRGI_allele_mapping_output.csv` allele file will include the data for the top allele hits for each sequence.

### How long does it take?

CARD-RGI is pretty fast, however the time required for analysis will depend on the analysis type and number of sequences requested. Expect approximately 2-5 minutes per sequence.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.


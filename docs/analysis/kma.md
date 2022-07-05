# KMA - Resistance Detection

### What does it do?

KMA (k-mer alignment) is a program developed by the Center for Genomic Epidemiology (CGE) to align gene targets to sequence assemblies or raw-reads. This redmine automator tool currently allows analysis for antimicrobial, biocide, and metal resistance, or a custom set of user targets in sequence data.

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
    - `custom` - gene detection using a custom target database uploaded by the user. The attached file **MUST** be named `targets.fasta`. The output csv file will use your fasta-file gene headers as names.


You must also include a list of SEQIDs one per line.

**Optional Components**

By default, KMA will run the analysis for isolate assemblies. In order to customise your CARD-RGI analyses, additional settings can be optionally modified:

- seqtype - the default analysis will use genome assemblies.
    - default is `fasta` 
    - If you want to analyse raw-read data, add the following line:
        - `seqtype=fastq`

- nanopore - the default analysis will be for short-read Illumina data. **This function is for assemblies**
    - default is `False` 
    - If you want to analyse long-read Nanopore data, add a line to your description:
        - `nanopore=TRUE`

#### Example

For an example KMA analysis, see [issue 28117](https://redmine.biodiversity.agr.gc.ca/issues/28117). The zip file has been attached to this request as an example (the ftp links expire after approx. 2 weeks).

#### Interpreting Results

KMA will upload a zipped folder called `kma_output_redmineID.zip` to the ftp once it has completed. This will contain a  kma_output.csv file with the target-hits/results for all of the sequences requested in the analysis. Just because a gene/resistance is listed here does not necessarily mean the strain carries that resistance - it's important
to look at the __Template_Identity__ and __Template_Coverage__ columns. You can be pretty sure that anything with 100 for both
is actually there, but anything else requires further analysis to be sure.

#### Databases Provided with the Automator

The databases for AMR, biocide, and metal resistance were derived from the NCBI [AMRFinderPlus database](https://www.ncbi.nlm.nih.gov/pathogens/antimicrobial-resistance/AMRFinder/), version 3.10 downloaded on 2021-05-06. This database was then manually curated and split into seqparate AMR, biocide, and metal resistance databases. The following genes were added to the biocide resistance database, as they were of interest for some research projects:
>sugE_sugE_quaternary_ammonium_compound_efflux_NC_011514.1:c6661-6344
>sugE_sugE_quaternary_ammonium_compound_efflux_NC_003197.2:4581504-4581833
>sugE_sugE_quaternary_ammonium_compound_efflux_NC_017731.1:2886005-2886319
>sugE_sugE_quaternary_ammonium_compound_efflux_NC_020418.1:c2409693-2409373
>sugE_sugE_quaternary_ammonium_compound_efflux_NC_016830.1:5098537-5098851
>sugE(p)_sugESalmonella_quaternary_ammonium_compound_efflux_NC_010259.1:c4461-4144
>sugE(c)_sugESalmonella_quaternary_ammonium_compound_efflux_NC_003198.1:4557993-4558310
>sugE(p)_sugEEcoli_quaternary_ammonium_compound_efflux_NC_019069.1:c60194-59877
>sugE(p)_sugEEcoli_quaternary_ammonium_compound_efflux_KC285365.1
>sugE(c)_sugEEcoli_quaternary_ammonium_compound_efflux_LT903847.1:c240852-240535
>bcrA_bcrABC_Listeria_benzalkonium_chloride_efflux_JX023276.1:106-645
>bcrB_bcrABC_Listeria_benzalkonium_chloride_efflux_JX023276.1:657-974
>bcrC_bcrABC_Listeria_benzalkonium_chloride_efflux_JX023276.1:992-1336
>qacH_qacH_Listeria_quaternary_ammonium_compound_efflux_HG329628.1:983-1354
>emrE_emrE_Listeria_Listeria_quaternary_ammonium_compound_resistance_NC_013768.1:1817073-1817396
>qacA_qacA_Listeria_quaternary_ammonium_compound_efflux_KC980922.1
>qacC_qacC_Listeria_quaternary_ammonium_compound_efflux_RJZ34303
>qacED1_qacED1_Acinetobacter_quaternary_ammonium_compound_efflux_KM972592.1


### How long does it take?

KMA is very fast, however the time required for analysis will depend on the analysis type and number of sequences requested.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.

2) A custom analysis was requested but `targets.fasta` file was not attached. You will get a warning message informing you of it.


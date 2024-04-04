# KMA - Gene Detection

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
    - `verotoxin` - used for toxin gene detection in Shiga toxin-producing E. coli (curated by Catherine Carrillo for poresippr)
    - `bacmet` - used for metal resistance gene detection using the [BacMet database](http://bacmet.biomedicine.gu.se/), which was converted to nucleotide format by James Robertson (reference to follow)
    - `custom` - gene detection using a custom target database uploaded by the user. The user **MUST** also add the flag `targetsfile=` and the name of their database file including the suffix, for example `targets.fasta` or `genes.fasta`. The output csv file will use your fasta-file gene headers as names.
<br>
- Bacterial Integrative and Conjugative Elements (ICEs) [ICEberg databases](https://bioinfo-mml.sjtu.edu.cn/ICEberg2/download.html) from the [ICEfinder publication](https://academic.oup.com/nar/article/47/D1/D660/5165266):
*CAUTION: the ICEfinder webtool seems to work differently than KMA, and may detect ICEs where KMA does not*
    - `all_ices` - used for all ICE gene detection
    - `aice` - used for actinomycete (AICEs) type ICE gene detection
    - `cime` - used for cis-mobilizable elements (CIMEs) ICE gene detection
    - `ime` - used for Integrative and Mobilizable Elements (IME) type ICE gene detection
    - `t4ss` - used for Type IV Secretion System (T4SS) type ICE gene detection


You must also include a list of SEQIDs one per line.

**Optional Components**

By default, KMA will run the analysis for isolate assemblies. In order to customise your CARD-RGI analyses, additional settings can be optionally modified:

- **seqtype** - the default analysis will use genome assemblies.
    - default is `fasta` 
    - If you want to analyse raw-read data, add the following line:
        - `seqtype=fastq`
        - `seqtype=minionfastq`
    - Note: seqtype for minion raw-reads is slightly different, as KMA uses different commands for paired-end (illumina) and single-end (nanopore) data

- **targetsfile** - the default analysis will use genome assemblies.
    - default is `False` 
    - If you want to selected `analysis=custom`, add the following line:
        - `targetsfile=filename.fasta`
    - Note: the filename can be whatever you would like. You must also include the suffix for your fasta file (e.g. genes.fasta, targets.fasta, vtxgenes.fasta, etc)

- **nanopore** - the default analysis will be for short-read Illumina data. **This function is for assemblies**
    - default is `False` 
    - If you want to analyse long-read Nanopore data, add a line to your description:
        - `nanopore=TRUE`

- **min_ID** - Use this option to specify the minimum template identity (i.e. relatedness to gene(s) in your database), in percent, needed in order to report a template as a hit.
    - default is `False` 
    - If you want to analyse long-read Nanopore data, add a line to your description (example is for matches greater than 90%):
        - `min_ID=90`

- **readcount** - the default analysis will only output a csv file containing information on matching target gene(s) presence including template identity, coverage, depth, etc. This flag will output read-mapping data for your targets. **This function is best used with raw-reads - Assembly analysis will likely only report 1 read mapping to each gene-target**
    - default is `False` 
    - If you want the analysis to output a file including information on the number of reads in your sequence mapping to each database-target, add a line to your description:
        - `readcount=TRUE`

- **align** - Create consensus alignment files *.fsa and *.aln.
    - default is `False`
    - If you want the analysis to create additional alignment files, add a line to your description:
        - `align=TRUE`

- **vcf** - Produce vcf-files, including any positions different from the template. This will use "-vcf 2" which applies the filter used for basecalling. **This function will make KMA run longer**
    - default is `False`
    - If you want the analysis to output a vcf file for each sequence, add a line to your description:
        - `vcf=TRUE`

- **hmm** - Use a HMM (Hidden Markov Model) to identify high scoring subsequences within the query.
    - default is `False`
    - If you want the analysis to use HMM, add a line to your description:
        - `hmm=TRUE`

#### Example

For an example KMA analysis, see [issue 28117](https://redmine.biodiversity.agr.gc.ca/issues/28117). The zip file has been attached to this request as an example (the ftp links expire after approx. 2 weeks).

#### Interpreting Results

KMA will upload a zipped folder called `kma_output_redmineID.zip` to the ftp once it has completed. This will contain a  kma_output.csv file with the target-hits/results for all of the sequences requested in the analysis. Just because a gene/resistance is listed here does not necessarily mean the strain carries that resistance - it's important
to look at the __Template_Identity__ and __Template_Coverage__ columns. You can be pretty sure that anything with 100 for both
is actually there, but anything else requires further analysis to be sure.

#### Databases Provided with the Automator

The databases for AMR, biocide, and metal resistance were derived from the NCBI [AMRFinderPlus database](https://www.ncbi.nlm.nih.gov/pathogens/antimicrobial-resistance/AMRFinder/), version 3.11 downloaded on 2023-10-27. This database was then manually curated and split into separate AMR, biocide, and metal resistance databases. The following genes were added to the biocide resistance database, as they were of interest for some research projects:

    -sugE_sugE_quaternary_ammonium_compound_efflux_NC_011514.1:c6661-6344
    -sugE_sugE_quaternary_ammonium_compound_efflux_NC_003197.2:4581504-4581833
    -sugE_sugE_quaternary_ammonium_compound_efflux_NC_017731.1:2886005-2886319
    -sugE_sugE_quaternary_ammonium_compound_efflux_NC_020418.1:c2409693-2409373
    -sugE_sugE_quaternary_ammonium_compound_efflux_NC_016830.1:5098537-5098851
    -sugE(p)_sugESalmonella_quaternary_ammonium_compound_efflux_NC_010259.1:c4461-4144
    -sugE(c)_sugESalmonella_quaternary_ammonium_compound_efflux_NC_003198.1:4557993-4558310
    -sugE(p)_sugEEcoli_quaternary_ammonium_compound_efflux_NC_019069.1:c60194-59877
    -sugE(p)_sugEEcoli_quaternary_ammonium_compound_efflux_KC285365.1
    -sugE(c)_sugEEcoli_quaternary_ammonium_compound_efflux_LT903847.1:c240852-240535
    -bcrA_bcrABC_Listeria_benzalkonium_chloride_efflux_JX023276.1:106-645
    -bcrB_bcrABC_Listeria_benzalkonium_chloride_efflux_JX023276.1:657-974
    -bcrC_bcrABC_Listeria_benzalkonium_chloride_efflux_JX023276.1:992-1336
    -qacH_qacH_Listeria_quaternary_ammonium_compound_efflux_HG329628.1:983-1354
    -emrE_emrE_Listeria_Listeria_quaternary_ammonium_compound_resistance_NC_013768.1:1817073-1817396
    -qacA_qacA_Listeria_quaternary_ammonium_compound_efflux_KC980922.1
    -qacC_qacC_Listeria_quaternary_ammonium_compound_efflux_RJZ34303
    -qacED1_qacED1_Acinetobacter_quaternary_ammonium_compound_efflux_KM972592.1
    -mdfA_multidrug_efflux_pump_NC_00913.3:883673-884905


A verotoxin gene database has also been provided, which has been curated by our lab. Please contact Catherine Carrillo for more information.

### How long does it take?

KMA is very fast, however the time required for analysis will depend on the analysis type and number of sequences requested.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.

2) A custom analysis was requested but targets file was not attached. You will get a warning message informing you of it. You must also add the flag `targetsfile=name.fasta` for the automator to recognize your attached multifasta file.

### Version (as of 2023-12-20)
KMA version = 1.4.9

NCBI AMRFinderPlus database (used for amr, biocide, and metal detection) = 3.11


# GWAS-pyseer

### What does it do?

pyseer is a tool for microbial-pangenome wide association studies (mGWAS). It is the reimplementation of [seer](https://github.com/johnlees/seer). pyseer allows one to analyse a set of genomes for genetic variation associated with bacterial phenotypes (such as antibiotic resistance, virulence, and host specificity). It is strongly suggested that you check out the [pyseer publication](https://academic.oup.com/bioinformatics/article/34/24/4310/5047751) and [pyseer documentation](https://pyseer.readthedocs.io/en/master/index.html) before use to determine which analysis is appropriate for your dataset. If used for publication, dont forget to cite Lees et al., 2018!

This redmine-automator will allow you to conduct a mGWAS using sequence data stored on the OLC-CFIA cluster. There are a few different options available, but if you would like another customisable option added to the automator (see [pyseer documentation](https://pyseer.readthedocs.io/en/master/index.html)) please contact an OLC-bioinformatician.


### How do I use it?

#### Subject

In the `Subject` field, put `pyseer`. Spelling counts, but case sensitivity doesnt.

<p>&nbsp;</p>

### Description

####**Required Components**

In the `Description` field, you must include an analysistype as follows:
`analysistype=requested_analysis`:

- `kmer`:
    - Will detect and count k-mers in selected dataset, then use k-mers as a variant to test both short variation and gene presence/absence.
    - The k-mers will be annotated by the redmine automator using [prokka](https://github.com/tseemann/prokka#output-files)
    - If kmer analysis is selected, it is also suggested that you upload a references.txt file (see description below). 
- `Roary`:
    - Roary will use the gene_presence_absence.Rtab file from a previous Roary analysis.
    - To use this analysis type, you must first run a roary analysis using the OLC Redmine Automator. See the [roary docs](https://olc-bioinformatics.github.io/redmine-docs/analysis/roary/) for details.
    - You must then include `roaryissue=redmine_issue_id`, where redmine_issue_id is the biorequest number from the roary analysis previously run (this will copy the gene_presence_absence.Rtab file from the previous issue to the current pyseer biorequest). **Please note:** the gene_presence_absence.Rtab files are only retained by the OLC system for requests made after 24-11-2021.
- `snv` (currently under development):
    - Will test the association of SNPs mapped to a reference.

You **must** attach a traits.tsv file. It must be named **traits.tsv** for the automator to work. This file must contain the SEQ-IDs in the first column, and trait (numerical) in the second column. The automator defaults to `binary` for traits (1,0 - for presence,absence). However, it is possible to use continuous data if you also include `continuous=True` in the description (default is `continuous=False`). You may also include `NA` if trait was not determined for that sequence.


You must also include a list of SEQIDs one per line.  

<p>&nbsp;</p>

####**Optional Components**

In order to customise your pyseer analysis, several settings can be modified. These settings will depend on which options were selected.

##### **Population structure**

The first step of pyseer is to estimate the population structure. This can be done a number of ways. 

- `mash` (default):
    - [mash](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0997-x) is the default distance/phylogeny used by the pyseer automator.
    - uses mash sketch to produce pairwise distance matrix, then calculate distances between all pairs of samples using mash dist.
    - a mash tree is also produced, and this phylogeny will be used to create the kinship matrix if the linearmixed association model is chosen
- `bcgtree`: 
    - if [bcgtree](https://pubmed.ncbi.nlm.nih.gov/27603265/) is selected, you can change a number of parameters. Please see the OLC Redmine Automator documentation for [bcgtree docs](https://olc-bioinformatics.github.io/redmine-docs/analysis/bcgtree/) for a list of options.
    - bcgtree phylogeny creation will take a while, and required time will increased with larger datasets. It is suggested you run a bcgtree biorequest separately for large datasets.
    - a phylogeny is output by bcgtree (RAxML_bestTree.final), which is then used to calculate distances between samples, and/or kinship matrix creation.
- `custom`:
    - if `distance=custom` is selected, a phylogeny tree file in newick format named `tree.newick` must also be attached to the request. 
    - This tree can be created however you'd like, but must be named `tree.newick` and the tree tip-labels must be the same as the SEQIDs in the pyseer request
    - the uploaded phylogeny will then be used for distance calculations between pairs of samples, and/or kinship matrix creation.

##### **Association models**

Please review the [pyseer documentation](https://pyseer.readthedocs.io/en/master/usage.html#population-structure) for a description of models. The current association models available for the pyseer automator include:

- `fixed`:
    - The patristic distances between all samples are used
    - A GLM is run on each variant
- `linearmixed`:
    - A linear mixed model (LMM) of fixed and random effects is fitted to the data
    - The selected phylogeny will be used to calculate pairwise distances (**note**:mash may be less accurate than other phylogenies).
    - Calculates the similarities between sequences based on the shared branch length between each pair's most recent common ancestor and the root.

##### **References file for k-mer analysis**

If a references file (`references.txt`) is not uploaded to the redmine request, one will be automatically generated. The benefit of uploading the references file is that you can select a few trusted reference sequences for the annotations. Otherwise, all sequences in the request will be included as "reference" quality.

The references are used first, and allow a close match of a k-mer. The drafts are used second, and require an exact match of the k-mer.

This `references.txt` is a tab-separated file containing the sequence filename, annotation filename, and type of the references to be used (there are no headers required):


| XXXX-SEQ-0001.fasta | XXXX-SEQ-0001.gff |  ref  |
|:-------------------:|:-----------------:|:-----:|
| XXXX-MIN-0001.fasta | XXXX-MIN-0001.gff |  ref  |
| XXXX-SEQ-0002.fasta | XXXX-SEQ-0002.gff | draft |
| XXXX-SEQ-0340.fasta | XXXX-SEQ-0340.gff | draft |


#### Example

For an example pyseer issue see [issue 25083](https://redmine.biodiversity.agr.gc.ca/issues/25083). **NOTE**: the output files for these issues no longer exist on the ftp server.

### Interpreting Results

The pyseer automator will upload links to the ftp for files called `pyseer_output.zip`, and possibly `prokka_output.zip` and `bcgtree_output.zip` depending on selected options.

The `pyseer_output.zip` will contain all of the pyseer outputs including the tree files and distance matrices. See [pyseer tutorial](https://pyseer.readthedocs.io/en/master/tutorial.html)

- `kmer` analysis will output `pyseer_kmers.txt`, the significant kmers (`significant_kmers.txt`), `annotated_kmers.txt`, and `gene_hits.txt` which is a summary file of the annotated k-mers found by pyseer.
    - the summarised annotated `gene_hits.txt` file can be graphed using R. See the [pyseer tutorial](https://pyseer.readthedocs.io/en/master/tutorial.html)
- `roary` analysis will output a `pyseer_COGs.txt` (COG = clusters of orthologous groups) file which contains the COGs not filtered out by pyseer. 
    - The first column in this file will contain the gene name (from the roary output gene_presence_absence.Rtab file)
    - If `high-bse` or `bad-chisq` are listed in the last column, there may be a high effect size/low frequency result (`high-bse`) or MAF filter may not be stringent enough (`bad-chisq`).

If `analysistype=kmer`, and/or `distance=bcgtree` were selected, the automator will upload a link to the ftp `prokka_output.zip`. This contains all of the outputs from prokka. Prokka will output a lot of files for each genome you give it - you can find a quick description of
each file [here](https://github.com/tseemann/prokka#output-files). Of particular interest are the `.gff` files, which pyseer uses for annotations.

If `distance=bcgtree` was used for distance/phylogeny, the `bcgtree_output.zip` file contains all of the outputs from bcgTree. The alignment files, and gene-id files output by bcgtree can be found in subfolders in the zip file. The most interesting outputs from bcgtree are the RAxML files. These files conaint the phylogenetic trees output by bcgTree: 


### How long does it take?

This will depend on the number of sequences requested, and the analysis type.

K-mer analysis will take hours in order to count the number of k-mers in a large number of sequences.

Prokka isnt the quickest thing around - expect it to take 2 to 3 minutes for each genome you give it. 

The bcgTree pipeline time will depend on the number of sequences and bootstraps requested. If a large number of sequences is being analysed, it is suggested you create a separate issue for phylogeny generation with bcgtree (however this is yet to be tested).

### What can go wrong?

1. Requested SEQIDs are not available. If we cant find some of the SEQIDs that you request, you will get a warning message informing you of it.
2. A traits.tsv file was not uploaded to redmine. You will get a warning message informing you to re-submit the request.



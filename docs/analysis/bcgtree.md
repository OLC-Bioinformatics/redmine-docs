# bcgTree

### What does it do?

bcgTree is a an automated phylogenetic tree building pipeline that builds trees from bacterial core genomes. The pipeline "automatically extracts 107 essential single-copy core genes, found in a majority of bacteria" (these genes were statistically determined, see [bcgTree publication](https://cdnsciencepub.com/doi/10.1139/gen-2015-0175?url_ver=Z39.88-2003&rfr_id=ori:rid:crossref.org&rfr_dat=cr_pub%20%200pubmed)). It then uses hidden Markov models and performs a partitioned maximum-likelihood analysis. If you want to 
learn more about it, check out the [bcgTree publication](https://cdnsciencepub.com/doi/10.1139/gen-2015-0175?url_ver=Z39.88-2003&rfr_id=ori:rid:crossref.org&rfr_dat=cr_pub%20%200pubmed). Dont forget to cite Ankenbrand and Keller, 2016!


### How do I use it?

#### Subject

In the `Subject` field, put `bcgtree`. Spelling counts, but case sensitivity doesnt.

#### Description

**Required Components**

In the `Description` field, you must include a list of SEQIDs one per line.


**Optional Components**

In order to customise your bcgTree analysis, several settings can be optionally modified.

- If you would like to change the number of bootstraps performed, add the following line to the description before the list of SEQIDs:
    - default is `100`
    - modify as follows:
        - `bootstraps=1000`
- If you would like to change the minimum number of proteomes in which a gene must occur in order to be kept:
    - default is `2`
    - modify as follows:
        - `min_proteomes=5`
- If you would like to change the amino acid substitution model used for the partitions by RAxML:
    - default is `AUTO`
    - modify as follows:
        - `aa_substitution_model=GTR`
    - you can select one of the following amino acid substitution models to use:
        - AUTO, DAYHOFF, DCMUT, JTT, MTREV, WAG,RTREV, CPREV, VT, BLOSUM62, MTMAM, LG,
          MTART, MTZOA, PMB, HIVB,HIVW, JTTDCMUT, FLU, STMTREV, DUMMY, DUMMY2,
          LG4M, LG4X, PROT_FILE, GTR_UNLINKED, GTR

#### Example

For an example bcgtree issue see [issue 25083](https://redmine.biodiversity.agr.gc.ca/issues/25083). **NOTE**: the output files for these issues no longer exist on the ftp server.

#### Interpreting Results

The bcgTree automator will upload links to the ftp for files called `prokka_output.zip` and `bcgtree_output.zip` once it has completed. 

The `prokka_output.zip` contains all of the outputs from prokka. Prokka will output a lot of files for each genome you give it - you can find a quick description of
each file [here](https://github.com/tseemann/prokka#output-files). Of particular interest are the `.faa` files, which bcgTree uses for analysis.

The `bcgtree_output.zip` file contains all of the outputs from bcgTree. The alignment files, and gene-id files output by bcgtree can be found in subfolders in the zip file. The most interesting outputs from bcgtree are the RAxML files. These files conaint the phylogenetic trees output by bcgTree: 

- RAxML_bestTree.final
- RAxML_bipartitionsBranchLabels.final
- RAxML_bipartitions.final
- RAxML_bootstrap.final

These files can be opened using a phylogenetic tree viewer of your choice.

There will also be the `config.txt` file containing the command(s) passed to bcgTree, as well as a `bcgtree.log` file containing all of the executed commands and their output(s) (including the random seed values used by RAxML).


### How long does it take?

Prokka isnt the quickest thing around - expect it to take 2 to 3 minutes for each genome you give it. After prokka is finished the bcgTree pipeline time will depend on the number of sequences and bootstraps requested. 

### What can go wrong?

1. Requested SEQIDs are not available. If we cant find some of the SEQIDs that you request, you will get a warning message informing you of it.
2. There was an issue with the requested amino acid substitution model: there was a typo, or you requested a currently-unsupported substitution model. An error message detailing the problem will be added to the issue.



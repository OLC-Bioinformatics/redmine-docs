# RoaryScoary

### What does it do?

####Roary
Roary is a pipeline for calculating pan genomes. Roary is not intended for metagenomics or for comparing extremely diverse sets of genomes. If you want to 
learn more about it, check out the [Roary publication](https://academic.oup.com/bioinformatics/article/31/22/3691/240757).

####Scoary
Scoary takes the `gene_presence_absence.csv` file output from Roary and a traits file created by the user, and calculates the associations between the given traits and all genes in the accessory genome. For more information, check out the [Scoary github](https://github.com/AdmiralenOla/Scoary) or [Scoary publication](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-1108-8).

### How do I use it?

#### Subject

In the `Subject` field, put `roary`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

In the `Description` field, you must provide the requested analysis type as follows:

`analysistype=requested_analysis`

The Roary automator supports the following analyses which perform operations on the pan genome of isolate sequences (again, spelling counts, but case sensitivity doesn't):

- union - reports the union of genes found in sequences
- intersection - reports the intersection of genes found in isolate sequences (core genes)
- complement - reports the complement of genes found in isolates (accessory genes)
- gene_multifasta - extracts the sequence of each gene listed and creates multi-fasta files for each gene listed (outputs protein multi-fastas). **NOTE**: you must provide an additional line: `genes=gene1,gene2,geneN` which is a comma-separated list of genes you would like multi-fasta files for (eg. `genes=fliC,gyrA`). **Case sensitivity counts for gene names**
- difference - reports the gene differences between sets of isolates

You must also include a list of SEQIDs one per line.


**Optional Components - Scoary**

If you would like scoary analysis to be completed, calculating the associations between traits and all genes in the accessory genome **you must attach a csv file of traits (traits.csv) to the issue**. Attaching a `traits.csv` file will automatically result in scoary analysis being conducted.

**This traits file must be formatted in a specific way**.

- the rows should correspond to your isolate sequence IDs
- the top right cell should be left blank
- the trait(s) data must be binary (0 for trait absent, 1 for trait present)
- all SEQ-IDs and traits should be uniquely named and not contain any weird characters (e.g. %;,/&[]@? etc) 

The below table is an example from the Scoary github:

|   | Trait 1  | Trait 2  | ...  | Trait N  |
|:-:|:-:|:-:|:-:|:-:|
| YYYY-SEQ-0001  |  1  | 0 | ...  |  1 |
| YYYY-SEQ-0002  |  0 |  0 | ...  | 1  |
| YYYY-SEQ-0003  |  1 |  0 | ... | 1 |


For more information, check out the [Scoary github](https://github.com/AdmiralenOla/Scoary)
     


#### Example

For an example Roary/Scoary issue see [issue 24968](https://redmine.biodiversity.agr.gc.ca/issues/24968), for an example Roary `analysistype=gene_multifasta` issue see [issue 25013](https://redmine.biodiversity.agr.gc.ca/issues/25013). (**NOTE**: the output files for these issues no longer exist on the ftp server.)

#### Interpreting Results

The Roary automator will upload links to the ftp for files called `prokka_output.zip` and `roary_output.zip` once it has completed. 

The `prokka_output.zip` contains all of the outputs from prokka. Prokka will output a lot of files for each genome you give it - you can find a quick description of
each file [here](https://github.com/tseemann/prokka#output-files). Of particular interest are the `.gff` files, which Roary uses for analysis.

The `roary_output.zip` file contains all of the outputs from roary. Some additional files will be present depending on the analysis type chosen:

- gene_multifasta - the zip file will include `.fa` files that contain the aligned protein sequences for the requested gene(s) (one for file each gene included in the analysis, eg. `gene_multifasta_fliC.fa` and `gene_multifasta_gyrA.fa`). If you'd like, these sequences can be further analysed using NCBI protein blast.
- scoary - the zip file will include `.csv` file(s) for each of the traits (column headers) provided in the attached `traits.csv` file.

### How long does it take?

Prokka isn't the quickest thing around - expect it to take 2 to 3 minutes for each genome you give it. After prokka is finished the Roary pipeline time will depend on the analysis type, and the number of genes/traits requested (in the case of gene_multifasta and scoary). 

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.
2. There was an issue with the requested analysistype: either one was not supplied, the was a typo, or you requested a currently-unsupported analysis. An error message detailing the problem will be added to the issue.
3. **Scoary** You will not get an error if the traits.csv file is not attached or formatted correctly. Roary will complete, but no `.csv` files for each of the traits will be included in the zip file. Unfortunately, you will have to submit the analysis request again with the correct `traits.csv` formatting.


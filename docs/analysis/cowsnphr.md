# COWSNPhR

### What does it do?

COWSNPhR (CFIA OLC Workflow for Single Nucleotide PHylogeny Reporting) is a pipeline developed in conjunction with the 
USDA APHIS Veterinary Services that determines the presence of high quality Single Nucleotide Variants between a 
reference strain and other closely related strains. Following annotation of the reference strain, using 
[Prokka](https://github.com/tseemann/prokka), the pipeline maps the raw reads of the query strains to the reference 
genome using [bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml), finds genetic variants with 
[DeepVariant](https://github.com/google/deepvariant), extracts variants, creates multiple sequence alignments, and
calculates a phylogenetic tree using [FastTree](http://www.microbesonline.org/fasttree/) to show the relatedness of 
these strains. The location of each variant is mapped to the annotation of the reference strain, and all variants are 
entered in a summary table.

### How do I use it?

#### Subject

In the `Subject` field, put `cowsnphr`. Spelling counts, but case sensitivity does not.

#### Description

The first line of your description needs to be `reference`, and the second line the SEQID of the strain you want to act
as your reference strain. Ideally, you'll want to pick a high-quality assembly for your reference.

If you wish to attach a reference file instead of providing a SEQID, the second line must be `attached`

The third line of your description should be `compare`, and lines after that the SEQIDs for strains to which you want to 
compare your reference.

#### Example

For example COWSNPhR analyses, see [issue 15681](https://redmine.biodiversity.agr.gc.ca/issues/15681) and 
[issue 15682 (uploaded reference file)](https://redmine.biodiversity.agr.gc.ca/issues/15682).

#### Interpreting Results

The zip file uploaded on COWSNPhR completion should contain four folders: 

* `vcf_files`: compressed global VCF files 
* `summary_tables`: table summarizing the location, prevalence, and annotation of variants
* `alignments`: multiple sequence alignment of variants
* `tree_files`: phylogenetic tree of alignment. If you want to view this tree, you can use a program such as
[FigTree](http://tree.bio.ed.ac.uk/software/figtree/) or a web-based viewer like [phylo.io](http://phylo.io).

### How long does it take?

Most COWSNPhR requests take ~1 hour to complete. If you submit a request for a larger COWSNPhR analysis (>30 strains), 
it may take substantially longer.

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) Strains too far apart. COWSNPhR requires that the strains you want to compare to the reference be closely related to
the reference. If you ask for an analysis with strains that are not very related, you will get a warning telling you so.

3) Other errors. This software is still in active development, so there may be unforeseen issues arising for novel 
reference: query combinations. 

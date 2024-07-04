# Snippy

### What does it do?

Snippy is a tool for rapid haploid variant calling and core genome alignment of closely related genomes. This automator can also be used to build a phylogenetic tree to attempt to show the
relatedness of these strains. More info can be found at the [Snippy github site](https://github.com/tseemann/snippy).

### How do I use it?

#### Subject

In the `Subject` field, put `snippy`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

The first line of your description needs to be `reference=`, the SEQID of the strain you want to act
as your reference strain. Ideally, you'll want to pick a high-quality assembly for your reference.

   eg. `reference=YYYY-MIN-NNNN`

If you wish to attach a reference file instead of providing a SEQID, the line must be `reference=attached`

You must also include a list of SEQIDs one per line.


**Optional Components**

We have also included the ability to 'cleanup' a multiple sequence alignment, and generate a tree using [Gubbins](http://nickjcroucher.github.io/gubbins/) and [IQ-tree](http://www.iqtree.org/). If you call SNPs for multiple isolates from the same reference, you are able to produce an alignment of "core SNPs" to build a phylogeny. To use this function, add the following line in the description:

  - `cleanup_&_tree=True`

This automator currently uses IQtree and default settings for gubbins, but other options are available. If you would like additional functions, please contact a bioinformatician and we will do our best to add them.

#### Example

For an example snippy, see [issue 30388](https://redmine.biodiversity.agr.gc.ca/issues/30388).

#### Interpreting Results

The zip file uploaded on snippy completion will contain folders for each of the comparator sequences, as well as core alignment files. Some of the files will also be uploaded/attached directly to the redmine request. Important files are:

* `core.txt`: A summary file. Tab-separated columnar list of alignment/core-size statistics of the number of: aligned, unaligned, variants, and low coverage bases between every strain submitted and the reference.
* `core.tab`: Tab-separated columnar list of core SNP sites with alleles but NO annotations
* `core.vcf`: Multi-sample VCF file with genotype GT tags for all discovered alleles
* Within each sequence folder there will also be files, including a tab separated summary for that particular sequence in comparison to the reference.

If cleanup_&_tree was included, then tree files will also be included in the output:

* `{issue.id}.snippy.gubbins.iqtree.tree`: An intermediate phylogenetic tree created by gubbins + iqtree. 
* `{issue.id}.snippy_final.clean.core.snp.aln.tree`: The **final** phylogeny created from the core snp alignment (created using IQtree and the GTR substitution method).


If you want to view these tree files, you can use a program such as
[FigTree](http://tree.bio.ed.ac.uk/software/figtree/) or a web-based viewer like [phylo.io](http://phylo.io).


Other files can also be important - see the [docs on Snippy Output files](https://github.com/tseemann/snippy)
for more information.

### Variant types

For more information, see the [docs on Snippy Output files](https://github.com/tseemann/snippy):

| Type | Name |  Example  |
|:----------:|:-----------------:|:-----------:|
| snp | Single Nucleotide Polymorphism |  A => T  |
| mnp | Multiple Nuclotide Polymorphism | GC => AT |
| ins | Insertion | ATT => AGTT |
| del | Deletion | ACGG => ACG |
| complex | Combination of snp/mnp | ATTC => GTTA |

### How long does it take?

It depends on the number of sequences and coverage/size of sequence files. Small snippy requests take ~15 minutes to complete (see the example request). If you submit a request for a larger snippy (>30 strains), or include larger long-read sequences, it may take substantially longer.

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) Strains too far apart. Snippy requires that the strains you want to compare to the reference be closely related to
the reference. If you ask for a snippy-analysis with things that are not very related, the analysis will fail. In this case, look at the `core.txt` file that is uploaded to the redmine request. If there are sequences with 0 under "Aligned", remove those sequences from the request and re-submit.

3) Why is the cleaned snp tree not showing my single-end sequences? - I'm not sure... the cleaning step(s) using gubbins appears to remove the minion (single-end) sequence data from the cleaned alignment when a multi-analysis is performed using both illumina and minion sequences. The `{}.snippy_core_alignment.iqtree.treefile`, which is generated before the cleaning steps, will contain all SEQIDS.

### Should I use Snippy or SNVPhyl?

...This depends on your goal...
 
Snippy appears to be faster than SNVPhyl. It also allows use of both paired-end and single-end raw data files in the same analysis (whereas SNVPhyl **currently does not**).

A comparison of snippy vs SNVPhyl has not been conducted by our lab. The [SNVPhyl publication](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5628696/) does briefly discuss snippy.

### Version
Snippy version 4.6.0 (as of 2024-07-03)



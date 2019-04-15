# SNVPhyl

### What does it do?

SNVPhyl is a pipeline developed by the Public Health Agency of Canada for evaluating the number of SNPs between a
reference strain and other closely related strains. It also builds a phylogenetic tree to attempt to show the
relatedness of these strains. Lots more info can be found at the [SNVPhyl readthedocs site](https://snvphyl.readthedocs.io/en/latest/).

### How do I use it?

#### Subject

In the `Subject` field, put `SNVPhyl`. Spelling counts, but case sensitivity doesn't.

#### Description

The first line of your description needs to be `reference`, and the second line the SEQID of the strain you want to act
as your reference strain. Ideally, you'll want to pick a high-quality assembly for your reference.

If you wish to attach a reference file instead of providing a SEQID, the second line must be `attached`

The third line of your description should be `compare`, and lines after that the SEQIDs for strains you want to compare
your reference to.

#### Example

For an example SNVPhyl, see [issue 12494](https://redmine.biodiversity.agr.gc.ca/issues/12494).

#### Interpreting Results

The zip file uploaded on SNVPhyl completion should contain 10 files. Important files are:

* snvMatrix.tsv: Shows the number of SNVs between every strain submitted.
* vcf2core.tsv: Shows how much of the genome was covered by the analysis (look at the `Percentage of all positions that are valid, included, and part of the core genome`
column in the `all` row). This should be at least 90 percent, or the strains you were comparing were probably too far apart
to get good results.
* phylogeneticTree.nwk: The phylogenetic tree created by SNVPhyl. If you want to view this tree, you can use a program such as
[FigTree](http://tree.bio.ed.ac.uk/software/figtree/) or a web-based viewer like [phylo.io](http://phylo.io).

Other files can also be important - see the [docs on SNVPhyl Output files](https://snvphyl.readthedocs.io/en/latest/user/output/)
for more information.

### How long does it take?

Most SNVPhyl requests take ~1 hour to complete. If you submit a request for a larger SNVPhyl (>30 strains), it may take
substantially longer.

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) Strains too far apart. SNVPhyl requires that the strains you want to compare to the reference be closely related to
the reference. If you ask for a SNVPhyl with things that are not very related, you will get a warning telling you so.

3) No output files. Sometimes, SNVPhyl will say it has completed, but the typical output files will not be present. This
is either because _a)_ there are no SNVs between the two strains, and so SNVPhyl crashes or _b)_ SNVPhyl crashed for an
unknown reason, which does happen occasionally. If this happens, your best bet is to try running the SNVPhyl again. If
SNVPhyl keeps crashing even after subsequent attempts, let us know and we'll do our best to fix things.

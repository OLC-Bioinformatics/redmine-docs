# DiversiTree

### What does it do?

DiversiTree is used for creating phylogenetic trees using [parsnp](http://harvest.readthedocs.io/en/latest/content/parsnp.html),
and optionally finding a specified number of strains that represent the most genetic diversity.

### How do I use it?

#### Subject

In the `Subject` field, put `DiversiTree`. Spelling counts, but case sensitivity doesn't.

#### Description

The first line of the description should be the number of diverse strains you want picked. If all you want here is the
tree and don't care about the number of strains, you can set this to 1. Subsequent lines should be the SEQIDs you want
to create the tree from. Note that these strains should all be fairly closely related, or the tree creation may fail (so
try to keep things to the same species).

#### Example

For an example DiversiTree, see [issue 12100](https://redmine.biodiversity.agr.gc.ca/issues/12100).

#### Interpreting Results

DiversiTree will upload two files to Redmine on completion: `strains.txt`, and `tree.nwk`. The `strains.txt` file contains the
_X_ most diverse strains, where _X_ is the number specified in the first line of the description. The `tree.nwk` file
contains the phylogenetic tree created in newick format. If you want to view this tree, you can use a program such as
[FigTree](http://tree.bio.ed.ac.uk/software/figtree/) or a web-based viewer like [phylo.io](http://phylo.io).

### How long does it take?

This depends largely on the number of strains you want to use to create the tree. It can often be as quick as a few minutes
if you have 10 or less strains, or take several hours if you want a larger (100 or so strain) tree.

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) Strains too far apart. DiversiTree will warn you if it thinks that the strains you want to make a tree from are
not closely related enough. If you get this warning, you may want to consider creating a new issue while leaving out
the strains it says are too far.

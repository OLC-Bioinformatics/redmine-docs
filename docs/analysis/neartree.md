# NearTree

### What does it do?

NearTree will take a query SEQID, and then find the _X_ closest strains to it out of a list specified by creating a
phylogenetic tree and looking for the tips that are closest to your query strain.

### How do I use it?

#### Subject

In the `Subject` field, put `NearTree`. Spelling counts, but case sensitivity doesn't.

#### Description

The first line of the description should be the number of closest strains you want to your query strain.
The second line must say `query`, and the third line should be the query SEQID you want to find close relatives for.
The fourth line must say `reference`, and every line after that should be a SEQID you want to compare your query to.

#### Example

For an example NearTree, see [issue 12940](https://redmine.biodiversity.agr.gc.ca/issues/12940).

#### Interpreting Results

Upon completion, you'll be given a list of the SEQIDs that are closest to the query specified. The most closely related
will be listed first, with each SEQID listed after that becoming slightly more distant.

### How long does it take?

This depends largely on the number of strains you want to use to create the tree. It can often be as quick as a few minutes
if you have 10 or less strains, or take several hours if you want a larger (100 or so strain) tree.

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) Strains too far apart. NearTree will warn you if it thinks that the strains you want to make a tree from are
not closely related enough. If you get this warning, you may want to consider creating a new issue while leaving out
the strains it says are too far.

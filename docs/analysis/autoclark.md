# AutoCLARK

### What does it do?

This process runs CLARK, a metagenomics tool, to determine what species are present in a sample. This is useful if you
are unsure what species your sample is, or to check to see if any cross-species contamination occurred (or, obviously,
if you have a shotgun metagenomics sample). Lots of detail on CLARK is provided at the [CLARK website](http://clark.cs.ucr.edu/).

### How do I use it?

#### Subject

In the `Subject` field, put `AutoCLARK`. Spelling counts, but case sensitivity doesn't.

#### Description

In the `Description` field first specify if you want CLARK to look at raw reads or draft assemblies for species
determination. For reads, the first line of your description should be `fastq`, and for assemblies it should be `fasta`.
Subsequent lines should be the SEQIDs you want CLARK to be looking at.

#### Example

For an example AutoCLARK, see [issue 12819](https://redmine.biodiversity.agr.gc.ca/issues/12819).

#### Interpreting Results

Once CLARK is complete, a file called `abundance.xlsx` will be uploaded to Redmine. This file shows the species present
for each strain in the request. These results should be interpreted with caution - species that show up with low
proportions (less than 1-2 percent) are often not actually there and are just artifacts of the analysis.

### How long does it take?

CLARK will usually take 10 to 15 minutes to run, though it may take substantially longer if you requested that a large
number of SEQIDs be analyzed.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.


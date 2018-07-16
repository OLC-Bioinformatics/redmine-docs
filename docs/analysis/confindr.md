# ConFindr

### What does it do?

ConFindr looks for both intra-species and inter-species contamination in raw reads, which can cause misassemblies and
erroneous downstream analysis. More details on ConFindr can be found [on GitHub](https://lowandrew.github.io/ConFindr/).

### How do I use it?

#### Subject

In the `Subject` field, put `ConFindr`. Spelling counts, but case sensitivity doesn't.

#### Description

All you need to put in the description is a list of SEQIDs you want to detect contamination in, one per line.

#### Example

For an example ConFindr, see [issue 12881](https://redmine.biodiversity.agr.gc.ca/issues/12881).

#### Interpreting Results

When your request is complete, a file called `confindr_output.zip` will be uploaded. This contains two files: `confindr_log.txt`,
and `confindr_report.csv`. The report file contains 5 columns:

* Sample: The name of the sample
* Genus: What ConFindr thinks the genus of the sample is. If ConFindr finds more than one genus, both will be listed here.
* NumContamSNVs: How many times ConFindr found something that looked like a contaminating SNV. Anything 3 or over
is enough to call a sample as contaminated.
* NumUniqueKmers: How many unique kmers were found for the rMLST genes. If this number is over 45000, a sample is called
as contaminated.
* ContamStatus: Shown as `True` if ConFindr thinks a sample is contaminated, and `False` if the sample is thought to be
clean.

The log file can mostly be ignored - if any unexpected errors come up, we may use it for debugging purposes.

### How long does it take?

ConFindr will take between 1 and 2 minutes for each sample.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.
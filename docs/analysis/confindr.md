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

### How long does it take?

ConFindr will take between 1 and 2 minutes for each sample.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.
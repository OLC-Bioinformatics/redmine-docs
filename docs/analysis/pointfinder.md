# PointFinder

### What does it do?

PointFinder is a program complementary to ResFinder developed by the Danish Center for Genomic Epidemiology
for detection of antibiotic resistance in draft genome assemblies. Unlike ResFinder, PointFinder looks for point
mutations that are known to confer antibiotic resistance.

### How do I use it?

#### Subject

In the `Subject` field, put `PointFinder`. Spelling counts, but case sensitivity doesn't.

#### Description

All you need to put in the description is a list of SEQIDs you want to detect AMR in, one per line.
Note that PointFinder is limited to detecting mutations in a few genera:

* Campylobacter
* Escherichia
* Mycobacterium
* Neisseria
* Salmonella

Our PointFinder automator will automatically determine the genus of your requested SEQIDs, and will not attempt to
analyze any SEQIDs that are not from one of these genera.

#### Example

For an example PointFinder, see [issue 12933](https://redmine.biodiversity.agr.gc.ca/issues/12933).

#### Interpreting Results

PointFinder will upload a zip file called `pointfinder_output.zip`. Within this folder, you will find 3 files per
SEQID:

**_Note: Any SEQIDs that had no resistance detected will not have any files uploaded_**

* SEQID\_blastn\_PointFinder\_prediction.txt: Has each antibiotic for with known mutations for the genus of interest.
Anything with a 0 had no detected mutations, anything other had predicted point mutation(s) for that antibiotic.
* SEQID\_blastn\_PointFinder\_results.txt: A file listing the genes that had mutations, what they were, and what resistance those
mutations confer.
* SEQID\_blastn\_PointFinder\_table.txt: Much the same as the \_results.txt file, but also lists known AMR genes that
did not have point mutations.

### How long does it take?

PointFinder should take 30 seconds to 1 minutes for analysis of each sample.

### What can go wrong?

1) Requested SEQIDs are not available: If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) Can't run on requested genera: If the SEQIDs you request are not one of the genera that PointFinder works on,
you will get a message saying so. PointFinder will still run on any SEQIDs specified that are from the correct genera.


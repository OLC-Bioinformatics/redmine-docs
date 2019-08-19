# StarAMR

### What does it do?

StarAMR is a program from Canada's National Microbiology Laboratory that combines the ResFinder
and PointFinder programs for detecting antibiotic resistance from the [Danish Center for Genomic Epidemiology](http://www.genomicepidemiology.org/).

### How do I use it?

#### Subject

In the `Subject` field, put `staramr`. Spelling counts, but case sensitivity doesn't.

#### Description

All you need to put in the description is a list of SEQIDs you want to detect AMR in, one per line.
Note that StarAMR is limited to detecting mutations in a few genera:

* Campylobacter
* Salmonella

Our StarAMR automator will automatically determine the genus of your requested SEQIDs, and will not attempt to
analyze any SEQIDs that are not from one of these genera.

#### Example

For an example StarAMR, see [issue 15625](https://redmine.biodiversity.agr.gc.ca/issues/15625).

#### Interpreting Results

StarAMR will upload a zip file called `staramr_output.zip`. Within this folder, you will find a few files for each genus:

**_Note: Any SEQIDs from genera that are not Campylobacter or Salmonella not have any files uploaded_**

The most important file for each genus is `results.xlsx` - this shows what AMR genes were found in each
SeqID, and also lets you see if the resistance is conferred by a gene or a point mutation.

### How long does it take?

StarAMR should take 30 seconds to 1 minutes for analysis of each sample.

### What can go wrong?

1) Requested SEQIDs are not available: If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) Can't run on requested genera: If the SEQIDs you request are not one of the genera that PointFinder works on,
you will get a message saying so. PointFinder will still run on any SEQIDs specified that are from the correct genera.


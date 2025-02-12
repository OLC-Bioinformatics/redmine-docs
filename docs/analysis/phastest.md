## PHASTEST

### What does it do?

PHASTEST (PHAge Search Tool with Enhanced Sequence Translation) performs identification, annotation and visualization of prophage sequences within bacterial genomes and plasmids. PHASTEST also supports extensive annotation and interactive visualization of all other genes (protein coding regions, tRNA sequences and rRNA sequences) in those same genomes.

### How do I use it?

#### Subject

In the `Subject` field, put `phastest`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

In the `Description` field, you must provide:

1. a list of SEQIDs (one per line)

**Optional Components**

In order to customise your PHASTEST analyses, two settings can be optionally modified

- Annotation mode. `deep` uses Prophage Database and PHAST-BSD Bacterial Database. `lite` uses Prophage Database and Swissprot. (Note: 'deep' mode may take significantly longer to complete.)
    - default is `lite`
    - modify as follows:
        - `deep`
- Only annotate phage region.
    - default is `disabled`
    - modify as follows:
        - `phage-only`

#### Example

For an example PHASTEST issue, see [issue 35681](https://redmine.biodiversity.agr.gc.ca/issues/35681). NOTE: this is an older example, and the output file is missing the issue number.

#### Interpreting Results

The PHASTEST automator will upload a file called `phastest_output_$ISSUENUMBER.zip` once it has completed. This file will contain all the outputs generated.

### How long does it take?

It depends on the number of SEQIDs requested. The PHASTEST pipeline doesn't seem to be multi-threaded, so it takes about an hour per sample. I have added some parallelization, but this simply processes multiple SEQIDs concurrently. There is a limit of 20 CPUs allocated to this automator, so if you specify 20 or fewer SEQIDs, the analysis will take an hour, 21-40 SEQIDs will take two hours, etc.

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it (NOTE: this does not mean that the analysis is not going to occur).

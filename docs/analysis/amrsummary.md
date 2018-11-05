# AMRsummary

### What does it do?

AMRsummary performs ResFinder and MOB-suite analyses on FASTA files, and creates a report combining and summarising the outputs.

ResFinder is a program developed by the Danish Center for Genomic Epidemiology for detection of antibiotic resistance
in draft genome assemblies. It is very important to note that the Redmine version will only look for acquired antibiotic
resistance genes (generally plasmid-borne) and not chromosomally encoded AMR genes that are caused by point mutations.

MobSuite is a set of tools developed by the Public Health Agency of Canada for detecting plasmids in draft genome
assemblies. This tool runs the `mob_recon` part of the suite, which first detects plasmids in the assemblies, and then
performs typing on the plasmids. More details on MobSuite, including fairly extensive details on the output files
produced, can be found at the [MobSuite GitHub repository](https://github.com/phac-nml/mob-suite)).

### How do I use it?

#### Subject

In the `Subject` field, put `amr summary`. Spelling counts, but case sensitivity doesn't.

#### Description

All you need to put in the description is a list of SEQIDs you want to process, one per line.

#### Example

For an example AMRsummary, see [issue 14100](https://redmine.biodiversity.agr.gc.ca/issues/14100).

#### Interpreting Results

AMRSummary will upload three separate reports once it is complete

- `resfinder_blastn.xlsx` shows every AMR gene found in each sample. Just because a gene/resistance is listed here does not necessarily mean the strain carries that resistance - it's important
to look at the __PercentIdentity__ and __PercentCovered__ columns. You can be pretty sure that anything with 100 for both
is actually there, but anything else requires further analysis to be sure.
- `mob_recon_summary.csv` shows any contigs that are predicted to be plasmids - note that all contigs calculated to be chromosomal are ignored. __Location__ is the name of the predicted plasmid,
while __Contig__ is the name contig predicted to contain plasmid sequence. One plasmid can be composed of several contigs if it could not be circularised.
- `amr_summary.csv` combines the two previous reports. The contigs of all predicted AMR genes from the `resfinder_assembled` report are used to search the `mob_recon_summary` report.
The plasmid predictions, and well as all the incompatibility types for that plasmid are extracted, and used in the report. __Location__ will specify either `chromosome` or
the name of the predicted plasmid.

### How long does it take?

ResFinder is very fast, while MOB-suite is relatively slow - it should take a few minutes to analyze each SEQID requested.

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.


# Plasmid-Borne Identity
![Plasmid-Borne Identity](../img/plasmid_borne_identity.png)

### What does it do?

Plasmid-Borne Identity performs GeneSeekr and MOB-suite analyses on FASTA files, and creates a report combining and 
summarising the outputs.

MobSuite is a set of tools developed by the Public Health Agency of Canada for detecting plasmids in draft genome
assemblies. This tool runs the `mob_recon` part of the suite, which first detects plasmids in the assemblies, and then
performs typing on the plasmids. More details on MobSuite, including fairly extensive details on the output files
produced, can be found at the [MobSuite GitHub repository](https://github.com/phac-nml/mob-suite)).

### How do I use it?

#### Subject

In the `Subject` field, put `plasmid_borne_identity`. Spelling counts, but case sensitivity doesn't.

#### Description

All you need to put in the description is a list of SEQIDs you want to process, one per line.


#### Attachments

You are required to attach a FASTA-formatted file containing the gene(s) you wish analysed 


#### Optional arguments

- BLAST program. **NOTE:** GeneSeekr (and therefore PlasmidBorne Identity) does not check to see if your query or 
database are the appropriate molecule for the requested program. 
    - default is `blastn`
    - You can select one of the following BLAST programs to use:
        - blastn - nt query: nt db
        - blastp - protein query: protein db
        - blastx - translated nt query: protein db
        - tblastn - protein query: translated nt db
        - tblastx - translated nt query: translated nt db
    - modify as follows:
        - `blast=tblastx`
- Minimum cutoff for matches to be included in report.
    - default is `70`
    - modify as follows:
        - `cutoff=80`
- E-value cutoff
    - default is `1E-05`
    - modify as follows:
        - `evalue=1E-10` or
        - `evalue=0.01`

#### Example

For an example Plasmid-Borne Identity analysis, see [issue 15644](https://redmine.biodiversity.agr.gc.ca/issues/15644).

#### Interpreting Results

Plasmid-Borne Identity will upload five separate reports once it is complete

`geneseekr_{BLAST_PROGRAM}.xlsx` strain name and percent identity match for all query genes

`geneseekr_{BLAST_PROGRAM}_detailed.csv` strain name, and BLAST summary information, including percent match, alignment length, 
subject length, e-value, number of positives, number of mismatches, and number of gaps for every gene

`geneseekr_{BLAST_PROGRAM}.csv`  same as `geneseekr_{BLAST_PROGRAM}.csv`  same as, but in .csv format

`mob_recon_summary.csv` shows any contigs that are predicted to be plasmids - note that all contigs calculated to be 
chromosomal are ignored. __Location__ is the name of the predicted plasmid, while __Contig__ is the name contig 
predicted to contain plasmid sequence. One plasmid can be composed of several contigs if it could not be circularised.

`plasmid_borne_summary.csv` combines information from `geneseekr_{BLAST_PROGRAM}.csv`  and `mob_recon_summary.csv`. 
The contigs of all predicted AMR genes from the `geneseekr_{BLAST_PROGRAM}_detailed.csv` report are used to search 
the `mob_recon_summary` report. The plasmid predictions, and well as all the incompatibility types for that plasmid 
are extracted, and used in the report. __Location__ will specify either `chromosome` or the name of the predicted plasmid.

### How long does it take?

GeneSeekr is very fast, while MOB-suite is relatively slow - it should take a few minutes to analyze each SEQID requested.

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.
1. Issue with required FASTA-formatted targets file, including not attaching the file, or the FASTA formatting being incorrect
1. Specifying the incorrect BLAST analysis program for the provided sequences e.g. blastp with a nucleotide query and db

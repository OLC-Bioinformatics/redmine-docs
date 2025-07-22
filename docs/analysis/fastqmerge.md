# fastqmerge

### What does it do?

fastqmerge is a tool for merging the raw sequence data. This tool merges/cat the multiple raw sequence fastq ids and writes merged sequences in /mnt/nas2/raw_sequence_data/merged_sequences. Technically 

merge R1 from different runs of same sample into merged R1 and merge R2 from different runs of same sample into merged R2 for illumina seqs. For Minion, it combines multiple MIN ids into single merged 

MINion seq. At the end it shows the file where it is written along with the merged file sizes. 



### How do I use it?

#### Subject

In the `Subject` field, put `fastqmerge`. Spelling counts and case sensitive. Input the Seq ids and provide wether it is singlefastq or paired in the description along with the new seqID for saving the merged sequence. user can run on singlefastq with MINIOn or illumina or both, example is shown in the below issue.

#### Description

**Required Components**

You must include a list of SEQIDs with comma separated and need to provide the new name( seqids) for the merged sequence.


#### Example

For an example FastQC analysis, see [issue 36575](https://redmine.biodiversity.agr.gc.ca/issues/36575).

#### Interpreting Results

after merging the sequences, the sequences are saved into the merged_sequences in the nas2 and accessible for further analysis.

### How long does it take?

fastqmerge is usually takes 3 mins combining two MIN sequences to single merged MIN sequences. The time required for analysis will depend on the number of sequences requested.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request or new ids not provided,  you will get a warning message informing you of it.




# fastqmerge

### What does it do?

fastqmerge is a utility designed to consolidate multiple raw sequencing FASTQ files originating from different sequencing runs of the same biological sample. It streamlines downstream analysis by producing merged sequence files. For illumina data: Merges multiple R1 FASTQ files from different sequencing runs of the same sample into a single merged R1 file.Similarly, merges R2 FASTQ files into a single merged R2 file. Minion data: Combines multiple FASTQ files from various MinION run IDs into a single merged FASTQ file. After merging this tool reports , output file paths and the sizes of the merged files for verification.


### How do I use it?

#### Subject

In the `Subject` field, enter `fastqmerge` (case-sensitive and spelling matters). In the description, provide the list of SEQIDs and indicate whether the data type is singlefastq or paired. Also include the new output SEQID that will be used to save the merged sequence. The tool supports merging singlefastq files from MinION or paired end froIllumina, or can provide both. An example is shown in the issue below.

#### Description

**Required Components**

A comma-separated list of SEQIDs must be provided, followed by the new merged SEQID after an equals sign (=) for the merged sequence.


#### Example

For an example fastqmerge analysis, see [issue 36575](https://redmine.biodiversity.agr.gc.ca/issues/36575).

#### Interpreting Results

after merging the sequences, the sequences are saved into the merged sequences in the nas2 and accessible for further analysis.

### How long does it take?

fastqmerge is usually takes 3 mins combining two MIN sequences to single merged MIN sequences. The time required for analysis will depend on the number of sequences requested.

### What can go wrong?

Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request or new MER ids not provided,  you will get an error message informing you of it.


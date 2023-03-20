# FastQC/MultiQC

### What does it do?

FastQC is a tool for conducting quality control checks on raw sequence data. MultiQC is a tool to aggregate FastQC reports from multiple sequences into a single file/report.

For more information, see the [FastQC website](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/), and [MultiQC website](https://multiqc.info/).

### How do I use it?

#### Subject

In the `Subject` field, put `fastqc`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

You must include a list of SEQIDs one per line.


#### Example

For an example FastQC analysis, see [issue 30311](https://redmine.biodiversity.agr.gc.ca/issues/30311). The zip file has been attached to this request as an example (the ftp links expire after approx. 2 weeks).

#### Interpreting Results

FastQC/MultiQC will upload the multiqc report and a zipped folder called `fastqc_redmineID.zip` to both redmine and the ftp once it has completed. This will contain report files for individual sequences, and the multiqc report. (Files are uploaded to both redmine and the ftp in case a large number of sequences are analysed, and therefore reports are too large to be uploaded directly to redmine).

### How long does it take?

FastQC is usually pretty fast, <1 minute per sequence. The time required for analysis will depend on the number of sequences requested.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.



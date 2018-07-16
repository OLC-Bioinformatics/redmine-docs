# WGS Assembly

### What does it do?

WGS Assembly will take an Illumina MiSeq run and put it through the CFIA OLC Workflow for Bacterial Assembly and Typing (COWBAT).

### How do I use it?

#### Before Redmine

Before submitting your request to Redmine, you need to upload your sequence data. Your sequence data should be in a folder
named YYMMDD_LAB, where YYMMDD is the date sequencing was done, and LAB is a 3-letter code for your lab. For example, a
sequencing run from Calgary done on July 16, 2018 would be 180716_CAL.

This folder needs to have the following files:

* SampleSheet.csv
* CompletedJobInfo.xml
* config.xml
* GenerateFASTQRunStatistics.xml
* RunInfo.xml
* runParameters.xml
* the InterOp folder
* A forward and reverse set of reads for each SEQID specified in your SampleSheet.csv

To upload this folder, navigate to `ftp://ftp.agr.gc.ca/incoming/cfia-ak` in a file browser and drag and drop the folder
to upload. Upload speed will vary depending on your connection. Once your upload is complete, you can head to Redmine
to submit your issue.

#### Subject

In the `Subject` field, put `WGS Assembly`. Spelling counts, but case sensitivity doesn't.

#### Description

The only thing to put in your description is the name of the folder you created and uploaded.

#### Example

For an example WGS Assembly, see [issue 12782](https://redmine.biodiversity.agr.gc.ca/issues/12782).

### How long does it take?

WGS Assembly will likely take 5-6 hours to complete, although it can be as fast as 2 hours or as slow as 8.

### What can go wrong?

A few things can go wrong with this process:

1) FTP upload fails: Your upload to the FTP may time out or otherwise fail. If this happens, you will need to send us an
email so we can clear out the previous upload attempt before attempting to upload again.

2) Validation fail: We validate a number of things about the files that are submitted before starting assembly, including
making sure that all SEQIDs specified in the SampleSheet are uploaded, that the FASTQ files are appropriately sized, and
that all files required are present. If any of these validation checks fail, send us an email and we'll get things sorted
out.

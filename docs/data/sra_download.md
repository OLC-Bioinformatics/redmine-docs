# SRA Downloads

### What does it do?

SRA Download downloads fastq files from the SRA, saves them on the NAS, uploads them to Azure storage and then assembles them with COWBAT on FoodPort.

### How do I use it?

#### Subject

In the `Subject` field, put `sradownload`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required components:**

 - In the first line include the run name `run_name=[YYMMDD-sra]`. This should be the name of the container in Azure storage and the run name in FoodPort
 - The last line(s) should be a list of SRA ID's you would like to download (one per line)
 
**Optional components:**

 - `email=email_address` An email address may be supplied to Foodport so that the user can be notified when the assembly is complete. eg. `email=olcbioinforatics@gmail.com`
 - `basic_assembly` This command makes it so COWBAT only performs assembly, but does not run any typing modules
 - `preprocess` This command makes it so COWBAT only performs quality assessments and read trimming/error correcting. COWBAT will not perform any assembly or typing

#### Example

For an example of an SRA download, see [issue 34216](https://redmine.biodiversity.agr.gc.ca/issues/34216).

### How long does it take?

The run should approximately take about 15-20 minutes per SRA ID.

### What can go wrong?

Requested SRA IDs are not available. If we can't find some of the SRA IDs that you request, you will get a warning message informing you of it.

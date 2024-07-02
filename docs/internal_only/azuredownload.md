# Azuredownload

### What does it do?

Merge will take data from multiple MiSeq runs of the same strain and combine the FASTQ files in order 
to produce a better assembly. 

### How do I use it?

#### Subject

In the `Subject` field, put `azuredownload`. Spelling counts, but case sensitivity doesn't.

#### Description


In the `Description` field, the required components are the following:

First line: `machine=other`  
Next line: `location=ncbi`  
Next line: `sequence_folder=folder name`

The 'folder name' should be replaced with the name of the sequence folder used for assembly on [Foodport](http://10.148.57.4/). 

**Important note:** even if you uploaded the run to Foodport using an underscore "_" in the name, do not use an underscore in the redmine automator. It must be a dash:

`sequence_folder=YYMMDD-lab`

You must also provide the requested/used machine type as follows `machine=miseq`

Azuredownload supports the following analyses:


- `miseq` - used for MiSeq sequencer runs assembled on Foodport
- `nextseq` - used for NextSeq sequencer runs assembled on Foodport
- `other` - used for any "other" data assembled on Foodport.
	- For example data downloaded from NCBI and uploaded to Foodport to be assembled with our pipeline. Or random research assemblies done through Foodport.
	- The user must also add the `flag location=` and the name of the location the sequence assemblies are to be stored on the nas
	
Possible flag locations include:

- `atcc` - for ATCC assemblies 
- `enterobase` - for enterobase sequences
- `ncbi` - raw data from SRA or NCBI
- `merged` - any merged sequence data
- `refseq` - refseq sequences
- `other` - anything that does not fit in these categories (eg. research samples)

An example file can be found with the example issue below.

#### Example

For an example Azuredownload, see [issue 34137](https://redmine.biodiversity.agr.gc.ca/issues/34137).

#### Interpreting Results

A file named `legacy_combinedMetadata.csv` will be returned containing a table of information about each equence in the assembly folder.

### How long does it take?

Az will take quite a while, depending on how many samples you have. Probably roughly half an hour per sample.

### What can go wrong?

1) Sequence folder cannot be found. Check spelling of the file in description and ensure that you are using dashes instead of underscores. 

2) Sequence folder cannot be found. Do not download the assembly folder from Foodport. After the COWBAT run is finished it will automatically be placed in the correct location on the nas. If you download a replicate an error may occur.

If the upload is not done correctly you will get a warning message informing you of it.


# External Retrieve

### What does it do?

External retrieve is a process that will upload data you request (either raw reads or draft assemblies) to an FTP site
so that you can download it in the event that you need to have your files locally available.

### How do I use it?

#### Subject

In the `Subject` field, put `External Retrieve`. Spelling counts, but case sensitivity doesn't.

#### Description

The first line of your description tells the process whether you want to rename the retrieved files for upload to
the SRA (e.g. 2014-SEQ-0349_S11_L001_R1_001.fastq.gz renamed to 2014-SEQ-0349_R1.fastq.gz). Note that this option requires 
FASTQ files to be retrieved (see below)


The next line specifies whether you want raw reads or draft assemblies. For reads,
the first line should be `fastq`, and for assemblies the first line should be `fasta`. 

Every line after that should be a SEQID that you want the data for.

#### Example

For an example of an External Retrieve, see [issue 12822](https://redmine.biodiversity.agr.gc.ca/issues/12822) or
SRA formatting [issue 18760](https://redmine.biodiversity.agr.gc.ca/issues/18760).

### How long does it take?

If your request is for a small number of files, it will generally be done within a few minutes. The more files requested,
the longer the request will take.

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) FTP timeout. Sometimes, particularly for larger requests, the upload to the FTP will run into problems and time out,
in which case you will likely get an error message similar to this: `[Errno 104] Connection reset by peer`. If this occurs,
you can either try again later, or, if you had a large request, try splitting it into a few smaller requests. If the
problem persists, send us an email and we'll try to get it figured out.

###Alternatives to Redmine

For when redmine is being mean:
<details>
	<summary>Nastools.py</summary><br>

	<p>Note: Must be done on a linux computer connected to the nas</p><br>
	
	
	<ol>

	<li>Open a text document and copy paste your SEQID’s in it</li>
	<li>Download the text document into a folder</li>
	<li>Open the folder in the terminal

		<ul>
		<li> Enter <code>cd /mnt/[.../your_folder_name]</code> in the terminal OR</li>
		<li> Right click on your folder in files and select “open in terminal”</li>
		</ul></li>

	<li>In the terminal type <code>nastools.py gedit [name_of_SEQID_doc]</code></li>

	<li>Then enter “nastools.py” and the relevant commands:
	
	<ul>
		<li><code>-- file [name_of_SEQID_doc]</code>: sequences to retrieve</li>
		<li><code>-- type [fasta or fastq]</code>: what type of file you want to retrieve</li>
		<li><code>-- outdir [new_folder_name]</code>: where the output files will be located</li>
		<li><code>-- copy</code>: will create copies instead of links to your files <b>(OPTIONAL)</b></li>
	</ul></li>

	<li>This will then either create symbolic links (links that lead to the file location of desired files), or copies of the files requested if you added <code>--copy</code></li>
	
	</ol>

</details>

<details>
	<summary>Foodport</summary><br>
	
	<ol>
	
	<li>Scroll down on the Home Page and click the <code>File Zone</code> button</li>

	<li>Click <code>Locate Files</code></li>

	<li>Under the text box <code>Full or Partial File Name(s). One per line</code> enter your SEQIDs on separate lines</li>

	<li>Do not close or refresh your tab when the run is in progress</li>

	<li>From FileZone Home your run should be the latest run that has the blue bar “processing” (it’ll automatically be called <code>regexes-##</code>)</li>

	<li>Click the gray button <code>File Outputs</code> and click the download button(s) for the files you would like to download</li>
	
	</ol>
	
	</details>
<br>


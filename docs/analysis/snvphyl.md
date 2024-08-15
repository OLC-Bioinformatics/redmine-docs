# SNVPhyl

### What does it do?

SNVPhyl is a pipeline developed by the Public Health Agency of Canada for evaluating the number of SNPs between a
reference strain and other closely related strains. It also builds a phylogenetic tree to attempt to show the
relatedness of these strains. Lots more info can be found at the [SNVPhyl readthedocs site](https://snvphyl.readthedocs.io/en/latest/).

### How do I use it?

#### Subject

In the `Subject` field, put `SNVPhyl`. Spelling counts, but case sensitivity doesn't.

#### Description

The first line of your description needs to be `reference`, and the second line the SEQID of the strain you want to act
as your reference strain. Ideally, you'll want to pick a high-quality assembly for your reference.

If you wish to attach a reference file instead of providing a SEQID, the second line must be `attached`

The third line of your description should be `compare`, and lines after that the SEQIDs for strains you want to compare
your reference to.

#### Example

For an example SNVPhyl, see [issue 12494](https://redmine.biodiversity.agr.gc.ca/issues/12494).

#### Interpreting Results

The zip file uploaded on SNVPhyl completion should contain 10 files. Important files are:

* snvMatrix.tsv: Shows the number of SNVs between every strain submitted.
* vcf2core.tsv: Shows how much of the genome was covered by the analysis (look at the `Percentage of all positions that are valid, included, and part of the core genome`
column in the `all` row). This should be at least 90 percent, or the strains you were comparing were probably too far apart
to get good results.
* phylogeneticTree.nwk: The phylogenetic tree created by SNVPhyl. If you want to view this tree, you can use a program such as
[FigTree](http://tree.bio.ed.ac.uk/software/figtree/) or a web-based viewer like [phylo.io](http://phylo.io).

Other files can also be important - see the [docs on SNVPhyl Output files](https://snvphyl.readthedocs.io/en/latest/user/output/)
for more information.

### How long does it take?

Most SNVPhyl requests take ~1 hour to complete. If you submit a request for a larger SNVPhyl (>30 strains), it may take
substantially longer.

<details>
  <summary><b>How to check if your SNVPhyl will fail using Galaxy Docker</b></summary> <br><ol>
  
<li>Log into the head node by typing the following in the terminal
<code>ssh ubuntu@head</code> or <code>ssh ubuntu@192.168.1.5</code></li>

<li>If prompted for a password enter the standard bioinformatics password.</li>

<li>Enter <code>watch squeue</code>. Under the column <code>NAME</code> there will be a list of biorequests running. In the same row as your biorequest note your Job ID and the node in which your issue is running under <code>JOBID</code> and <code>NODELIST</code>.</li>

<li>On a web browser of your choice search <code>http://<b>[IP_address]</b>.<b>[node#]</b>:<b>[jobID]</b></code>. (Eg. for a job on node 03 with the ID 34595 the url would be: <u>http://[IP_address].3:34595/</u>)
	<ul>
	<li>Please ask a bioinformatician or Cathy for the IP address.</li>
	</ul>
</li>

<li>Under the tab user cick log in and login with the following credentials:<b> user: admin@galaxy.org; password: admin.</b></li>

<li>On the right there will be a tab labeled <code>history</code> that will display all the steps the the SNVPhyl runs. If there are many tabs with red x's it will likely fail.</li>

</ol></details><br>


### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) Strains too far apart. SNVPhyl requires that the strains you want to compare to the reference be closely related to
the reference. If you ask for a SNVPhyl with things that are not very related, you will get a warning telling you so.

3) No output files. Sometimes, SNVPhyl will say it has completed, but the typical output files will not be present. This
is either because:

&nbsp;&nbsp;&nbsp;&nbsp;a. there are no SNVs between the two strains, and so SNVPhyl crashes. If you want to confirm that there are no SNVs between your two strains you can add a third strain to your description that is related enough to run (eg. try a strain with the same rMLST) but may have variants.  
&nbsp;&nbsp;&nbsp;&nbsp;b. SNVPhyl crashed for an unknown reason, which does happen occasionally. If this happens, your best bet is to try running the SNVPhyl again.

  If SNVPhyl keeps crashing even after subsequent attempts, let us know and we'll do our best to fix things.
  

### Version
SNVPhyl on redmine is version 1.0.1 with snvphyl_cli_version=1.3 (as of 2024-04-05)

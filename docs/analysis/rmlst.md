# rMLST

### What does it do?

####rMLST

​​The automator runs rMLST analyses on provided SEQIDs. If the rMLST database for the requested genus is not present on the NAS, or an update is requested, the automator downloads and prepares the database. It also runs GeneSeekr to perform rMLST analyses. GeneSeekr rMLST reports are uploaded and attached to the issue​.

### How do I use it?

#### Subject and description

In the `Subject` field, put `rmlst`. Spelling counts, but case sensitivity doesn't.

You can also choose to update the OLC database by entering `update` on the next line.

Then add the SEQID's on separate lines to match the rest of the document.


#### Example

For an example rMLST issue see [issue 34424](https://redmine.biodiversity.agr.gc.ca/issues/34424).

#### Interpreting Results

The Roary automator will upload a zip file `rmlst_output_[redmine_issue_num].zip` once it has completed. It may also say `Using database version: [YYMMDD]` in the comments if the `update` command was included in the description.

### How long does it take?

​It should take a few minutes for the automator to run depending on whether the database needs to be installed/updated. Adding more sequences will scale up the time required​.

### What can go wrong?

Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.

### Version

The databases can be updated by including the `update` argument in the description.



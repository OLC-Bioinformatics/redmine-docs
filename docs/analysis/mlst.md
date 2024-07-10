# MLST

### What does it do?

​​The automator runs MLST analyses on provided SEQIDs. If the MLST database for the requested genus is not present on the NAS, or an update is requested, the automator downloads and prepares the database. It also runs GeneSeekr to perform MLST analyses. GeneSeekr MLST reports are uploaded and attached to the issue​.

### How do I use it?

#### Subject

In the `Subject` field, put `mlst` spelling counts, but case sensitivity doesn't.

#### Description

Include the genus on the first line `organism=[name of organism]`. The organisms currently available for this automator are: 

- `Escherichia` 
- `Vibrio` 
- `Campylobacter` 
- `Listeria` 
- `Bacillus` 
- `Staphylococcus` 
- `Salmonella` 
- `Yersinia` 

You can also choose to update the OLC database by entering `update` on the next line.

Then add the SEQID's on seperate lines.

Note that fasta files can also be attatched to the issue and be processed. This can used in addition to or instead of SEQIDs. However, there is a 5MB limit to the attatched file enforced by Redmine

#### Example

For an example mlst see [issue 34215](https://redmine.biodiversity.agr.gc.ca/issues/34215)

#### Interpreting Results

If submitted correctly you will recieve a zip folder named `geneseekr_output_[issue num].zip` that contains a excel sheet you can download.

### How long does it take?

​It should take a minute or two for the automator to run depending on whether the database needs to be installed/updated. Adding more sequences will scale up the time required​ 

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.

2) Requested an unsupported genus; _Escherichia, Vibrio, Campylobacter, Listeria, Bacillus, Staphylococcus, Salmonella,_ and _Yersinia_ are the currently supported genera. 

### Version

The databases can be updated by including the `update` argument in the description 


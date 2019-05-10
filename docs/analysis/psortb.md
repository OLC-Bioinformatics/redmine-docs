# PSORTb

### What does it do?

PSORTb is a program developed by the Brinkman lab at Simon Fraser University - it attempts to figure out what
the subcellular location of proteins might be. If you wnat to know more, you can check out the 
[PSORTb publication](https://academic.oup.com/bioinformatics/article/26/13/1608/201357) or the 
[PSORTb documentation](https://www.psort.org/documentation/index.html). 

### How do I use it?

#### Subject

In the `Subject` field, put `PSORTb`. Spelling counts, but case sensitivity doesn't.

#### Description

All you need to put in the description is a list of SEQIDs you want to find protein localizations for, one per line.

#### Example

For an example PSORTb, see [issue 16061](https://redmine.biodiversity.agr.gc.ca/issues/16061).

#### Interpreting Results

PSORTb will upload a zip file - this will contain FASTA-formatted proteins as predicted and annotated by prokka, and 
a text file for each SEQID that has the predicted subcellular localization for each protein.

### How long does it take?

PSORTb is pretty slow - expect it to take at least 20 to 30 minutes per SEQID requested.

### What can go wrong?

1) Requested SEQIDs are not available: If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) PSORTb needs to know whether a strain is gram positive or gram negative - you'll see that each of the text files
you get have either `grampos` or `gramneg` in them, depending on whether or not we predicted that isolate to be gram 
positive or gram negative. We're fairly sure that our classification is fairly robust, but there might be exceptions.
If you put an isolate through and it's wrongly classified, email `andrew.low@canada.ca` and we'll get it sorted out!
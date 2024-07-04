# Prokka

### What does it do?

Prokka annotates genomes, letting you know what genes and other features are present. If you want to 
learn more about it, check out the [prokka publication.](https://www.ncbi.nlm.nih.gov/pubmed/24642063)

### How do I use it?

#### Subject

In the `Subject` field, put `Prokka`. Spelling counts, but case sensitivity doesn't.

#### Description

All you need to put in the description is a list of SEQIDs you want to process, one per line.

#### Example

For an example Prokka, see [issue 15195](https://redmine.biodiversity.agr.gc.ca/issues/15195).

#### Interpreting Results

Prokka will output a lot of files for each genome you give it - you can find a quick description of
each file [here](https://github.com/tseemann/prokka#output-files). Of particular interest are the `.gff` and `.gbk` files.

### How long does it take?

Prokka isn't the quickest thing around - expect it to take 2 to 3 minutes for each genome you give it.

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.
2. FTP Upload issues. Since the files prokka produces are fairly big, the need to be uploaded to an FTP site. We'll sometimes
have connection issues that stop uploads from happening properly. If this is the case, you'll get a message telling you to 
try again later. FTP connection issues usually only persist for a few hours at a time.

### Version
Prokka version on redmine is 1.14.6 (as of 2024-07-03)

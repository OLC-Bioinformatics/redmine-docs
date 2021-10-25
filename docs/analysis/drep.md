# dRep

### What does it do?

[dRep](https://drep.readthedocs.io/en/latest/overview.html), developed by Matt Olm, is used to compare genomes and determine relatedness in the form of average nucleotide identity (ANI).
Optionally, it can be used to dreplicate genomes.

For more information, see the [dRep publication](https://www.nature.com/articles/ismej2017126). If you publish data using this automator, don't forget to cite the authors of the tool [Olm et al., 2017](https://www.nature.com/articles/ismej2017126)

### How do I use it?

#### Subject

In the `Subject` field, put `drep`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

The first line of the description should be the analysis you would like to run (e.g. `analysis=custom`). 

- The following options are currently supported:
    - `custom` - compare only the SEQIDs listed in the description
    - `enterobacterales` - compare the listed SEQIDs to a set of reference sequences for species from the order Enterobacterales
    - `listeriaceae` - compare the listed SEQIDs to a set of reference sequences for species from the family Listeriaceae

You must also include a list of SEQIDs one per line.

**Optional Components**

By default, dRep will run the compare command to compare the set of genomes. In order to customise your GeneSeekr analyses, several settings can be optionally modified

- MASH ANI cutoff - this value is important, as only sequences clustering within this 'relatedness' threshold will be analysed using the secondary ANI algorithm (which is what determines the ANI values)
    - default is `0.9` (90%) 
    - If you want to use custom cutoffs for MASH clustering, add a line to your description:
        - `mash_ANI_threshold=0.8`

- Secondary clustering cutoff
    - default is `0.99` (99%) 
    - modify as follows:
        - `S_ANI=0.9`

- coverage threshold (minimum overlap between genomes when doing secondary comparisons)
    - default is `0.1` (10%) 
    - modify as follows:
        - `coverage_threshold=0.2`

- cluster algorithm 
    - default is `average` 
    - modify as follows:
        - `clusteralgorithm=ward`
    - available algorithms include:
        - `median`, `weighted`, `single`, `complete`, `average`, `ward`, `centroid`

- comparison algorithm 
    - default is `ANImf` 
    - modify as follows:
        - `comparisonalgorithm=fastANI`
    - available algorithms include:
        - `fastANI`, `ANImf`, `ANIn`, `gANI`, `goANI`

- command type (compare genomes for ANI values, or dereplicate, see [dRep](https://drep.readthedocs.io/en/latest/overview.html)). **NOTE:currently debugging this option**
    - default is `compare` 
    - modify as follows:
        - `command=dereplicate`

#### Example



#### Interpreting Results

dRep will upload a zip file to the ftp on completion. This will contain folders with data, figures, .

### How long does it take?

This depends largely on the number of strains you want to compare and their relatedness. It can often be as quick as a few minutes
if you have 10 or less strains, or take several hours for more strains. The primary clustering step uses Mash to determine whether sequences are likely related (based on provided cutoff or default 90%). 

The secondary analysis which calculates the ANI values is only completed on sets of genomes that have at least 90% Mash ANI (unless this cutoff is modified). These secondary comparisons are slower (more sensitive and accurate for ANI). Therefore, if you have a large number of closely related genomes your job may take longer to complete.

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) You did not provide an analysis type. Custom analysis only completes dRep compare for the SEQ-IDs listed, whereas enterobacterales will compare your listed sequences to reference sequences from the order Enterobacterales, and listeriaceae will compare your listed sequences to reference sequences from the family Listeriaceae.

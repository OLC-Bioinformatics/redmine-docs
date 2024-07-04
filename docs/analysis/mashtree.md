# MashTree

### What does it do?

MashTree will take a list of SEQIDs, and then create a
phylogenetic tree using Mash distances. See [mashtree github](https://github.com/lskatz/mashtree) and the [mashtree docs](https://github.com/lskatz/mashtree/blob/master/docs/ALGORITHM.md) for more information. (The mashtree docs explains how mash is used to create the phylogeny).

If you use mashtree, please remember to cite [Katz et al., 2019](Mashtree: a rapid comparison of whole genome sequence files), [Ondov et al., 2016](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0997-x) and [Ondov et al., 2019](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1841-x)


### How do I use it?

#### Subject

In the `Subject` field, put `mashtree`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

The first line of the description should be the analysis you would like to run (e.g. `analysis=custom`). 

- The following options are currently supported:
    - `custom` - compare only the SEQIDs listed in the description
    - `enterobacterales` - compare the listed SEQIDs to a set of reference sequences for species from the order Enterobacterales
    - `listeriaceae` - compare the listed SEQIDs to a set of reference sequences for species from the family Listeriaceae

You must also include a list of SEQIDs one per line.

**Optional Components**

- genomesize: 
    - default is `5000000` 
    - If you want to use a genome size estimate, add a line to your description:
        - `genomesize=3000000`

- mindepth - if mindepth is zero, then it will be chosen in a smart but slower method to discard lower-abundance kmers.
    - default is `5` 
    - If you want to use a minimum depth, add a line to your description:
        - `mindepth=10`

- kmerlength - [Mash kmer size](https://mash.readthedocs.io/en/latest/sketches.html) "larger k-mers will provide more specificity while smaller k-mers will provide more sensitivity. (Larger genomes will also require larger k-mers to avoid k-mers that are shared by chance)."
    - default is `21` 
    - If you want to change the k-mer size, add a line to your description:
        - `kmerlength=30`

- sketch-size - [Mash sketch size](https://mash.readthedocs.io/en/latest/sketches.html) "corresponds to the number of (non-redundant) min-hashes that are kept. Larger sketches will better represent the sequence, but at the cost of larger sketch files and longer comparison times."
    - default is `10000` 
    - If you want to change the mash sketch-size, add a line to your description:
        - `sketch-size=1000`


#### Example

For an example MashTree, see [issue 26206](https://redmine.biodiversity.agr.gc.ca/issues/26206).

#### Interpreting Results

Upon completion, you'll be given a treefile.

### How long does it take?

This depends largely on the number of strains you want to use to create the tree. It can often be as quick as a few minutes. 

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

### Version

Version 0.35.4 is currently available at the OLC. (as of 2024-07-4)

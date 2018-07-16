# StrainMash

### What does it do?

StrainMash uses the MinHash algorithm (as implemented by [mash](https://github.com/marbl/mash)) in order to very quickly
find which RefSeq type strain an assembly is closest to. This can be very useful if you aren't entirely sure what species
it is that you've actually sequnced.

### How do I use it?

#### Subject

In the `Subject` field, put `StrainMash`. Spelling counts, but case sensitivity doesn't.

#### Description

All you need to put in the description is a list of SEQIDs you want to analyze, one per line.

#### Example

For an example StrainMash, see [issue 12860](https://redmine.biodiversity.agr.gc.ca/issues/12860).

#### Interpreting Results

StrainMash will upload a zip file once it finishes. This contains a folder called output, which will have a
text file for each SEQID entered showing results.

These text files have 6 columns:

* MashDistance: Represents roughly how close the query sequence is the the reference strain. 1 is a perfect match, 0 is no
similarity. To say that something very closely matches a reference strain, this number should be _*at least*_ 0.98
* NumMatchingHashes: Another measure of how close the query sequence is to the reference strain. 1000/1000 is a perfect match,
0 is no similarity. Number needs to be fairly high (>850) for close matches to reference.
* MedianMultiplicity: Can mostly be ignored. Should always be 1.
* Pvalue: An attempt to assign significance to the hit. Should be zero for anything that very closely matches a reference
strain.
* ReferenceStrain: The NCBI Accession for the reference strain.
* Organism: The name of the organism that is the reference strain.

### How long does it take?

StrainMash should take about 30 seconds to 1 minute to analyze each sample you send to it.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.
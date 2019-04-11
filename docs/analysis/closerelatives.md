# CloseRelatives

### What does it do?

CloseRelatives uses mash to try to figure out what genomes in the CFIA collection are closest
to a query genome. 

### How do I use it?

#### Subject

In the `Subject` field, put `Close Relatives`. Spelling counts, but case sensitivity doesn't.

#### Description

The first line of the description should be the number of closely related strains you want to find, 
and the second line should be the SeqID you want to use as a query. For example, to find the 5 closest
genomes to 2014-SEQ-0276, you would enter:

```
10
2014-SEQ-0276
```

#### Example

For an example CloseRelatives issue, see [issue 14676](https://redmine.biodiversity.agr.gc.ca/issues/14676).

#### Interpreting Results

The automator will give you a list of the number of closest strains it found and their mash distances.
In the event that you want to look at more strains, it will also upload a file called `close_relatives_results.csv` that
has the distance for every genome in the CFIA collection.

For interpretation of the mash distance reported, know that 0 means that two strains are identical (or at least extremely
close to identical), and 1 means that two strains have essentially no similarity.

### How long does it take?

Mash is super quick - close relatives should finish in 2 or 3 minutes.

### What can go wrong?

1. Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.
2. Number of strains desired not requested. If the first line of the description wasn't a number, you'll get an 
error telling you to enter a number.

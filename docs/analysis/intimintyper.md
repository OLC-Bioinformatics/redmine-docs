# IntiminTyper

### What does it do?

IntiminTyper uses [phylotyper](https://github.com/superphy/insilico-subtyping) developed at the Public Health Agency of Canada to assign subtypes to intimin (*eae*) 
genes in *Escherichia coli*.

### How do I use it?

#### Subject

In the `Subject` field, put `intimin_typer`. Spelling counts, but case sensitivity doesn't.

#### Description

All you need to put in the description is a list of SEQIDs you want to subtype intimin genes in, one per line.
Nothing bad will happen if you put in SEQIDs that aren't from *E. coli*, but you won't get any results for those
SeqIDs.

#### Example

For an example IntiminTyper, see [issue 16512](https://redmine.biodiversity.agr.gc.ca/issues/16512).

#### Interpreting Results

IntiminTyper will upload a file called `intimin_predictions.tsv` - this file will tell what subtype the intimin gene in 
each of your samples is, noting if it's an exact match to a known allele or a guess based on what the allele is similar 
to. When IntiminTyper guesses you'll also receive a probability showing how certain it is with its guess.

### How long does it take?

IntiminTyper should take about 30 seconds for analysis of each sample.

### What can go wrong?

1) Requested SEQIDs are not available: If we can't find some of the SEQIDs that you request, they won't show up in your
final report.



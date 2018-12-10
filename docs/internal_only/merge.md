# Merge

### What does it do?

Merge will take data from multiple MiSeq runs of the same strain and combine the FASTQ files in order 
to produce a better assembly. 

### How do I use it?

#### Subject

In the `Subject` field, put `Merge`. Spelling counts, but case sensitivity doesn't.

#### Description

No text necessary in the description. You will, however, need to attach an excel file (.xlsx extension)
with the following headers: 

- Location: Should be 'MER'
- SEQID: Put the name of the merged SEQID (YYYY-MER-####)
- OLNID: Can be blank
- LabID: Can be blank
- LabIDIsolate: Can be blank
- OtherName: The SeqIDs you want to merge, separated by semi-colons.
- Genus: Genus of the isolate. Can be left blank.
- Species: Can be blank 
- Subspecies: Can be blank
- Serotype: Can be blank

An example file can be found with the example issue below.

#### Example

For an example Merge, see [issue 14285](https://redmine.biodiversity.agr.gc.ca/issues/14285).

#### Interpreting Results

You'll get back the reports and assemblies, as you would for a `WGS Assembly` issue.

### How long does it take?

Merge will take quite a while, depending on how many samples you have. Probably roughly half an hour per sample.

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.


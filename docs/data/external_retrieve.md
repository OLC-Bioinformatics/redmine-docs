# External Retrieve

### What does it do?

External retrieve is a process that will upload data you request (either raw reads or draft assemblies) to an FTP site
so that you can download it in the event that you need to have your files locally available.

### How do I use it?

#### Subject

In the `Subject` field, put `External Retrieve`. Spelling counts, but case sensitivity doesn't.

#### Description

The first line of your description tells the process whether you want raw reads or draft assemblies. For reads,
the first line should be `fastq`, and for assemblies the first line should be `fasta`. Every line after that should be a
SEQID that you want the data for.

#### Example

For an example of an External Retrieve, see [issue 12822](https://redmine.biodiversity.agr.gc.ca/issues/12822).

### How long does it take?

If your request is for a small number of files, it will generally be done within a few minutes. The more files requested,
the longer the request will take.

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) FTP timeout. Sometimes, particularly for larger requests, the upload to the FTP will run into problems and time out,
in which case you will likely get an error message similar to this: `[Errno 104] Connection reset by peer`. If this occurs,
you can either try again later, or, if you had a large request, try splitting it into a few smaller requests. If the
problem persists, send us an email and we'll try to get it figured out.
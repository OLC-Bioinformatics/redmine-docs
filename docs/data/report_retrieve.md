# Report Retrieve

### What does it do?

Report retrieve will grab assembly reports for you and upload them to the FTP.

### How do I use it?

#### Subject

In the `Subject` field, put `Report Retrieve`. Spelling counts, but case sensitivity doesn't.

#### Description

Put the SEQIDs you want reports for in the description, one per line. 

#### Example

For an example of an External Retrieve, see [issue 13931](https://redmine.biodiversity.agr.gc.ca/issues/13931).

### How long does it take?

This should be almost instant - your request should be processed within a few minutes.

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) FTP timeout. Sometimes, particularly for larger requests, the upload to the FTP will run into problems and time out,
in which case you will likely get an error message similar to this: `Upload of result files was unsucessful due to
FTP connectivity issues`. If this occurs, your best bet is to try again later. If problems persist, send us an email
and we'll look into it.

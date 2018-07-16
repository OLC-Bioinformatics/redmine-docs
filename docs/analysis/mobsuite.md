# MobSuite

### What does it do?

MobSuite is a set of tools developed by the Public Health Agency of Canada for detecting plasmids in draft genome
assemblies. This tool runs the `mob_recon` part of the suite, which first detects plasmids in the assemblies, and then
performs typing on the plasmids. More details on MobSuite, including fairly extensive details on the output files
produced, can be found at the [MobSuite GitHub repository](https://github.com/phac-nml/mob-suite).

### How do I use it?

#### Subject

In the `Subject` field, put `MobSuite`. Spelling counts, but case sensitivity doesn't.

#### Description

All you need to put in the description is a list of SEQIDs you want to look for plasmids in, one per line.

#### Example

For an example MobSuite, see [issue 12823](https://redmine.biodiversity.agr.gc.ca/issues/12823).

### How long does it take?

MobSuite works fairly quickly - it should take roughly one minute to analyze each SEQID that you have requested.

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning
message informing you of it.

2) FTP timeout. Sometimes, particularly for larger requests, the upload of results to the FTP will run into problems and time out,
in which case you will likely get an error message similar to this: `[Errno 104] Connection reset by peer`. If this occurs,
you can either try again later, or, if you had a large request, try splitting it into a few smaller requests. If the
problem persists, send us an email and we'll try to get it figured out.

# SRA Upload

### What does it do?

SRA Upload allows you to preload your sequence data for submission to the NCBI SRA using an FTP connection.

### How do I use it?

#### Subject

In the `Subject` field, put `SRA Upload`. Spelling counts, but case sensitivity doesn't.

#### Description

Before submitting, you'll need to get your FTP username, FTP password, and FTP folder name from the SRA.
To do this, log in to the SRA submission portal. Click on the `My Submissions` tab towards the top right of the page,
and then in the `Start a new submission` box, click `Sequence Read Archive`. Under `Options to preload data`, click 
`FTP upload`, and take note of the __Username__, __Password__, and the text under __Navigate to your account folder.__

Once you have those three things, you're ready to go. Put the username in the first line of the description, password 
in the second line, and the folder specified in the third (it should be something like `uploads/youremail_a1d3a1`).
In subsequent lines, put the SeqIDs you want to upload files for. Once the request finishes running, you should be 
able to find a folder named with your Redmine issue ID when you select a preload folder during submission.

#### Example

For an example of an SRA Upload, see [issue 14963](https://redmine.biodiversity.agr.gc.ca/issues/14963).

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

3) Bad FTP credentials. If the username and password for the NCBI FTP site didn't work you'll get a message telling you so.
Make sure your username is on the first line of the request and password is second.

4) FTP folder does not exist. If the FTP folder specified on the third line of the description isn't correct,
you'll get a message saying so.
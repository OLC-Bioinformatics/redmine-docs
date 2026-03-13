# Downsample

### What does it do?

The Downsample automator retrieves FASTQ files from the OLC NAS (by SEQID)
and then uses BBMap's `reformat.sh` to randomly subsample the reads. It can
subsample by **coverage**, **compressed output size**, or by providing any of
the standard BBMap `reformat.sh` sampling options.

The output is packaged as a zip file and made available via Dropbox.

### How do I use it?

#### Subject

In the `Subject` field, put `downsample`. Spelling counts, but case does not.

#### Description

**Required components**

- A list of SEQIDs, one per line. These are the identifiers used to locate
  FASTQ files on the NAS.

**Optional components**

You can tell the automator how much to downsample using one of the following:

- `COVERAGE=<float>` — target coverage (e.g., `COVERAGE=5`)
- `TARGETSIZE=<size>` — target compressed output size (e.g., `TARGETSIZE=1G`, `500M`, `100000000`)

If neither `COVERAGE` nor `TARGETSIZE` is supplied, the automator will
calculate a target size that aims to keep the compressed output under ~1 GB
(by estimating the compression ratio of the input files).

**Reformat / sampling options**

Any `reformat.sh` option supplied as `KEY=VALUE` in the description will be
forwarded to `reformat.sh`. Keys are treated case-insensitively (the automator
uppercases them internally), so `coverage=5` and `COVERAGE=5` behave the same.
In practice, this means you can use *any* `reformat.sh` parameter in your request.

The most commonly used sampling options are:

- `SAMPLEREADSTARGET` (exact number of reads)
- `SAMPLEBASETARGET` (exact number of bases)
- `SAMPLERATE` (fraction to keep)
- `SAMPLEFRACTION` (same as samplerate)
- `SAMPLESEED` (random seed; set this for repeatable sampling runs)
- `INTERLEAVED` (treat input as interleaved paired reads)
- `MINLENGTH` / `MIN_LEN` (discard reads shorter than this)
- `MAXLENGTH` / `MAX_LEN` (discard reads longer than this)
- `QTRIM` / `MINQ` (trim low-quality bases from read ends)
- `TRIMQ` (quality threshold used for trimming)

<details>
<summary><b>Full list of supported `reformat.sh` options (from `reformat.sh --help`)</b></summary>

```text
Parameters and their defaults:

ow=f                    (overwrite) Overwrites files that already exist.
app=f                   (append) Append to files that already exist.
zl=4                    (ziplevel) Set compression level, 1 (low) to 9 (max).
int=f                   (interleaved) Determines whether INPUT file is considered interleaved.
fastawrap=70            Length of lines in fasta output.
fastareadlen=0          Set to a non-zero number to break fasta files into reads of at most this length.
fastaminlen=1           Ignore fasta reads shorter than this.
qin=auto                ASCII offset for input quality.  May be 33 (Sanger), 64 (Illumina), or auto.
qout=auto               ASCII offset for output quality.  May be 33 (Sanger), 64 (Illumina), or auto (same as input).
qfake=30                Quality value used for fasta to fastq reformatting.
qfin=<.qual file>       Read qualities from this qual file, for the reads coming from 'in=<fasta file>'
qfin2=<.qual file>      Read qualities from this qual file, for the reads coming from 'in2=<fasta file>'
qfout=<.qual file>      Write qualities from this qual file, for the reads going to 'out=<fasta file>'
qfout2=<.qual file>     Write qualities from this qual file, for the reads coming from 'out2=<fasta file>'
outsingle=<file>        (outs) If a read is longer than minlength and its mate is shorter, the longer one goes here.
deleteinput=f           Delete input upon successful completion.
ref=<file>              Optional reference fasta for sam processing.

Processing Parameters:

verifypaired=f          (vpair) When true, checks reads to see if the names look paired.  Prints an error message if not.
verifyinterleaved=f     (vint) sets 'vpair' to true and 'interleaved' to true.
allowidenticalnames=f   (ain) When verifying pair names, allows identical names, instead of requiring /1 and /2 or 1: and 2:
tossbrokenreads=f       (tbr) Discard reads that have different numbers of bases and qualities.  By default this will be detected and cause a crash.
ignorebadquality=f      (ibq) Fix out-of-range quality values instead of crashing with a warning.
addslash=f              Append ' /1' and ' /2' to read names, if not already present.  Please include the flag 'int=t' if the reads are interleaved.
spaceslash=t            Put a space before the slash in addslash mode.
addcolon=f              Append ' 1:' and ' 2:' to read names, if not already present.  Please include the flag 'int=t' if the reads are interleaved.
underscore=f            Change whitespace in read names to underscores.
rcomp=f                 (rc) Reverse-complement reads.
rcompmate=f             (rcm) Reverse-complement read 2 only.
comp=f                  (complement) Reverse-complement reads.
changequality=t         (cq) N bases always get a quality of 0 and ACGT bases get a min quality of 2.
quantize=f              Quantize qualities to a subset of values like NextSeq.  Can also be used with comma-delimited list, like quantize=0,8,13,22,27,32,37
tuc=f                   (touppercase) Change lowercase letters in reads to uppercase.
uniquenames=f           Make duplicate names unique by appending _<number>.
remap=                  A set of pairs: remap=CTGN will transform C>T and G>N.
                        Use remap1 and remap2 to specify read 1 or 2.
iupacToN=f              (itn) Convert non-ACGTN symbols to N.
monitor=f               Kill this process if it crashes.  monitor=600,0.01 would kill after 600 seconds under 1% usage.
crashjunk=t             Crash when encountering reads with invalid bases.
tossjunk=f              Discard reads with invalid characters as bases.
fixjunk=f               Convert invalid bases to N (or X for amino acids).
dotdashxton=f           Specifically convert . - and X to N (or X for amino acids).
recalibrate=f           (recal) Recalibrate quality scores.  Must first generate matrices with CalcTrueQuality.
maxcalledquality=41     Quality scores capped at this upper bound.
mincalledquality=2      Quality scores of ACGT bases will be capped at lower bound.
trimreaddescription=f   (trd) Trim the names of reads after the first whitespace.
trimrname=f             For sam/bam files, trim rname/rnext fields after the first space.
fixheaders=f            Replace characters in headers such as space, *, and | to make them valid file names.
warnifnosequence=t      For fasta, issue a warning if a sequenceless header is encountered.
warnfirsttimeonly=t     Issue a warning for only the first sequenceless header.
utot=f                  Convert U to T (for RNA -> DNA translation).
padleft=0               Pad the left end of sequences with this many symbols.
padright=0              Pad the right end of sequences with this many symbols.
pad=0                   Set padleft and padright to the same value.
padsymbol=N             Symbol to use for padding.

Sampling parameters:

reads=-1                Set to a positive number to only process this many INPUT reads (or pairs), then quit.
skipreads=-1            Skip (discard) this many INPUT reads before processing the rest.
samplerate=1            Randomly output only this fraction of reads; 1 means sampling is disabled.
sampleseed=-1           Set to a positive number to use that prng seed for sampling (allowing deterministic sampling).
samplereadstarget=0     (srt) Exact number of OUTPUT reads (or pairs) desired.
samplebasestarget=0     (sbt) Exact number of OUTPUT bases desired.
                        Important: srt/sbt flags should not be used with stdin, samplerate, qtrim, minlength, or minavgquality.
upsample=f              Allow srt/sbt to upsample (duplicate reads) when the target is greater than input.
prioritizelength=f      If true, calculate a length threshold to reach the target, and retain all reads of at least that length (must set srt or sbt).

Trimming and filtering parameters:

qtrim=f                 Trim read ends to remove bases with quality below trimq.
                        Values: t (trim both ends), f (neither end), r (right end only), l (left end only), w (sliding window).
trimq=6                 Regions with average quality BELOW this will be trimmed.  Can be a floating-point number like 7.3.
minlength=0             (ml) Reads shorter than this after trimming will be discarded.  Pairs will be discarded only if both are shorter.
mlf=0                   (mlf) Reads shorter than this fraction of original length after trimming will be discarded.
maxlength=0             If nonzero, reads longer than this after trimming will be discarded.
breaklength=0           If nonzero, reads longer than this will be broken into multiple reads of this length.  Does not work for paired reads.
requirebothbad=t        (rbb) Only discard pairs if both reads are shorter than minlen.
invertfilters=f         (invert) Output failing reads instead of passing reads.
minavgquality=0         (maq) Reads with average quality (after trimming) below this will be discarded.
maqb=0                  If positive, calculate maq from this many initial bases.
chastityfilter=f        (cf) Reads with names  containing ' 1:Y:' or ' 2:Y:' will be discarded.
barcodefilter=f         Remove reads with unexpected barcodes if barcodes is set, or barcodes containing 'N' otherwise.  
                        A barcode must be the last part of the read header.
barcodes=               Comma-delimited list of barcodes or files of barcodes.
maxns=-1                If 0 or greater, reads with more Ns than this (after trimming) will be discarded.
minconsecutivebases=0   (mcb) Discard reads without at least this many consecutive called bases.
forcetrimleft=0         (ftl) If nonzero, trim left bases of the read to this position (exclusive, 0-based).
forcetrimright=-1       (ftr) If nonnegative, trim right bases of the read after this position (exclusive, 0-based).
forcetrimright2=0       (ftr2) If positive, trim this many bases on the right end.
forcetrimmod=5          (ftm) If positive, trim length to be equal to zero modulo this number.
mingc=0                 Discard reads with GC content below this.
maxgc=1                 Discard reads with GC content above this.
gcpairs=t               Use average GC of paired reads.
                        Also affects gchist.

Illumina-specific parameters:
top=true                Include reads from the top of the flowcell.
bottom=true             Include reads from the bottom of the flowcell.

Kmer counting and cardinality estimation parameters:
k=0                     If positive, count the total number of kmers.
cardinality=f           (loglog) Count unique kmers using the LogLog algorithm.
loglogbuckets=1999      Use this many buckets for cardinality estimation.

```

</details>

> Note: The automator treats paired files with `_R1`/`_R2` suffixes as a pair
> and passes both to `reformat.sh` so that the output remains synchronized.

#### Example description

```
COVERAGE=5
2024-SEQ-1234
2024-SEQ-5678
```

or

```
TARGETSIZE=500M
SAMPLESEED=42
2024-SEQ-1234
```

#### Full example request

This is what a complete request might look like (one item per line):

```
COVERAGE=5
SAMPLESEED=42
SAMPLEBASETARGET=100000000
INTERLEAVED=true
2024-SEQ-1234
2024-SEQ-5678
```

This will downsample the paired reads for both SEQIDs to ~100 million bases, using a fixed seed for reproducibility.

### Interpreting Results

When the automator finishes, it uploads a zip file named
`downsample_results_<issue number>.zip` to Dropbox and posts a download link
in the Redmine issue.

The zip contains the downsampled FASTQ files, with the same filenames as the
originals (the automator removes its internal `_downsampled` suffix before
zipping).

### How long does it take?

Time depends on the input size and the sampling target. It runs one `reformat.sh`
call per input file (paired files are processed together), so the runtime scales
with the number of FASTQ files and their size.

### What can go wrong?

1. Requested SEQIDs are not available: you will get a warning in the issue and
   only the available SEQIDs will be processed.
2. The automator relies on BBMap (`reformat.sh` and `kmercountexact.sh`) being
   available in the configured conda environment; if those tools are missing or
   fail, the issue will be updated with an error.
3. Invalid or misspelled `KEY=VALUE` options will cause the job to fail.
   The automator validates each option against the supported `reformat.sh`
   parameters and reports unsupported keys in the issue.
4. Invalid values (e.g., `COVERAGE=FIVE` or `TARGETSIZE=oneG`) will also
   trigger an error because the automator expects numeric inputs for those
   fields.
5. If you request `COVERAGE=` and the genome size cannot be estimated (e.g.
   `kmercountexact.sh` fails or produces no peaks), the automator may fail.

### Version

This automator uses BBMap/BBTools from the `bbtools` conda environment.
The exact BBTools version is reported in the issue when the job completes.

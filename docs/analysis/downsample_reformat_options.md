# Downsample: Supported `reformat.sh` Options

This page is an advanced reference for BBMap `reformat.sh` options accepted by the [Downsample automator](downsample.md).

Use the main Downsample page for Subject, Description, examples, result interpretation, runtime, and troubleshooting. Supply options in the Redmine issue Description using `KEY=VALUE` syntax. The automator treats parameter names case-insensitively.

The values below are documented defaults from the supported option reference. An option may interact with other sampling, trimming, filtering, or pairing settings; review the [Downsample documentation](downsample.md) before combining options.

## Input and output options

### `ow`

Overwrite output files that already exist.

- Default: `false`
- Example: `ow=true`

### `app`

Append to output files that already exist.

- Default: `false`
- Example: `app=true`

### `zl`

Set the compression level from `1` to `9`.

- Default: `4`
- Example: `zl=6`

### `int`

Treat the input as interleaved.

- Default: `false`
- Example: `int=true`

### `fastawrap`

Set the output line length for FASTA files.

- Default: `70`
- Example: `fastawrap=80`

### `fastareadlen`

Break FASTA sequences into reads with this maximum length. A value of `0` disables this behavior.

- Default: `0`
- Example: `fastareadlen=1000`

### `fastaminlen`

Ignore FASTA reads shorter than this length.

- Default: `1`
- Example: `fastaminlen=100`

### `qin`

Set the input quality-score offset.

- Default: `auto`
- Example: `qin=33`

### `qout`

Set the output quality-score offset.

- Default: `auto`
- Example: `qout=33`

## Read validation and transformation

### `verifypaired`

Verify that paired-read names appear to be paired.

- Default: `false`
- Example: `verifypaired=true`

### `verifyinterleaved`

Enable paired-read verification for interleaved input.

- Default: `false`
- Example: `verifyinterleaved=true`

### `tossbrokenreads`

Discard reads with inconsistent base and quality counts.

- Default: `false`
- Example: `tossbrokenreads=true`

### `ignorebadquality`

Repair out-of-range quality values instead of stopping.

- Default: `false`
- Example: `ignorebadquality=true`

### `rcomp`

Reverse-complement reads.

- Default: `false`
- Example: `rcomp=true`

### `rcompmate`

Reverse-complement read 2 only.

- Default: `false`
- Example: `rcompmate=true`

### `tuc`

Convert lowercase bases to uppercase.

- Default: `false`
- Example: `tuc=true`

## Sampling options

### `reads`

Process no more than this many input reads or read pairs. A value of `-1` means no limit.

- Default: `-1`
- Example: `reads=1000000`

### `skipreads`

Discard this many reads before processing. A value of `-1` uses the documented default behavior.

- Default: `-1`
- Example: `skipreads=1000`

### `samplerate`

Set the random fraction of reads to retain.

- Default: `1`
- Example: `samplerate=0.25`

### `sampleseed`

Set the sampling seed. Supply a fixed integer for reproducible random sampling.

- Default: `-1`
- Example: `sampleseed=42`

### `samplereadstarget`

Set the desired number of output reads or read pairs. A value of `0` disables this target.

- Default: `0`
- Example: `samplereadstarget=1000000`

### `samplebasestarget`

Set the desired number of output bases. A value of `0` disables this target.

- Default: `0`
- Example: `samplebasestarget=100000000`

### `upsample`

Permit read duplication when the requested target exceeds the input.

- Default: `false`
- Example: `upsample=true`

### `prioritizelength`

Prioritize reads based on length during sampling.

- Default: `false`
- Example: `prioritizelength=true`

## Trimming and filtering options

### `qtrim`

Select whether quality trimming is applied to read ends.

- Default: `false`
- Example: `qtrim=rl`

### `trimq`

Set the trimming quality threshold.

- Default: `6`
- Example: `trimq=10`

### `minlength`

Discard reads shorter than this length after trimming. A value of `0` disables the minimum.

- Default: `0`
- Example: `minlength=50`

### `maxlength`

Discard reads longer than this threshold. A value of `0` disables the maximum.

- Default: `0`
- Example: `maxlength=300`

### `minavgquality`

Discard reads below this average quality. A value of `0` disables the threshold.

- Default: `0`
- Example: `minavgquality=20`

### `maxns`

Discard reads containing more than this number of `N` bases. A value of `-1` disables the threshold.

- Default: `-1`
- Example: `maxns=0`

### `mingc`

Set the minimum accepted GC fraction.

- Default: `0`
- Example: `mingc=0.30`

### `maxgc`

Set the maximum accepted GC fraction.

- Default: `1`
- Example: `maxgc=0.70`

## K-mer and cardinality options

### `k`

Count k-mers when set to a positive value. A value of `0` disables k-mer counting.

- Default: `0`
- Example: `k=31`

### `cardinality`

Estimate unique k-mers using the LogLog algorithm.

- Default: `false`
- Example: `cardinality=true`

### `loglogbuckets`

Set the number of buckets used for cardinality estimation.

- Default: `1999`
- Example: `loglogbuckets=4000`

## Example advanced request

```text
samplerate=0.25
sampleseed=42
qtrim=rl
trimq=10
minlength=50
2026-SEQ-0001
```

This example retains a random quarter of the input reads using a fixed seed, trims both read ends at the selected quality threshold, and discards reads shorter than 50 bases after trimming.

# Downsample

## What does it do?

Use **Downsample** to retrieve FASTQ files by `SEQID` and reduce the amount of raw-read data with BBMap `reformat.sh`.

The automator can downsample by:

- target coverage with `COVERAGE`;
- target compressed output size with `TARGETSIZE`;
- target read or read-pair count;
- target base count;
- sampling fraction; or
- other supported `reformat.sh` options.

When neither `COVERAGE` nor `TARGETSIZE` is supplied, the automator estimates a target intended to keep the compressed output below approximately 1 GB. This estimate uses the apparent compression ratio of the input files.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
downsample
```

Spelling matters, but matching is not case-sensitive.

### Description

The **Description** field must contain:

1. the desired downsampling or processing options in `KEY=VALUE` form; and
2. one `SEQID` per line.

Parameter names are case-insensitive because the automator normalizes them internally. For example, `coverage=5` and `COVERAGE=5` are equivalent.

The automator treats files with `_R1` and `_R2` suffixes as a pair and passes both files to `reformat.sh` together so their outputs remain synchronized.

### Attachments

No attachment is required. The automator retrieves the FASTQ files associated with each requested `SEQID` from the OLC NAS.

### Primary downsampling parameters

#### `COVERAGE`

Sets a target sequencing coverage.

- Value: numeric coverage
- Example: `COVERAGE=5`

The automator must estimate genome size to calculate a coverage-based target. Coverage-based downsampling can fail when genome-size estimation does not produce a usable result.

#### `TARGETSIZE`

Sets a target compressed output size.

Accepted examples include:

```text
TARGETSIZE=1G
TARGETSIZE=500M
TARGETSIZE=100000000
```

### Common sampling parameters

#### `SAMPLEREADSTARGET`

Sets the desired number of output reads or read pairs.

```text
SAMPLEREADSTARGET=1000000
```

#### `SAMPLEBASETARGET`

Sets the desired number of output bases.

```text
SAMPLEBASETARGET=100000000
```

#### `SAMPLERATE`

Sets the fraction of reads to retain.

```text
SAMPLERATE=0.25
```

`SAMPLEFRACTION` is accepted as an alias for `SAMPLERATE`.

#### `SAMPLESEED`

Sets the random seed used for sampling. Supply an explicit seed when reproducible random sampling is required.

```text
SAMPLESEED=42
```

#### `INTERLEAVED`

Controls whether the input is treated as interleaved paired reads.

```text
INTERLEAVED=true
```

#### Length and quality options

Common options include:

- `MINLENGTH` or `MIN_LEN` — discard reads shorter than the specified length;
- `MAXLENGTH` or `MAX_LEN` — discard reads longer than the specified length;
- `QTRIM` — choose the read ends from which low-quality bases are trimmed;
- `TRIMQ` — set the quality threshold used for trimming;
- `MINQ` — set the minimum quality value accepted by the automator.

### Important option interactions

Do not combine `SAMPLEREADSTARGET` or `SAMPLEBASETARGET` with incompatible sampling or filtering options unless the underlying `reformat.sh` operation supports that combination.

The automator validates option names before submitting the job. Unsupported or misspelled keys cause an error in the Redmine issue.

For the broader option allowlist and documented defaults, see [Downsample: Supported reformat.sh Options](downsample_reformat_options.md).

### Examples

#### Downsample by coverage

```text
COVERAGE=5
SAMPLESEED=42
2026-SEQ-0001
2026-SEQ-0002
```

#### Downsample by compressed output size

```text
TARGETSIZE=500M
SAMPLESEED=42
2026-SEQ-0001
```

#### Downsample to a target number of bases

```text
SAMPLEBASETARGET=100000000
SAMPLESEED=42
2026-SEQ-0001
2026-SEQ-0002
```

This request retains approximately 100 million bases for each processed input while using a fixed random seed.

## Interpreting results

When Downsample finishes, it posts a Dropbox download link in the Redmine issue for an archive named with the issue number:

```text
downsample_results_<issue number>.zip
```

The archive contains the downsampled FASTQ files. Output files use the same filenames as the originals because the automator removes its internal `_downsampled` suffix before creating the archive.

Verify that all expected `SEQID`s and read pairs are present. If the issue warned that requested sequences were unavailable, only the available data will be included.

## How long does it take?

Runtime depends on input-file size, the number of FASTQ files, and the requested sampling or filtering operations. The automator runs one `reformat.sh` operation per single input or synchronized pair, so runtime scales with input volume.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that one or more requested sequences are unavailable, and only available sequences are processed.

**Likely cause:** The automator cannot locate the required FASTQ files on the OLC NAS.

**What to do:** Verify each `SEQID` and confirm that its raw-read files are available.

### A requested option is unsupported or misspelled

**Symptom:** The job fails and the issue identifies an unsupported key.

**Likely cause:** A `KEY=VALUE` option is not in the automator's allowlist or is misspelled.

**What to do:** Use an option documented on this page or in [Downsample: Supported reformat.sh Options](downsample_reformat_options.md).

### An option value is invalid

**Symptom:** The job reports an invalid value, such as `COVERAGE=FIVE` or `TARGETSIZE=oneG`.

**Likely cause:** The parameter requires a numeric value or a supported size suffix.

**What to do:** Correct the value using a documented example and submit a new request.

### Coverage-based downsampling cannot estimate genome size

**Symptom:** A request using `COVERAGE` fails before downsampling.

**Likely cause:** BBMap `kmercountexact.sh` failed or did not produce a usable genome-size estimate.

**What to do:** Review the issue error. If the available data are suitable, use `TARGETSIZE`, `SAMPLEREADSTARGET`, `SAMPLEBASETARGET`, or `SAMPLERATE` instead.

### BBMap is unavailable or fails

**Symptom:** The Redmine issue reports a `reformat.sh` or `kmercountexact.sh` error.

**Likely cause:** A required BBMap command is unavailable in the configured environment or failed while processing the input.

**What to do:** Retry only after confirming the input and options are valid. Escalate persistent environment or tool failures to the bioinformatics team.

## Related automators

- [FastQC/MultiQC](fastqc.md) — assesses raw-read quality before or after read-processing decisions.
- [Downsample: Supported reformat.sh Options](downsample_reformat_options.md) — advanced allowlist and default-value reference for the Downsample automator.

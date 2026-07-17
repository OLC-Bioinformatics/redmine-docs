# GWAS-pyseer

## What does it do?

Use **pyseer** to perform a microbial genome-wide association study (mGWAS) and identify genetic variants associated with a measured bacterial phenotype, such as antimicrobial resistance, virulence, or host specificity.

The Redmine automator supports association testing with:

- k-mers detected from the submitted genomes;
- gene-presence/absence data from a previous Roary request;
- an SNV mode documented as under development.

Association studies can be strongly affected by population structure, phenotype quality, sample size, relatedness, and multiple testing. Review the [pyseer documentation](https://pyseer.readthedocs.io/en/master/index.html), [usage guidance](https://pyseer.readthedocs.io/en/master/usage.html#population-structure), and [Lees et al. (2018)](https://academic.oup.com/bioinformatics/article/34/24/4310/5047751) before selecting a model. Cite the pyseer authors when publishing results.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
pyseer
```

Spelling matters, but matching is not case-sensitive.

### Description

The Description must include:

1. `analysistype=...`;
2. analysis-specific parameters;
3. population-structure and model settings when required;
4. one assembly `SEQID` per line.

### Supported analysis types

#### `kmer`

Detects and counts k-mers, then tests them as variants representing short sequence variation and gene presence/absence. The automator uses Prokka to annotate significant k-mer matches.

```text
analysistype=kmer
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

A custom `references.txt` file is recommended when trusted reference-quality assemblies are available.

#### `Roary`

Uses `gene_presence_absence.Rtab` from a previous Roary Redmine request.

Also provide the source Redmine issue identifier:

```text
analysistype=Roary
roaryissue=12345
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

The source Roary issue must contain a retained `gene_presence_absence.Rtab`. The supplied documentation states that these files are retained only for requests made after November 24, 2021.

Confirm whether `Roary` is case-sensitive in the current parser; the supplied page uses an uppercase `R` here.

#### `snv`

The supplied documentation labels SNV analysis as under development. It requires an attached phenotype file named exactly:

```text
traits.tsv
```

The first column contains `SEQID`s and the second contains the trait value. Binary traits use `1` for presence and `0` for absence. `NA` can be used when the phenotype was not determined.

For continuous phenotypes, include:

```text
continuous=True
```

The default is documented as:

```text
continuous=False
```

Do not rely on `analysistype=snv` for production analysis until its current implementation and validation status have been confirmed.

### Trait attachment

The supplied documentation explicitly requires `traits.tsv` for SNV mode and later lists a missing `traits.tsv` as a general failure. It does not clearly state the phenotype-file requirements for `kmer` and `Roary` modes, even though association testing requires a phenotype.

Before publishing this page as final, verify whether every analysis type requires `traits.tsv`, and confirm the accepted header, delimiter, phenotype column, sample matching, covariates, and missing-value behavior.

### Population structure

Select the source of population-structure information with `distance`.

#### Mash distance

Mash is the documented default. The automator creates Mash sketches, a pairwise distance matrix, and a Mash tree. The tree can be used to create the kinship matrix for a linear mixed model.

```text
distance=mash
```

#### bcgTree

Use bcgTree to create a core-gene phylogeny:

```text
distance=bcgtree
```

bcgTree options documented on the [bcgTree page](bcgtree.md) can also affect this step. This mode is slower, especially for large datasets. For a large study, consider producing and validating the tree separately.

#### Custom tree

Attach a Newick file named exactly:

```text
tree.newick
```

and specify:

```text
distance=custom
```

Every tree tip label must exactly match a `SEQID` in the pyseer request.

### Association models

#### `fixed`

Uses pairwise patristic distances and applies a generalized linear model to each variant.

```text
model=fixed
```

The supplied documentation describes the model names but does not explicitly show the request parameter key. Verify whether the deployed parser uses `model`, `association`, or another key before final publication.

#### `linearmixed`

Fits a linear mixed model with fixed and random effects and uses the selected phylogeny to construct sample relatedness or kinship.

```text
model=linearmixed
```

The request-key caveat above also applies. Mash-based relatedness may be less accurate than a validated phylogeny for some datasets.

### `references.txt` for k-mer annotation

When no file is attached, the automator generates one and treats all submitted sequences as reference quality.

To choose trusted references explicitly, attach a tab-separated file named:

```text
references.txt
```

It has no header and contains:

1. sequence filename;
2. annotation filename;
3. type: `ref` or `draft`.

Example:

```text
2026-SEQ-0001.fasta	2026-SEQ-0001.gff	ref
2026-MIN-0001.fasta	2026-MIN-0001.gff	ref
2026-SEQ-0002.fasta	2026-SEQ-0002.gff	draft
2026-SEQ-0340.fasta	2026-SEQ-0340.gff	draft
```

Reference-quality records are searched first and allow a close k-mer match. Draft records are searched afterward and require an exact match.

### Example

See [issue 25083](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/25083) for a historical pyseer request. Its result files are no longer available.

## Interpreting results

The automator uploads:

```text
pyseer_output.zip
```

Depending on selected options, it can also upload:

```text
prokka_output.zip
bcgtree_output.zip
```

### K-mer analysis

Important files include:

```text
pyseer_kmers.txt
significant_kmers.txt
annotated_kmers.txt
gene_hits.txt
```

- `pyseer_kmers.txt` contains tested k-mer results;
- `significant_kmers.txt` contains k-mers passing the workflow's significance criteria;
- `annotated_kmers.txt` contains annotation results for significant k-mers;
- `gene_hits.txt` summarizes annotated gene hits and can be used for downstream visualization.

### Roary analysis

The important output is:

```text
pyseer_COGs.txt
```

The first column contains the gene name from the Roary `gene_presence_absence.Rtab`. Results not filtered out by pyseer remain in this file.

Flags in the last column can include:

- `high-bse` — a high effect-size/low-frequency result;
- `bad-chisq` — a result for which the minor-allele-frequency filter may be insufficient.

Treat flagged results cautiously and review the pyseer documentation and model assumptions.

### Population-structure outputs

`pyseer_output.zip` can contain tree files and distance matrices. When `analysistype=kmer` or `distance=bcgtree` is selected, `prokka_output.zip` can contain annotation files, particularly `.gff` files used in the workflow.

When `distance=bcgtree` is used, `bcgtree_output.zip` contains the core-gene alignment, identifiers, logs, and RAxML tree files described on the [bcgTree page](bcgtree.md).

Association does not establish causation. Review effect size, frequency, population-structure correction, phenotype quality, multiple-testing correction, annotation, and biological plausibility. Validate important findings independently.

## How long does it take?

Runtime depends on sample count, analysis type, population-structure method, model, and annotation requirements.

K-mer analysis can take several hours because k-mers must be counted across the complete dataset. Prokka generally takes approximately two to three minutes per genome. bcgTree runtime depends on genome count and bootstrap settings.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that one or more assemblies cannot be found.

**Likely cause:** An identifier is incorrect or its assembly is unavailable.

**What to do:** Verify every `SEQID` and confirm that its assembly exists.

### The phenotype file is missing or invalid

**Symptom:** The issue asks for `traits.tsv`, or the association analysis cannot align phenotypes with samples.

**Likely cause:** The file is missing, named incorrectly, malformed, or contains identifiers that do not match the request.

**What to do:** Supply a correctly formatted file named `traits.tsv` after verifying the requirements for the selected analysis type.

### The previous Roary result is unavailable

**Symptom:** `analysistype=Roary` cannot load `gene_presence_absence.Rtab`.

**Likely cause:** `roaryissue` is incorrect, the source request predates retention, or its output has been removed.

**What to do:** Verify the issue identifier or run a new Roary analysis before resubmitting pyseer.

### A custom tree cannot be used

**Symptom:** Population-structure setup fails with `distance=custom`.

**Likely cause:** The file is not named `tree.newick`, is not valid Newick, or its tip labels do not exactly match the requested `SEQID`s.

**What to do:** Correct the file name, syntax, and tip labels.

### The model or request key is unsupported

**Symptom:** The automator rejects the selected association model.

**Likely cause:** The model value or request parameter key differs from the legacy documentation.

**What to do:** Verify a current known-good request or inspect the parser before resubmitting.

### Results are unstable or misleading

**Symptom:** Associations have extreme effects, low frequency, warning flags, or change substantially with the population-structure method.

**Likely cause:** The dataset may be small, imbalanced, highly structured, confounded, or inadequately filtered.

**What to do:** Reassess study design, phenotype balance, variant frequency, population-structure correction, and independent validation with a qualified bioinformatician or statistician.

## Related automators

- [Roary/Scoary](roary.md) — calculates a pan-genome and performs simpler binary accessory-gene/trait association analysis.
- [bcgTree](bcgtree.md) and [MashTree](mashtree.md) — provide population-structure options or supporting phylogenies.
- [Prokka](prokka.md) — provides genome annotations used in k-mer interpretation.

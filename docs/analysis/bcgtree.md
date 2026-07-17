# bcgTree

## What does it do?

Use **bcgTree** to build a bacterial phylogeny from conserved single-copy core genes.

The pipeline automatically extracts 107 essential single-copy genes found across a broad range of bacteria, identifies homologues with hidden Markov models, aligns the selected protein sequences, and performs a partitioned maximum-likelihood analysis with RAxML.

bcgTree is intended for bacterial assemblies. Unlike MashTree, its primary tree is based on aligned core-gene protein sequences rather than only estimated whole-genome Mash distances.

For background, see the [bcgTree publication](https://cdnsciencepub.com/doi/10.1139/gen-2015-0175). Cite Ankenbrand and Keller (2016) when publishing results produced with this workflow.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
bcgtree
```

Spelling matters, but matching is not case-sensitive.

### Description

Enter optional parameters first, followed by one bacterial assembly `SEQID` per line:

```text
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

### Attachments

The supplied documentation does not identify support for attached assemblies. Use available `SEQID`s unless attachment support has been verified in the current implementation.

### Optional parameters

#### `bootstraps`

Sets the number of bootstrap replicates.

- Default: `100`
- Example: `bootstraps=1000`

More bootstrap replicates increase runtime.

#### `min_proteomes`

Sets the minimum number of proteomes in which a gene must occur to be retained.

- Default: `2`
- Example: `min_proteomes=5`

Increasing this value can reduce the number of genes retained when taxa are diverse or assemblies are incomplete.

#### `aa_substitution_model`

Selects the amino-acid substitution model used for RAxML partitions.

- Default: `AUTO`
- Example: `aa_substitution_model=GTR`

Supported values from the supplied documentation are:

```text
AUTO
DAYHOFF
DCMUT
JTT
MTREV
WAG
RTREV
CPREV
VT
BLOSUM62
MTMAM
LG
MTART
MTZOA
PMB
HIVB
HIVW
JTTDCMUT
FLU
STMTREV
DUMMY
DUMMY2
LG4M
LG4X
PROT_FILE
GTR_UNLINKED
GTR
```

Copy the model name exactly. Confirm this allowlist against the deployed RAxML/bcgTree version before final publication.

### Example

```text
bootstraps=100
min_proteomes=2
aa_substitution_model=AUTO
2026-SEQ-0001
2026-SEQ-0002
2026-SEQ-0003
```

See [issue 25083](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/25083) for an example bcgTree request. The historical result files associated with that issue have been purged.

## Interpreting results

When bcgTree finishes, it provides Dropbox links for two archives:

```text
prokka_output.zip
bcgtree_output.zip
```

### `prokka_output.zip`

Contains Prokka annotation output for each genome. The `.faa` protein FASTA files are used as inputs to bcgTree. See the [Prokka output documentation](https://github.com/tseemann/prokka#output-files) for descriptions of the other files.

### `bcgtree_output.zip`

Contains bcgTree alignments, gene-identifier files, configuration, logs, and RAxML outputs.

Important tree files include:

```text
RAxML_bestTree.final
RAxML_bipartitionsBranchLabels.final
RAxML_bipartitions.final
RAxML_bootstrap.final
```

- `RAxML_bestTree.final` contains the best-scoring maximum-likelihood tree.
- `RAxML_bipartitions.final` contains a tree with bootstrap support values mapped to branches.
- `RAxML_bipartitionsBranchLabels.final` contains bootstrap support represented as branch labels.
- `RAxML_bootstrap.final` contains the bootstrap replicate trees.

Open these files in a RAxML/Newick-compatible tree viewer. Use a supported tree file, alignment, model, and bootstrap evidence together when interpreting relationships.

The archive also contains:

- `config.txt` — commands and settings passed to bcgTree;
- `bcgtree.log` — executed commands, output, and RAxML random seed values.

Keep these files when reproducibility is important.

## How long does it take?

Prokka generally takes approximately two to three minutes per genome. After annotation, bcgTree runtime depends on sample count, retained genes, selected substitution model, and number of bootstrap replicates.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The issue warns that one or more assemblies cannot be found.

**Likely cause:** The requested `SEQID` has no available assembly.

**What to do:** Verify each identifier and confirm that its assembly exists.

### The substitution model is unsupported or misspelled

**Symptom:** The issue reports an invalid amino-acid substitution model.

**Likely cause:** `aa_substitution_model` contains a typo or a value not accepted by the deployed workflow.

**What to do:** Copy a supported model exactly and resubmit the request.

### Too few core genes are retained

**Symptom:** The workflow produces limited alignment data or fails during tree construction.

**Likely cause:** The assemblies are incomplete, the organisms are too diverse, or `min_proteomes` is too restrictive.

**What to do:** Review assembly quality and taxonomic scope, then use the default `min_proteomes` or analyze a more coherent bacterial set.

### Runtime is much longer than expected

**Symptom:** Annotation or tree inference takes substantially longer than a small default request.

**Likely cause:** The request contains many genomes, a high bootstrap count, or computationally demanding settings.

**What to do:** Reduce the dataset or bootstrap count when methodologically appropriate.

## Related automators

- [MashTree](mashtree.md) — builds a rapid distance-based whole-genome tree from Mash estimates.
- [NearTree](neartree.md) — ranks the closest strains to one query among a supplied set.
- [Prokka](prokka.md) — annotates assemblies without running the bcgTree phylogeny workflow.

# OLC Redmine Automator

The OLC Redmine Automator provides access to genomic data, analysis tools, and data-management workflows through Redmine. Access requires the appropriate organizational network and CFIA Genomics project permissions.

- New users: [Getting Started](getting_started.md)
- General help: [Troubleshooting](troubleshooting.md)
- Documentation contributions: [Adding documentation](tutorials/adding_documentation.md)

## Tool and database versions

Automator requests report tool and database versions when available. If a completed request does not report information needed for reproducibility, contact the bioinformatics team and include the Redmine issue number.

## Choose a workflow

### Import, export, or retrieve data and reports

- [External Retrieve](data/external_retrieve.md) — exports raw reads or draft assemblies for local download.
- [Report Retrieve](data/report_retrieve.md) — retrieves COWBAT assembly-pipeline reports for requested `SEQID`s.
- [SRA Download](data/sra_download.md) — imports FASTQ data from NCBI SRA and submits it to FoodPort/COWBAT processing.
- [SRA Upload](data/sra_upload.md) — transfers internal raw-read files into an NCBI SRA submission preload folder.

### Assess, combine, or reduce raw-read data

- [FastQC/MultiQC](analysis/fastqc.md) — creates per-sample read-quality reports and a combined MultiQC report.
- [QUAST](analysis/quast.md) - evaluates genome assemblies by computing various metrics
- [MetaQUAST](analysis/metaquast.md) - evaluates metagenome assemblies by computing various metrics
- [Downsample](analysis/downsample.md) — reduces FASTQ data by coverage, compressed size, read count, base count, sampling fraction, or supported BBMap options.
- [Downsample option reference](analysis/downsample_reformat_options.md) — lists additional supported `reformat.sh` options and defaults.
- [fastqmerge](analysis/fastqmerge.md) — combines FASTQ files from multiple sequencing runs of the same biological sample.

### Detect genes or run in silico PCR

- [GeneSeekr](analysis/geneseekr.md) — searches FASTA-formatted assemblies for predefined or attached custom targets.
- [Sipprverse](analysis/sipprverse.md) — searches raw paired-end FASTQ reads for predefined or custom targets.
- [KMA](analysis/kma.md) — screens assemblies or raw reads against curated or custom target databases.
- [PrimerFinder](analysis/primerfinder.md) — performs *in silico* PCR using VTyper or attached custom primer sets.

### Detect or summarize antimicrobial resistance

- [ResFinder](analysis/resfinder.md) — detects acquired AMR genes in draft genome assemblies; it does not detect chromosomal point mutations.
- [PointFinder](analysis/pointfinder.md) — detects supported resistance-associated point mutations in selected genera.
- [StarAMR](analysis/staramr.md) — combines acquired-gene detection with supported mutation detection for *Campylobacter* and *Salmonella* assemblies.
- [CARD-RGI](analysis/cardrgi.md) — predicts resistomes in isolate assemblies or raw FASTQ data.
- [AMRsummary](analysis/amrsummary.md) — combines acquired-AMR detection with predicted plasmid or chromosome location.
- [Plasmid-Borne Identity](analysis/plasmidborneidentity.md) — searches for user-supplied targets and predicts whether matching targets are plasmid-borne.

GeneSeekr, Sipprverse, and KMA also provide AMR-related target-screening modes. Use their pages when input type or a curated/custom target database determines the workflow choice.

### Identify an organism, profile taxonomy, or investigate contamination

- [Unknown Isolate](analysis/unknownisolate.md) — identifies an uncertain isolate assembly using rMLST, Mash, ANIb, and ANIm evidence in a GROBI report.
- [StrainMash](analysis/strainmash.md) — identifies the closest represented RefSeq type strain for an assembly.
- [AutoCLARK](analysis/autoclark.md) — reports species represented in raw reads or draft assemblies.
- [Kraken2/Bracken](analysis/kraken2.md) — performs taxonomic classification and refined abundance estimation using selectable databases.
- [MetaPhlAn4](analysis/metaphlan.md) — profiles microbial communities using clade-specific marker genes.

AutoCLARK, Kraken2/Bracken, and MetaPhlAn4 are taxonomic-analysis workflows.

#### Suspected contamination in raw reads

If you think raw FASTQ reads might be contaminated, run
[ConFindr](analysis/confindr.md). ConFindr checks for evidence of
intra-species and inter-species contamination before assembly or downstream
analysis.

Do not use Downsample, External Retrieve, or AutoCLARK as substitutes for a
ConFindr contamination assessment.

### Type an isolate

- [MLST](analysis/mlst.md) — determines an organism- and scheme-specific multilocus sequence type.
- [rMLST](analysis/rmlst.md) — performs ribosomal multilocus sequence typing.
- [ECTyper](analysis/ectyper.md) — predicts O- and H-antigen serotypes for *Escherichia coli* assemblies.
- [IntiminTyper](analysis/intimintyper.md) — assigns intimin (`eae`) subtypes to supported *E. coli* assemblies.
- [eCGF](analysis/eCGF.md) — performs comparative genomic fingerprinting subtyping for *Campylobacter* assemblies.

### Compare closely related isolates by SNVs or SNPs

- [SNVPhyl](analysis/snvphyl.md) — analyzes paired-end query reads and produces pairwise SNV counts, core-genome statistics, an alignment, and a Newick tree.
- [Snippy](analysis/snippy.md) — performs rapid variant calling and core alignment, supports paired-end and single-end reads, and can optionally create cleaned IQ-TREE outputs.
- [COWSNPhR](analysis/cowsnphr.md) — calls variants with DeepVariant, maps them to reference annotations, and creates summary tables, alignments, and a FastTree phylogeny.

All three workflows require a suitable reference and closely related query isolates. Their filters and output values are not interchangeable.

### Build a tree or select close or diverse genomes

- [MashTree](analysis/mashtree.md) — builds a rapid whole-genome distance tree from Mash estimates.
- [bcgTree](analysis/bcgtree.md) — builds a partitioned maximum-likelihood bacterial tree from essential single-copy core genes.
- [NearTree](analysis/neartree.md) — ranks candidate strains closest to one query within a supplied comparison set.
- [CloseRelatives](analysis/closerelatives.md) — finds CFIA collection genomes closest to one query using Mash distance.
- [DiversiTree](analysis/diversitree.md) — creates a Parsnp or MashTree tree and selects a diverse subset from the supplied genomes.
- [dRep](analysis/drep.md) — compares assemblies by Mash and secondary ANI algorithms or dereplicates a genome set.

### Analyze a pan-genome or microbial association

- [Roary/Scoary](analysis/roary.md) — calculates a pan-genome and can test binary trait associations against accessory-gene presence/absence.
- [GWAS-pyseer](analysis/pyseer.md) — performs microbial genome-wide association with explicit population-structure correction.

### Annotate genomes or detect mobile genetic elements

- [Prokka](analysis/prokka.md) — annotates genome assemblies and produces standard feature, nucleotide, and protein outputs.
- [MobSuite](analysis/mobsuite.md) — reconstructs and types predicted plasmids in draft genome assemblies.
- [PHASTEST](analysis/phastest.md) — identifies, annotates, and visualizes prophage regions in assembled bacterial genomes and plasmids.

### Extract or inspect assembly data

- [SequenceExtractor](analysis/sequence_extractor.md) — extracts a requested interval or complete contig from an assembly.
- [GFA Retrieve](analysis/gfa_retrieve.md) — retrieves GFA graphs from supported hybrid assemblies for visualization in Bandage.

### Predict protein subcellular localization

- [PSORTb](analysis/psortb.md) — predicts the subcellular localization of proteins encoded by a genome assembly.

## Before submitting a request

1. Open the selected workflow page.
2. Confirm whether it requires raw reads, an assembly, an attachment, or a previous Redmine result.
3. Copy the documented Subject exactly.
4. Preserve the documented Description order and parameter spelling.
5. Review attachment filenames, formats, and size limits.
6. Check the expected runtime and troubleshooting section.

Internal operational workflows are intentionally excluded from this standard-access index.

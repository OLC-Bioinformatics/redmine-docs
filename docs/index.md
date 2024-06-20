# OLC Redmine Automator

To facilitate the sharing of the CFIA's genomic data and tools to analyze this data, we make use of the Redmine
platform. Our instance of Redmine can be found [here](https://redmine.biodiversity.agr.gc.ca) (note that you must be on
the CFIA network in order to access Redmine).

If you have not yet gotten started with Redmine or want to know how the Redmine automator works and what it can do,
take a look at our [Getting Started](getting_started.md) page for instructions on how to start using Redmine and a
brief overview of what Redmine can do.

For any issues or general help with Redmine, see the [Troubleshooting](troubleshooting.md) page.

<br>
## Possible Analyses
###I want to:
<details>
  <summary><b>Download sequence data, or share sequence data with a collaborator</b></summary>  
  To retrieve a zip file of your sequence data (which can also be shared with external collaborators) use automators:  

   * [**External retrieve**](data/external_retrieve.md) - exports a list of sequences in a zipped file

</details>
<br>
<details>
  <summary><b>Download COWBAT reports for a list of sequence assemblies</b></summary>  
  To retrieve a zip file of the COWBAT assembly pipeline reports for your list of SeqIDs:  

   * [**Report retrieve**](data/report_retrieve.md) - exports a csv file of the legacy_combinedMetadata for your sequences, as well as a zip file containing all reports for those sequences.

</details>
<br>
<details>
  <summary><b>Detect gene(s) in my sequences</b></summary>  
  Automators that allow you to screen sequence(s) for gene targets:  

   * [**GeneSeekr**](analysis/geneseekr.md) - assemblies only  
   * [**KMA**](analysis/kma.md) - allows you to screen raw reads and assemblies
   * [**CARDRGI**](analysis/cardrgi.md) - *specific to AMR detection*

</details>
<br>
<details>
  <summary><b>Inspect the contigs of my Hybrid Assembly</b></summary>  
  Automators that allow you to retrieve gfa (graph files) for your hybrid assemblies:  

   * [**gfaretrieve**](analysis/gfa_retrieve.md) - hybrid assemblies only (MIN sequences)  


</details>
<br>
<details>
  <summary><b>Determine the genus/species of a whole genome sequence assembly</b></summary>  
  If you are unsure of the genus/species of your isolate, you can use the automator:  

   * [**Unknownisolate**](analysis/unknownisolate.md) - compares WGS assembly to ATCC and RefSeq genomes, and determines rMLST type. Outputs a GROBI report for probably identity.

</details>
<br>
<details>
  <summary><b>Find the <i>X</i> most diverse sequences in a set of sequences</b></summary>  
  If you want to find a number of diverse (e.g. distantly related) sequences from a set of sequences :  

   * [**diversitree**](analysis/diversitree.md) - will output a list based on user's requested number of sequences.

</details>
<br>
<details>
  <summary><b>Find sequences closely related to a query sequence</b></summary>  
  If you want to find <i>X</i> number of closest strains in a list to a query SeqID:  

   * [**NearTree**](analysis/neartree.md) - calculates the most closely related strains to your query SeqID based on MASH distance

</details>
<br>
<details>
  <summary><b>Determine taxonomy in metagenomes</b></summary>  
  If you want to detect the different organisms in your metagenomic sequence, you can use the automators:  

   * [**Metaphlan**](analysis/metaphlan.md) - more specific than Kraken2, but less sensitive.
   * [**Kraken2/Bracken**](analysis/kraken2.md) - more sensitive than metaphlan, but likely to give false positives to closely related genera/species. Bracken is more accurate than Kraken2 and Metaphlan4 (according to our publication [Cooper et al, 2023](https://bmcmicrobiol.biomedcentral.com/articles/10.1186/s12866-023-03148-6)).

</details>
<br>
<details>
  <summary><b>Find SNPs in my sequences</b></summary>  
  To detect single nucleotide polymorphisms (SNPs) in your sequences, you can use the automators:  

   * [**SNVPhyl**](analysis/snvphyl.md) - 
   * [**Snippy**](analysis/snippy.md) - 

</details>
<br>
<details>
  <summary><b>Annotate my sequence assemblies</b></summary>  
  To annotate your sequence assemblies, you can use the automators:  

   * [**Prokka**](analysis/prokka.md) - whole genome annotation to identify features in gDNA (bacterial, archaeal, and viral)
   * **Bakta** - automator currently under development

</details>
<br>
<details>
  <summary><b>Create a phylogeny</b></summary>  
  To create a phylogenetic tree from a list of sequences, you can use the automators:  

   * [**MASHtree**](analysis/mashtree.md) - creates a phylogeny using MASH distances
   * [**bcgtree**](analysis/bcgtree.md) - builds a phylogeny using bacterial core genes ("107 essential single-copy core genes")
   * **iqtree** - automator currently under development

</details>
<br>
<details>
  <summary><b>Run a genome wide association study (GWAS)</b></summary>  
  You can use the automators:  

   * [**Roary/Scoary**](analysis/roary.md) - 
   * [**Pyseer**](analysis/pyseer.md) - 

</details>
<br>

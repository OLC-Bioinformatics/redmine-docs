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
  <summary><b>Download sequence data, or share sequence data with a collaborator</b></summary> <br>
  To retrieve a zip file of your sequence data (which can also be shared with external collaborators) use automators: <a href="data/external_retrieve.md">External retrieve</a> - exports a list of sequences in a zipped file

</details>
<br>
<details>
  <summary><b>Download COWBAT reports for a list of sequence assemblies</b></summary> <br>
  To retrieve a zip file of the COWBAT assembly pipeline reports for your list of SeqIDs: <br>

<a href="data/report_retrieve.md">Report retrieve</a> - exports a csv file of the legacy_combinedMetadata for your sequences, as well as a zip file containing all reports for those sequences.

</details>
<br>
<details>
  <summary><b>Detect gene(s) in my sequences</b></summary><br>
  Automators that allow you to screen sequence(s) for gene targets: <br>

<ul>
<li><b><a href="analysis/geneseekr.md">GeneSeekr</a></b> - assemblies only</li>
<li><b><a href="analysis/kma.md">KMA</a></b> - assemblies only</li>
<li><b><a href="analysis/cardrgi.md">CARDRGI</a></b> - specific to AMR detection</li>
</ul>

</details>
<br>
<details>
  <summary><b>Inspect the contigs of my Hybrid Assembly</b></summary> <br>
  Automators that allow you to retrieve gfa (graph files) for your hybrid assemblies: 

  <ul>
  <li><b><a href="analysis/gfa_retrieve.md">gfaretrieve</a></b> - hybrid assemblies only (MIN sequences)</li>
  </ul>

</details>
<br>
<details>
  <summary><b>Determine the genus/species of a whole genome sequence assembly</b></summary> <br>
  If you are unsure of the genus/species of your isolate, you can use the automator: 

<ul>
<li><b><a href="analysis/unknown_isolate.md">Unknownisolate</a></b> -  compares WGS assembly to ATCC and RefSeq genomes, and determines rMLST type. Outputs a GROBI report for probably identity.</li>
</ul>

</details>
<br>
<details>
  <summary><b>Find the <i>X</i> most diverse sequences in a set of sequences</b></summary><br>
  If you want to find a number of diverse (e.g. distantly related) sequences from a set of sequences :  

<ul>
	<li><b><a href="analysis/diversitree.md">diversitree</a></b> - will output a list based on user's requested number of sequences.</li>
</ul>

</details>
<br>
<details>
  <summary><b>Find sequences closely related to a query sequence</b></summary><br>
  If you want to find <i>X</i> number of closest strains in a list to a query SeqID:  
  
<ul><li><b><a href="analysis/neartree.md">NearTree</a></b> - calculates the most closely related strains to your query SeqID based on MASH distance</li></ul>

</details>
<br>
<details>
  <summary><b>Determine taxonomy in metagenomes</b></summary><br>
  If you want to detect the different organisms in your metagenomic sequence, you can use the automators:  

<ul>
   <li><b><a href="analysis/metaphlan.md">Metaphlan</a></b> - more specific than Kraken2, but less sensitive.</li>
   <li><b><a href="analysis/kraken2.md">Kraken2/Bracken</a></b> - more sensitive than metaphlan, but likely to give false positives to closely related genera/species. Bracken is more accurate than Kraken2 and Metaphlan4 (according to our publication <b><a href="https://bmcmicrobiol.biomedcentral.com/articles/10.1186/s12866-023-03148-6">Cooper et al, 2023</a></b>).</li>
</ul>

</details>
<br>
<details>
  <summary><b>Find SNPs in my sequences</b></summary><br>
  To detect single nucleotide polymorphisms (SNPs) in your sequences, you can use the automators:  

<ul>
   <li><b><a href="analysis/snvphyl.md">SNVPhyl</a></b></li>
   <li><b><a href="analysis/snippy.md">Snippy</a></b></li>

</details>
<br>
<details>
  <summary><b>Annotate my sequence assemblies</b></summary><br>
  To annotate your sequence assemblies, you can use the automators:  

<ul>
<li><b><a href="analysis/prokka.md">Prokka</a></b> - whole genome annotation to identify features in gDNA (bacterial, archaeal, and viral)</li>
<li><b>Bakta</b> - automator currently under development</li>
</ul>

</details>
<br>
<details>
  <summary><b>Create a phylogeny</b></summary> <br>
  To create a phylogenetic tree from a list of sequences, you can use the automators:  

<ul>
   <li><b><a href="analysis/mashtree.md">MASHtree</a></b> - creates a phylogeny using MASH distances</li>
   <li><b><a href="analysis/bcgtree.md">bcgtree</a></b> - builds a phylogeny using bacterial core genes ("107 essential single-copy core genes")</li>
   <li><b>iqtree</b> - automator currently under development</li>
</ul>

</details>
<br>
<details>
  <summary><b>Run a genome wide association study (GWAS)</b></summary><br>
  You can use the automators:  

<ul>
   <li><b><a href="analysis/roary.md">Roary/Scoary</a></b></li>
   <li><b><a href="analysis/pyseer.md">Pyseer</a></b></li>
</ul>

</details>
<br>

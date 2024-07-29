# MLST

### What does it do?

​​The automator runs MLST analyses on provided SEQIDs. If the MLST database for the requested genus is not present on the NAS, or an update is requested, the automator downloads and prepares the database. It also runs GeneSeekr to perform MLST analyses. GeneSeekr MLST reports are uploaded and attached to the issue​.

### How do I use it?

#### Subject

In the `Subject` field, put `mlst` spelling counts, but case sensitivity doesn't.

#### Description

Include the genus on the first line `organism=[name of organism]`. Please note that both spelling AND case sensitivity are important. Here are the organisms currently available for this automator: 

<details>
  <summary><b>Full list of supported organisms</b></summary> <br>
	<ul>
	
<li>Achromobacter spp.</li>
<li>Acinetobacter baumannii#1</li>
<li>Acinetobacter baumannii#2</li>
<li>Aeromonas spp.</li>
<li>Aggregatibacter actinomycetemcomitans</li>
<li>Anaplasma phagocytophilum</li>
<li>Arcobacter spp.</li>
<li>Aspergillus fumigatus</li>
<li>Bacillus cereus</li>
<li>Bacillus licheniformis</li>
<li>Bacillus subtilis</li>
<li>Bacteroides fragilis</li>
<li>Bartonella bacilliformis</li>
<li>Bartonella henselae</li>
<li>Bartonella washoensis</li>
<li>Bordetella spp.</li>
<li>Borrelia spp.</li>
<li>Brachyspira hampsonii</li>
<li>Brachyspira hyodysenteriae</li>
<li>Brachyspira intermedia</li>
<li>Brachyspira pilosicoli</li>
<li>Brachyspira spp.</li>
<li>Brucella spp.</li>
<li>Burkholderia cepacia complex</li>
<li>Burkholderia pseudomallei</li>
<li>Campylobacter concisus/curvus</li>
<li>Campylobacter fetus</li>
<li>Campylobacter helveticus</li>
<li>Campylobacter hyointestinalis</li>
<li>Campylobacter insulaenigrae</li>
<li>Campylobacter jejuni</li>
<li>Campylobacter lanienae</li>
<li>Campylobacter lari</li>
<li>Campylobacter sputorum</li>
<li>Campylobacter upsaliensis</li>
<li>Candida albicans</li>
<li>Candida glabrata</li>
<li>Candida krusei</li>
<li>Candida tropicalis</li>
<li>Candidatus Liberibacter solanacearum</li>
<li>Carnobacterium maltaromaticum</li>
<li>Chlamydiales spp.</li>
<li>Citrobacter freundii</li>
<li>Clonorchis sinensis</li>
<li>Clostridioides difficile</li>
<li>Clostridium botulinum</li>
<li>Clostridium perfringens</li>
<li>Clostridium septicum</li>
<li>Corynebacterium diphtheriae</li>
<li>Cronobacter spp.</li>
<li>Cutibacterium acnes</li>
<li>Dichelobacter nodosus</li>
<li>Edwardsiella spp.</li>
<li>Enterobacter cloacae</li>
<li>Enterococcus faecalis</li>
<li>Enterococcus faecium</li>
<li>Escherichia coli#1</li>
<li>Escherichia coli#2</li>
<li>Flavobacterium psychrophilum</li>
<li>Gallibacterium anatis</li>
<li>Geotrichum spp.</li>
<li>Glaesserella parasuis</li>
<li>Haemophilus influenzae</li>
<li>Helicobacter cinaedi</li>
<li>Helicobacter pylori</li>
<li>Helicobacter suis</li>
<li>Kingella kingae</li>
<li>Klebsiella aerogenes</li>
<li>Klebsiella oxytoca</li>
<li>Klebsiella pneumoniae</li>
<li>Kudoa septempunctata</li>
<li>Lactobacillus salivarius</li>
<li>Lactococcus lactis bacteriophage</li>
<li>Leptospira spp.</li>
<li>Leptospira spp.#2</li>
<li>Leptospira spp.#3</li>
<li>Listeria monocytogenes</li>
<li>Macrococcus canis</li>
<li>Macrococcus caseolyticus</li>
<li>Mammaliicoccus sciuri</li>
<li>Mannheimia haemolytica</li>
<li>Melissococcus plutonius</li>
<li>Moraxella catarrhalis</li>
<li>Mycobacteria spp.</li>
<li>Mycobacteroides abscessus</li>
<li>Mycoplasma agalactiae</li>
<li>Mycoplasma anserisalpingitidis</li>
<li>Mycoplasma bovis</li>
<li>Mycoplasma flocculare</li>
<li>Mycoplasma gallisepticum#1</li>
<li>Mycoplasma gallisepticum#2</li>
<li>Mycoplasma hominis</li>
<li>Mycoplasma hyopneumoniae</li>
<li>Mycoplasma hyorhinis</li>
<li>Mycoplasma iowae</li>
<li>Mycoplasma pneumoniae</li>
<li>Mycoplasma synoviae</li>
<li>Neisseria spp.</li>
<li>Orientia tsutsugamushi</li>
<li>Ornithobacterium rhinotracheale</li>
<li>Paenibacillus larvae</li>
<li>Pasteurella multocida#1</li>
<li>Pasteurella multocida#2</li>
<li>Pediococcus pentosaceus</li>
<li>Photobacterium damselae</li>
<li>Piscirickettsia salmonis</li>
<li>Porphyromonas gingivalis</li>
<li>Pseudomonas aeruginosa</li>
<li>Pseudomonas fluorescens</li>
<li>Pseudomonas putida</li>
<li>Rhodococcus spp.</li>
<li>Riemerella anatipestifer</li>
<li>Salmonella enterica</li>
<li>Saprolegnia parasitica</li>
<li>Shewanella spp.</li>
<li>Sinorhizobium spp.</li>
<li>Staphylococcus aureus</li>
<li>Staphylococcus chromogenes</li>
<li>Staphylococcus epidermidis</li>
<li>Staphylococcus haemolyticus</li>
<li>Staphylococcus hominis</li>
<li>Staphylococcus lugdunensis</li>
<li>Staphylococcus pseudintermedius</li>
<li>Stenotrophomonas maltophilia</li>
<li>Streptococcus agalactiae</li>
<li>Streptococcus bovis/equinus complex (SBSEC)</li>
<li>Streptococcus canis</li>
<li>Streptococcus dysgalactiae equisimilis</li>
<li>Streptococcus gallolyticus</li>
<li>Streptococcus oralis</li>
<li>Streptococcus pneumoniae</li>
<li>Streptococcus pyogenes</li>
<li>Streptococcus suis</li>
<li>Streptococcus thermophilus</li>
<li>Streptococcus thermophilus#2</li>
<li>Streptococcus uberis</li>
<li>Streptococcus zooepidemicus</li>
<li>Streptomyces spp</li>
<li>Taylorella spp.</li>
<li>Tenacibaculum spp.</li>
<li>Treponema pallidum</li>
<li>Trichomonas vaginalis</li>
<li>Ureaplasma spp.</li>
<li>Vibrio cholerae</li>
<li>Vibrio cholerae#2</li>
<li>Vibrio parahaemolyticus</li>
<li>Vibrio spp.</li>
<li>Vibrio tapetis</li>
<li>Vibrio vulnificus</li>
<li>Wolbachia</li>
<li>Xylella fastidiosa</li>
<li>Yersinia pseudotuberculosis</li>
<li>Yersinia ruckeri</li>
	</ul> 
</details>
<br>
You can also choose to update the OLC database by entering `update` on the next line.

Then add the SEQID's on separate lines to match the rest of the document.

Note that FASTA files can also be attatched to the issue and be processed. This can used in addition to or instead of SEQIDs. However, there is a 5MB limit to the attatched file enforced by Redmine

#### Example

For an example mlst see [issue 34327](https://redmine.biodiversity.agr.gc.ca/issues/34327) or [issue 34328](https://redmine.biodiversity.agr.gc.ca/issues/34328)

#### Interpreting Results

If submitted correctly you will recieve a zip folder named `geneseekr_output_[issue num].zip` that contains a excel sheet you can download.

### How long does it take?

​It should take a minute or two for the automator to run depending on whether the database needs to be installed/updated. Adding more sequences will scale up the time required​ 

### What can go wrong?

A few things can go wrong with this process:

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.

2) Requested an unsupported organism. Please see the list of supported organisms above.

### Version

The databases can be updated by including the `update` argument in the description 


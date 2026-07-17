# MLST

## What does it do?

Use **MLST** to determine multilocus sequence types for supported organisms from draft genome assemblies or attached FASTA files.

The automator prepares the requested MLST database when it is not already available on the NAS or when an update is requested. It then runs GeneSeekr and attaches the MLST reports to the Redmine issue.

## How do I use it?

### Subject

In the **Subject** field, enter:

```text
mlst
```

Spelling matters, but matching is not case-sensitive.

### Description

The first line must identify the exact organism or scheme:

```text
organism=SUPPORTED ORGANISM
```

The organism value is both spelling-sensitive and case-sensitive. Copy it exactly from the supported-organism list below.

Enter one `SEQID` per subsequent line:

```text
organism=Salmonella enterica
2026-SEQ-0001
2026-SEQ-0002
```

### Multiple schemes for one organism

Some organisms have more than one MLST scheme. For *Escherichia coli*:

- `Escherichia coli#1` selects the Achtman scheme;
- `Escherichia coli#2` selects the Pasteur scheme.

Example:

```text
organism=Escherichia coli#1
2026-SEQ-0001
```

### Database update

Add `update` on the line after `organism=...` to request preparation of the current database used by the automator:

```text
organism=Listeria monocytogenes
update
2026-SEQ-0001
```

Updating or installing a database increases runtime.

### Attachments

FASTA files can be attached and processed either instead of, or in addition to, `SEQID`s.

Redmine enforces a 5 MB limit for attached files. Use an available `SEQID` rather than an attachment when the FASTA file exceeds that limit.

### Optional parameters

The supplied documentation does not identify options other than the `update` command and the exact `organism` or scheme selection.

### Examples

See [issue 34327](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/34327) and [issue 34328](https://redmine-dev.cloud-nuage.inspection.gc.ca/issues/34328) for example MLST requests.

## Supported organisms and schemes

Use the exact value shown below after `organism=`.

```text
Achromobacter spp.
Acinetobacter baumannii#1
Acinetobacter baumannii#2
Aeromonas spp.
Aggregatibacter actinomycetemcomitans
Anaplasma phagocytophilum
Arcobacter spp.
Aspergillus fumigatus
Bacillus cereus
Bacillus licheniformis
Bacillus subtilis
Bacteroides fragilis
Bartonella bacilliformis
Bartonella henselae
Bartonella washoensis
Bordetella spp.
Borrelia spp.
Brachyspira hampsonii
Brachyspira hyodysenteriae
Brachyspira intermedia
Brachyspira pilosicoli
Brachyspira spp.
Brucella spp.
Burkholderia cepacia complex
Burkholderia pseudomallei
Campylobacter concisus/curvus
Campylobacter fetus
Campylobacter helveticus
Campylobacter hyointestinalis
Campylobacter insulaenigrae
Campylobacter jejuni
Campylobacter lanienae
Campylobacter lari
Campylobacter sputorum
Campylobacter upsaliensis
Candida albicans
Candida glabrata
Candida krusei
Candida tropicalis
Candidatus Liberibacter solanacearum
Carnobacterium maltaromaticum
Chlamydiales spp.
Citrobacter freundii
Clonorchis sinensis
Clostridioides difficile
Clostridium botulinum
Clostridium perfringens
Clostridium septicum
Corynebacterium diphtheriae
Cronobacter spp.
Cutibacterium acnes
Dichelobacter nodosus
Edwardsiella spp.
Enterobacter cloacae
Enterococcus faecalis
Enterococcus faecium
Escherichia coli#1
Escherichia coli#2
Flavobacterium psychrophilum
Gallibacterium anatis
Geotrichum spp.
Glaesserella parasuis
Haemophilus influenzae
Helicobacter cinaedi
Helicobacter pylori
Helicobacter suis
Kingella kingae
Klebsiella aerogenes
Klebsiella oxytoca
Klebsiella pneumoniae
Kudoa septempunctata
Lactobacillus salivarius
Lactococcus lactis bacteriophage
Leptospira spp.
Leptospira spp.#2
Leptospira spp.#3
Listeria monocytogenes
Macrococcus canis
Macrococcus caseolyticus
Mammaliicoccus sciuri
Mannheimia haemolytica
Melissococcus plutonius
Moraxella catarrhalis
Mycobacteria spp.
Mycobacteroides abscessus
Mycoplasma agalactiae
Mycoplasma anserisalpingitidis
Mycoplasma bovis
Mycoplasma flocculare
Mycoplasma gallisepticum#1
Mycoplasma gallisepticum#2
Mycoplasma hominis
Mycoplasma hyopneumoniae
Mycoplasma hyorhinis
Mycoplasma iowae
Mycoplasma pneumoniae
Mycoplasma synoviae
Neisseria spp.
Orientia tsutsugamushi
Ornithobacterium rhinotracheale
Paenibacillus larvae
Pasteurella multocida#1
Pasteurella multocida#2
Pediococcus pentosaceus
Photobacterium damselae
Piscirickettsia salmonis
Porphyromonas gingivalis
Pseudomonas aeruginosa
Pseudomonas fluorescens
Pseudomonas putida
Rhodococcus spp.
Riemerella anatipestifer
Salmonella enterica
Saprolegnia parasitica
Shewanella spp.
Sinorhizobium spp.
Staphylococcus aureus
Staphylococcus chromogenes
Staphylococcus epidermidis
Staphylococcus haemolyticus
Staphylococcus hominis
Staphylococcus lugdunensis
Staphylococcus pseudintermedius
Stenotrophomonas maltophilia
Streptococcus agalactiae
Streptococcus bovis/equinus complex (SBSEC)
Streptococcus canis
Streptococcus dysgalactiae equisimilis
Streptococcus gallolyticus
Streptococcus oralis
Streptococcus pneumoniae
Streptococcus pyogenes
Streptococcus suis
Streptococcus thermophilus
Streptococcus thermophilus#2
Streptococcus uberis
Streptococcus zooepidemicus
Streptomyces spp
Taylorella spp.
Tenacibaculum spp.
Treponema pallidum
Trichomonas vaginalis
Ureaplasma spp.
Vibrio cholerae
Vibrio cholerae#2
Vibrio parahaemolyticus
Vibrio spp.
Vibrio tapetis
Vibrio vulnificus
Wolbachia
Xylella fastidiosa
Yersinia pseudotuberculosis
Yersinia ruckeri
```

This list reflects the supplied documentation. Because MLST schemes change over time, confirm the current scheme list before treating it as permanent.

## Interpreting results

When MLST finishes, it uploads an archive named using the Redmine issue number:

```text
geneseekr_output_<issue number>.zip
```

The archive contains an Excel report generated by the GeneSeekr MLST analysis.

Interpret the reported sequence type according to the selected organism and scheme. Results from different schemes for the same organism are not interchangeable; record whether the Achtman, Pasteur, or another numbered scheme was used.

When `update` is requested, the issue may report the database version used. Record that version when results need to be reproducible.

## How long does it take?

MLST generally takes one to two minutes, depending on whether the selected database must be installed or updated. Runtime increases with the number of sequences and attachments.

## What can go wrong?

### A requested `SEQID` is unavailable

**Symptom:** The Redmine issue receives a warning identifying unavailable assemblies.

**Likely cause:** The automator cannot locate the requested sequence assembly.

**What to do:** Verify each `SEQID` and confirm that its assembly is available.

### The organism or scheme is unsupported

**Symptom:** The issue reports that the requested MLST database cannot be selected.

**Likely cause:** The value after `organism=` is misspelled, has incorrect capitalization, or is not in the supported list.

**What to do:** Copy an exact value from the supported-organism list, including any `#1`, `#2`, or `#3` suffix.

### The wrong scheme is selected

**Symptom:** The result uses a sequence-type scheme different from the one expected for the project.

**Likely cause:** An organism with multiple schemes was requested without selecting the intended numbered value.

**What to do:** Confirm the required scheme before submission. For *E. coli*, use `Escherichia coli#1` for Achtman or `Escherichia coli#2` for Pasteur.

### An attached FASTA file is rejected

**Symptom:** Redmine does not accept the attachment or the automator cannot process it.

**Likely cause:** The file exceeds the 5 MB attachment limit or is not valid FASTA.

**What to do:** Verify FASTA formatting and file size. Use an existing `SEQID` when possible.

## Related automators

- [rMLST](rmlst.md) — performs ribosomal MLST using ribosomal protein genes and can update its database.
- [ECTyper](ectyper.md) — determines O- and H-antigen serotypes for *Escherichia coli* assemblies rather than sequence type.
- [Unknown Isolate](unknownisolate.md) — combines rMLST with MASH and ANI evidence for uncertain organism identification.

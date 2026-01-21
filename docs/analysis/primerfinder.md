# PrimerFinder

### What does it do?

The PrimerFinder performs _in silico_ PCR analyses on FASTA files.

![PrimerFinder Legacy](../img/primer_finder_legacy.png)

__PrimerFinder Legacy__ computes primer binding and amplicon statistics on FASTA formatted files using the now retired ePCR suite of tools from NCBI. 

### How do I use it?

#### Subject

In the `Subject` field, put `primer_finder`. Spelling counts, but case sensitivity does not.

#### Description

In the `Description` field, you must provide:

**Required Components**

1. `analysis=requested_analysis`

    - acceptable analyses are `custom` and `vtyper`
        - `custom` analyses require a FASTA-formatted file of primers you wish to use (see Attachments section for 
        additional details)
        
        - `vtyper` analyses will use the primer set from the vtyper tool

**And one of the following:**

1. a list of SEQIDs (one per line) - this must be after any other supplied parameters
    - __IMPORTANT:__ You can set optionally `inclusivity` and `exclusivity` panels
    - If you do not specify a panel, it will default to `inclusivity`

    example:
```
    inclusivity
    2014-SEQ-0276
    exclusivity
    2025-SEQ-0791
    2025-SEQ-0799
```


1. `database=/path/to/database.fasta`

   - A path to a (multi-)FASTA database to use instead of SEQIDs
   - the file must be present on the NAS
   - sequences will extracted from the database and saved into individual files with names taken from the FASTA headers
        - e.g. gb|CAJ32491.1|ARO:3002622|aadA6/aadA10 [Pseudomonas aeruginosa] -> gb_CAJ32491_1_ARO_3002622_aadA6_aadA10__Pseudomonas_aeruginosa_
            - Note that the following characters will be substituted for underscores: `"/", ".", "|", " ", "[", "]", "(", ")", ":", and "'"`


**Optional Components**

In order to customise your PrimerFinder analyses, several settings can be optionally modified

- Number of mismatches allowed.
    - default is `2`
    - options are `0`, `1`, `2`, `3`. Anything else will return an error
    - modify as follows:
        - `mismatches=1`
- Minimum Amplicon size
    - Default is 0 (no minimum size)
    - Maximum is 10000
    - Must be smaller than `maximum amplicon size`
    - modify as follows:
        - `minimum_amplicon_size=150`
- Maximum Amplicon size
    - Default is 1500
    - Maximum is 10000
    - Must be larger than `minimum amplicon size`
    - modify as follows:
        - `maximum_amplicon_size=1750`
- Contig breaks
    - If ipcress cannot find an amplicon, search the genome for the primers and return a positive result if they are found on separate contigs
    - default is False
    - modify as follows:
        - `contig_breaks=True`
- Probe
    - Probe sequence to be searched against amplicons generated from primer pairs
    - this can be supplied more than once
    - modify as follows:
        - `probe=CGCGTTATCATCACTGTTACCGATAGCG`

#### Attachments

For `custom` analyses, you are required to attach a FASTA-formatted file containing the primer set(s) you wish analysed. 
The file must have the following format:

    >gene1-F
    seq
    >gene1-R
    seq
    >gene2-F1
    seq
    >gene2-R1
    seq
    >gene2-F2
    seq
    >gene2-R2
    seq
    .....

You are allowed to use IUPAC degenerate bases in this file. 
__Please don't add too many degenerate bases, as the number of primers combinations can increase quickly.__

    # Dictionary of degenerate IUPAC codes
    iupac = {
        'R': ['A', 'G'],
        'Y': ['C', 'T'],
        'S': ['C', 'G'],
        'W': ['A', 'T'],
        'K': ['G', 'T'],
        'M': ['A', 'C'],
        'B': ['C', 'G', 'T'],
        'D': ['A', 'G', 'T'],
        'H': ['A', 'C', 'T'],
        'V': ['A', 'C', 'G'],
        'N': ['A', 'C', 'G', 'T'],
        '-': ['-']
    } 

#### Examples

Example PrimerFinder analyses:
 
vtyper analyses [issue 37405](https://redmine.biodiversity.agr.gc.ca/issues/37405)

custom analyses [issue 37407](https://redmine.biodiversity.agr.gc.ca/issues/37407)

vtyper analyses, no inclusivity/exclusivity defined, custom mismatches [issue 37408](https://redmine.biodiversity.agr.gc.ca/issues/37408)

custom analyses with probe [issue 37409](https://redmine.biodiversity.agr.gc.ca/issues/37409)


### How long does it take?

PrimerFinder is very fast (seconds per sample)

### What can go wrong?

1. Requested SEQIDs are not available. 
1. Not including the `program=requested_program` component, or requesting an unsupported program
1. Not including the `analysis=requested_analysis` component, or requesting an unsupported analysis
1. Specifying an unsupported number of `mismatches`
1. Incorrectly formatted `kmersize`
1. Providing a `minimum_amplicon_size` and/or `maximum_amplicon_size` with a value that is either too large or to small
1. Attaching an incorrectly formatted primer file, or not including a primer file for `custom` analyses
1. ?

If anything goes wrong, an error message explaining the error should be returned.
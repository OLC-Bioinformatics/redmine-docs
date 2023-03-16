# MetaPhlAn4

### What does it do?

MetaPhlAn is a tool for profiling the composition of microbial communities from metagenomic shotgun sequence data.

For more information, see the [MetaPhlAn 4.0 website](https://huttenhower.sph.harvard.edu/metaphlan/), and/or the tutorial describing use of [MetaPhlAn](https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4).

If you publish data using this automator, don't forget to cite the authors of the tool [Blanco-Míguez et al., 2023](https://www.nature.com/articles/s41587-023-01688-w).

### How do I use it?

#### Subject

In the `Subject` field, put `metaphlan`. Spelling counts, but case sensitivity doesn't.

#### Description

**Required Components**

You must include a list of SEQIDs one per line.

**Optional Components**

In order to customise your analyses, additional settings can be optionally modified:

- **analysis** - By default, MetaPhlAn4 automator will run analysis for relative abundance with read stats.
    - default is `rel_ab_w_read_stats` - profiling a metagenomes in terms of relative abundances and estimate the number of reads coming from each clade
    - If you want a different analysis, add the following line:
        - `analysis=rel_ab` - profiling a metagenome in terms of relative abundances
        - `analysis=reads_map`  -  mapping from reads to clades (only reads hitting a marker)
        - `analysis=clade_profiles` - normalized marker counts for clades with at least a non-null marker
        - `analysis=marker_ab_table` - normalized marker counts
        - `analysis=marker_counts` - non-normalized marker counts [use with extreme caution]
        - `analysis=marker_pres_table` - list of markers present in the sample



#### Example

For an example MetaPhlAn4 analysis, see [issue 30290](https://redmine.biodiversity.agr.gc.ca/issues/30290). The zip file has been attached to this request as an example (the ftp links expire after approx. 2 weeks).

#### Interpreting Results

MetaPhlAn4 will upload a zipped folder called `metaphlan4_output_redmineID.zip` to the ftp once it has completed. This will contain report files for MetaPhlAn4 and bracken analyses.

### How long does it take?

The time required for analysis will depend on the analysis type and number of sequences requested. 

### What can go wrong?

1) Requested SEQIDs are not available. If we can't find some of the SEQIDs that you request, you will get a warning message informing you of it.



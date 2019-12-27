# StructureFold2 <img src='assets/sf2_logo.png' align='right' width='400px' >

For information on using StructureFold2, Please consult the included manual, SF2_Manual.pdf.
If you use StructureFold2, please cite [_Tack et al._, 2018](https://www.sciencedirect.com/science/article/pii/S1046202317303535).


**Software Dependencies**
+ [Python 2.7.X](https://www.python.org/)
+ [BioPython](https://biopython.org/)
+ [Numpy](https://numpy.org/)
+ [Cutadapt](https://cutadapt.readthedocs.io/en/stable/)
+ [SAMtools](http://samtools.sourceforge.net/)
+ [Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml)

**Recommended Software**
+ [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
+ [RNAStructure](https://rna.urmc.rochester.edu/RNAstructure.html)
+ [R](https://www.r-project.org/)

## Updates and Errata

### <img src='assets/script.png' width='30px'> structure_statistics.py
structure_statisics.py replaces make_standed_csv.py and make_MFE_csv.py, with the added
functionality of being able to combine multiple directories of <.ct> files (connectivity tables) generated by batch_fold_rna.py
into a single report. Older versions of RNAStructure did not include a DeltaG value for folds performed under MFE settings, 
but the new more consistent reporting of <.ct> files allows both strandedness and DeltaG to be extracted from such 
MFE folds, thus making one module sufficient for both tasks. Fused mode (-mode F) summarizes one or multiple 
directories, generating a single <.csv> organized by transcript. When using multiple 
directories, the names of the directories must be different. If your transcript names contain underscores, 
input the number they contain (-offset). Raw mode (-mode R) instead operates on a single directory and 
generates a single <.csv> organized by the files contained in the directory, and may be of use when 
it is hard to automatically parse transcript names. By default, 'NA' will be logged as the DetlaG for <.ct> files
that do not contain a DeltaG value; this value is configurable (-na).

**Usage**
```
Summarizes MFE <.ct> files

optional arguments:
  -h, --help      show this help message and exit
  -d D [D ...]    CT directory/directories
  -mode {F,R}     Fused/Raw statistics
  -name NAME      Output file name
  -na NA          [default = NA] Null deltaG value
  -offset OFFSET  Number of Underscores in Transcript Names
```

### rtsc_coverage.py
The functionality of coverage_overlap.py has been combined into react_coverage.py for convenience and
overall organization. Coverage overlap files are now generated concurently to the coverage calculation (-ol). 
Input file selection is now more precise for complex experimental designs (-f).

**Usage**
```
Creates a <.csv> of stop coverages from <.rtsc> files

positional arguments:
  index         <.fasta> file used to generate <.rtsc>

optional arguments:
  -h, --help    show this help message and exit
  -f F [F ...]  <.rtsc> files to operate on
  -bases BASES  [default = AC] Coverage Specificity
  -name NAME    Output file name
  -ol           Create an overlap file
  -ot OT        [default = 1.0] Overlap file threshold
  -on ON        Overlap file name
```

### rtsc_downscale.py
This module offers several avenues to downscale <.rtsc> files, creating
new <.rtsc> files with only a subset of all the reverse transcriptase stops. To
multiply the total reverse transcriptase counts of each base by a given ratio (-ratio), 
use FRACTIONAL mode; fractional stops do not interfere with the reactivity calculation.
Pseudo-random removal of stops can be performed using RANDOMREAD mode; each RT stop, regardless
of the base it occurs on has specificed chance (-ratio) to be retained. RANDOMPOSITION mode
continually selects a random position along each transcript until it can remove a stop, 
removing stops in this manner until a specified ratio (-ratio) of the total original 
number of stops remain on that transcript.

**Usage**
```
Downscales <.rtsc> files.

positional arguments:
  {FRACTIONAL,RANDOMREAD,RANDOMPOSITION}

optional arguments:
  -h, --help            show this help message and exit
  -f F [F ...]          Specific <.rtsc> to operate on
  -ratio RATIO          [default = 0.50] Fraction of RT stops to retain
  -restrict RESTRICT    Limit downscaling to these specific transcripts <.txt>
  -sort                 Sort output by transcript name
```

## Planned Updates
* make_PPV_csv.py will be merged into structure_statisics.py<br><br>
* Features that are exclusive to <.react> files have been requested to be available for <.rtsc>,
or vice-versa. I.E, rather than making a separate module for react_correlation to complement rtsc_correlation,
both features will be included in a single module; modules that work on both will be prefixed with 'rx'. 
So react_correlation and rtsc_correlationwould become rx_correlation, thus 
deprecating the old module(s) entirely.<br><br>
* Excessive amounts of wildcards used in react_static_motif.py result in a very
computationally expensive and slow search, when the targeted motif may actually not be
specific, i.e. the user just wants any region of length n with a given base composition,
i.e. all stretches of only G and C of over 10 bases in length. 
A new mode supporting this will be added.<br><br>
* Add support for [STAR](https://github.com/alexdobin/STAR) into fastq_mapper.py. STAR is **much** faster than 
[Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml), and will be integrated
into the SF pipeline in the future. STAR contains much better internal logging as well. 
Bowtie2 support will be maintianed for users that do not have access to a machine
with enough RAM run STAR.<br><br>
* Hardware guides (i.e. Linux workstation builds) for those labs
looking to get a machine to do bioinformatics may be created.<br><br>
* Merge check_ligation_bias.py and rtsc_specificity.py into a new module.


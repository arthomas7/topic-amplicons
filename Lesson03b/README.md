# DADA2 pipeline in RStudio using DECIPHER and idTaxa for taxonomy assignment


The goal of this tutorial is to follow-up on Lesson 03a where Dr. Liz Suter ran though using the DADA2 in the QIIME2 pipeline on Cyverse. Both of these tutorials aim to replicate the [full example amplicon analysis](https://astrobiomike.github.io/amplicon/) from Happy Belly Bioinformatics.

**Prerequirements:**

- If you're running RStudio on Windows you'll want to setup [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

- Installation of Miniconda in order to use cutadapt from the command line [Miniconda installers](https://docs.conda.io/en/latest/miniconda.html)

 
**Tutorial:**
  
[RStudio + DADA2 Tutorial](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson03b/Amplicons_Lesson_03b.Rmd) | Video (_link coming soon_)

You can find the tutorial data files [here](https://github.com/biovcnet/amplicons-lesson-3-repo/tree/master/dada2_wd), which include:
- An .Rproj file (optional)
- The tutorial RData file (optional)
- The DECIPHER Silva 132 database file (necessary ONLY if you want to compare to our results, otherwise the latest DECIPHER Silva 138 training set is available [here](http://www2.decipher.codes/Downloads.html))

For reference, the [DADA2 Pipeline Tutorial](https://benjjneb.github.io/dada2/tutorial.html)
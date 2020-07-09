# TOPIC: Amplicons
A repository for the lessons and tutorials for the Amplicons TOPIC channel of the [BVCN](https://biovcnet.github.io/)


## Prerequisites
* [Experience with R](https://github.com/biovcnet/biovcnet.github.io/wiki/TOPIC:-R)
* [Experience with the command line](https://github.com/biovcnet/biovcnet.github.io/wiki/2.-Using-the-Command-line)
* [Experience with Cyverse](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson03a/setting.up.md) | [Setting Up Qiime2 in Cyverse: Video tutorial](https://youtu.be/zNdho4gwZ9M)


# Overview
This BVCN topic will cover:

* the different approaches for analyzing amplicon data
* how to construct count tables, alignment files, and extract taxonomy from sequencing files
* statistical and data visualization techniques for analyzing amplicon data

# Lesson 1
### Title: Intro to Amplicons
Goals

* What are amplicons?
* What are the different pipelines for processing amplicon data?
* What are the available methods for producing count tables?

[Watch the lesson](https://www.youtube.com/watch?v=XDdmSb2BvqY&feature=youtu.be)

Access the presentation [here](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson01/AmpliconsLesson1.pdf) and check out some of the links from presentation in this [table](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson01/PipelineTutorialsLinksTable.pdf).



# Lesson 2
### Coffee Hour

This lesson is an informal discussion among instructors about topics such as
* What are your preferred workflows for analyzing amplicons?
* What are your opinions on certain caveats associated with amplicon analyses?

Watch the entire session [here](https://www.youtube.com/watch?v=egkCswqQMWM&feature=youtu.be)

Or just view the discussion on particular questions:
* [How important is it to use ASVs over OTUs?](https://youtu.be/egkCswqQMWM?t=51)
  * [Callahan et al. 2017](https://www.nature.com/articles/ismej2017119)- values of AVS over OTUs
  * Summary of that paper’s main points at Happy Belly Bioinformatics [here](https://astrobiomike.github.io/misc/amplicon_and_metagen#a-note-on-otus-vs-asvs)
* [Is it still acceptable to use a 454 dataset?](https://youtu.be/egkCswqQMWM?t=504)
* [Are -omics approaches replacing amplicon approaches?](https://youtu.be/egkCswqQMWM?t=632)
* [Should we delete singletons?](https://youtu.be/egkCswqQMWM?t=1060)
  * [Edgar 2017](https://peerj.com/articles/3889/): Evidence that 97% clustering falsely inflates singletons
* [Have there been any recent advances for getting enough DNA from low biomass samples?](https://youtu.be/egkCswqQMWM?t=1313)
  * Some kit options for [50ng input](https://sequencing.roche.com/en/products-solutions/by-category/library-preparation/dna-library-preparation/kapa-hyperplus.html ) and [1ng input](https://emea.illumina.com/products/by-type/sequencing-kits/library-prep-kits/nextera-xt-dna.html)
  * It's important to account for contamination when working with low biomass! A [blog post](https://rosekantor.github.io/blog/2020-04-11-decontaminating-amplicon-seq-data) from Rose Kantor's website.
* [What kind of server do you use for running your amplicon pipeline?](https://youtu.be/egkCswqQMWM?t=1478)
* [What does your typical amplicon workflow look like?](https://youtu.be/egkCswqQMWM?t=1679)
* [Is there a difference between calling DADA2 in Qiime2 vs calling it on its own?](https://youtu.be/egkCswqQMWM?t=2106)
* [Which interface do you use for running your code?](https://youtu.be/egkCswqQMWM?t=2196)
* [How do you manage and store your data?](https://youtu.be/egkCswqQMWM?t=2426)
   * [SRA](https://www.ncbi.nlm.nih.gov/sra)
   * [Figshare](https://figshare.com/)
   * [Dryad](https://datadryad.org/stash)
   * [github](https://github.com/) repository
   * [Zenodo](https://zenodo.org/)
   * [OSF](https://osf.io/)
* [How do you treat your count tables to account for the way that high throughput sequencing ‘collects’ data (ie. as relative abundances rather than absolute counts)?](https://youtu.be/egkCswqQMWM?t=2908)





# Lesson 3 
## Title: Pipelines for 16S rRNA Analyses 

*These are a set of tutorials that replicate the amplicon analysis from [Happy Belly](https://astrobiomike.github.io/amplicon/) using different pipeline/ software combinations. The tutorial from Happy Belly implements DADA2 in R and uses a naive Bayes classifer for taxonomy assignment.* 


### Lesson3a: QIIME2 + DADA2
*This tutorial implements DADA2 in QIIME2 uses a naive Bayes classifer for taxonomy assignment.*  
* [Watch the lesson](https://youtu.be/2kvdLbbKcJc).  
* [Follow the tutorial](https://github.com/biovcnet/topic-amplicons/tree/master/Lesson03a).  
* Dataset and the notebook are available in this [repo](https://github.com/biovcnet/amplicons-lesson-3-repo/tree/master).  


### Lesson 3b: RStudio + DADA2 (and DECIPHER)
*This tutorial implements DADA2 in R and uses DECIPHER for taxonomy assignment.*  
* [Watch the lesson](https://youtu.be/NzHc8HFEtlw).  
* [Follow the tutorial](https://github.com/biovcnet/topic-amplicons/tree/master/Lesson03b).  
* Dataset and DECIPHER training set are available in this [repo](https://github.com/biovcnet/amplicons-lesson-3-repo/tree/master/dada2_wd).

# Lesson 4 
### Title: Preparing Output from Amplicon Pipelines for Analysis

Goals

* Importing files into R
* Removing contamination
* Checking sequencing depth with rarefaction curves
* Removing singletons
* Normalization
* Exporting formatted files from R

Links

* [Watch the lesson](https://www.youtube.com/watch?v=LJZn25EiHDo&feature=youtu.be)
* [Follow the tutorial](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson04/BioVCN_Amplicons_Lesson04.Rmd)
* Input data and lesson material are available in this [repo](https://github.com/biovcnet/topic-amplicons/tree/master/Lesson04)

# Lesson 5 
## Title: Ordinations 
 
### Lesson 5a: Ordinations I 
R crossover tutorial, see [topic-R lesson 8](https://github.com/biovcnet/topic-R) for more. Using the output from qiime2 analysis in Lesson 4


Goals

* Import ASV table into phyloseq
* Explore functionality of phyloseq: making tree, re-rooting tree, bar plot of taxa
* Hellinger transformations
* Ordinations with phyloseq: 
  * PCoA with Bray-Curtis distance matrices
  * PCoA with weighted UniFrac 
  * NMDS with Bray-Curtis distance matrices

Links

* [Watch the lesson](https://youtu.be/OHIL1TDLLt0)
* [Follow the tutorial](https://github.com/biovcnet/topic-R/blob/master/Lesson-8/lesson8.Rmd)
* Input data and lesson material are available in this [repo](https://github.com/biovcnet/topic-R/tree/master/Lesson-8)


Primary tools/programs used:
* [phyloseq](https://joey711.github.io/phyloseq/)
   * [Citation](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0061217)
   
### Lesson 5b: Ordinations II
R crossover tutorial, see [topic-R lesson 8](https://github.com/biovcnet/topic-R) for more. 

Goals

* Import and manipulate ASV tables using tidyverse
* Log-ratio transformations
* Ordinations with vegan:
  * Screeplots
  * PCA plots
  * PCoA with Jaccard and Euclidean distance matrices
  * NMDS with Jaccard and Euclidean distance matrices
 
Links
* [Watch the lesson](https://www.youtube.com/watch?v=lSgwJBPW88k&feature=youtu.be)
* [Follow the tutorial](https://github.com/biovcnet/topic-R/blob/master/Lesson-8b/lesson-08b-ordinationsII.R)
* Input data and lesson material are available in this [repo](https://github.com/biovcnet/topic-R/tree/master/Lesson-8b)

Primary tools/programs used:
* [vegan](https://cran.r-project.org/web/packages/vegan/vegan.pdf)
* [compositions](https://cran.r-project.org/web/packages/compositions/compositions.pdf)

### Lesson 5c: Ordinations III
*Continuation of lesson 5b*



   


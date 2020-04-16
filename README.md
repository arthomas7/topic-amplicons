# TOPIC: Amplicons
A repository for the lessons and tutorials for the Amplicons TOPIC channel of the [BVCN](https://biovcnet.github.io/)


## Prerequisites
* [Experience with R](https://github.com/biovcnet/biovcnet.github.io/wiki/TOPIC:-R)
* [Experience with the command line](https://github.com/biovcnet/biovcnet.github.io/wiki/2.-Using-the-Command-line)

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
### Coffe Hour

This lesson is an informal discussion among instructors about topics such as
* What are your preferred workflows for analyzing amplicons?
* What are your opinions on certain caveats associated with amplicon analyses?

[Watch the entire session here](https://www.youtube.com/watch?v=egkCswqQMWM&feature=youtu.be)

Or just view the discussion on particular questions:

* [How important is it to use ASVs over OTUs?](https://youtu.be/egkCswqQMWM?t=51)
  * [Callahan et al. 2017](https://www.nature.com/articles/ismej2017119)- values of AVS over OTUs
   * Summary of that paper’s main points at Happy Belly Bioinformatics [here](https://astrobiomike.github.io/misc/amplicon_and_metagen#a-note-on-otus-vs-asvs)

* [Is it still acceptable to use a 454 dataset?](https://youtu.be/egkCswqQMWM?t=504)

* [Are -omics approaches replacing amplicon approaches?](https://youtu.be/egkCswqQMWM?t=632)

* [Should we delete singletons?](https://youtu.be/egkCswqQMWM?t=1060)
  * [Edgar 2017](https://peerj.com/articles/3889/): Evidence that 97% clustering falsely inflates singletons

* [Have there been any recent advances for getting enough DNA from low biomass samples?](https://youtu.be/egkCswqQMWM?t=1313)
  * Some kit options:
   * 50ng input: [KAPA HyperPlus Kits, Roche](https://sequencing.roche.com/en/products-solutions/by-category/library-preparation/dna-library-preparation/kapa-hyperplus.html )
   * 1ng input: [Nextera XT DNA Library Preparation Kit, Illumina](https://emea.illumina.com/products/by-type/sequencing-kits/library-prep-kits/nextera-xt-dna.html)
  * It's important to account for contamination when working with low biomass! A [blog post](https://rosekantor.github.io/blog/2020-04-11-decontaminating-amplicon-seq-data) from Rose Kantor's website.
  * 
  

* [What kind of server do you use for running your amplicon pipeline?](https://youtu.be/egkCswqQMWM?t=1478)

* [What does your typical amplicon workflow look like?](https://youtu.be/egkCswqQMWM?t=1679)

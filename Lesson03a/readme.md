# Replicating the Happy Belly Amplicon Analysis in QIIME2


The goal of this tutorial is to replicate the original [amplicons tutorial](https://astrobiomike.github.io/amplicon/) from Happy Belly, which is an analysis of a microbiome dataset from the East Pacific Rise (see the publication [here](https://www.frontiersin.org/articles/10.3389/fmicb.2015.01470/full)). In the original tutorial, DADA2 is implemented in R. Here, DADA2 is implemented in a Qiime2 pipeline. This was working in an QIIME2 installation natively on my laptop but I am also writing this tutorial out specifically so it can be implemented in Cyverse.

[Cyverse](https://cyverse.org/) is a cyberinfrastructure funded by the NSF Directorate for Biological Sciences that can be used for running large-scale computational analyses. Before beginning this tutorial, you will need to make a Cyverse account.

There are several different services within Cyverse, including Atmosphere, the Discovery Environment, and DNA Subway, which all have existing QIIME2 setups. Here, I am going to use the Discovery Environment. If you want to see how to implement QIIME2 in Atmosphere, [here](https://github.com/joslynnlee/qiime2-workflow-cyverse/wiki) is a nice tutorial. The [DNA Subway](https://dnasubway.cyverse.org/) Purple Line option is also useful for those who want a QIIME2 pipeline without too much coding.


I primarily patched this together using [this useful tutorial](https://cyverse-jupyter-qiime2.readthedocs-hosted.com/en/latest/) and [this video webinar](https://www.youtube.com/watch?time_continue=561&v=9AT2YHkduz0&feature=emb_logo) for setting up and using the QIIME2 Jupyter notebook in the Cyverse environment. You can follow the Cyverse example to learn how to use the Discovery Environment and test out some of their sample notebooks using their example data, or you can follow along here where I import the Happy Belly data and start a QIIME2 analysis from scratch.

Next steps:

  1- [How to prepare the data for Qiime2](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson03a/organize.data.md).  
  2- [How to set up the Cyverse Discovery Environment for a Qiime2 analysis using Jupyter Labs](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson03a/setting.up.md).  
  3- [The Qiime2 analysis example](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson03a/analysis.md). 

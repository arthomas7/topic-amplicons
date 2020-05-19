# Replicating the Happy Belly Amplicon Analysis in QIIME2


The goal of this tutorial is to replicate the [amplicons analysis](https://astrobiomike.github.io/amplicon/) from Happy Belly, which is an analysis of a microbiome dataset from the East Pacific Rise (see the publication [here](https://www.frontiersin.org/articles/10.3389/fmicb.2015.01470/full)). In the tutorial, DADA2 is implemented in R. Here, DADA2 is implemented in a Qiime2 pipeline. This can be run in a QIIME2 installation natively on a laptop or it can be implemented in Cyverse.

[Cyverse](https://cyverse.org/) is a cyberinfrastructure funded by the NSF Directorate for Biological Sciences that can be used for running large-scale computational analyses. Before beginning this tutorial, you will need to make a Cyverse account.

There are several different services within Cyverse, including Atmosphere, the Discovery Environment, and DNA Subway, which all have existing QIIME2 setups. Here, we are using the Discovery Environment. If you want to see some other ways of implementing QIIME2 in the Cyverse infrastrucutre,see these links:

- [QIIME2 in Atmosphere](https://github.com/joslynnlee/qiime2-workflow-cyverse/wiki) 
- [DNA Subway](https://dnasubway.cyverse.org/) 


This lesson was primarily put together based on [this useful tutorial](https://cyverse-jupyter-qiime2.readthedocs-hosted.com/en/latest/) and [this video webinar](https://www.youtube.com/watch?time_continue=561&v=9AT2YHkduz0&feature=emb_logo) for setting up and using the QIIME2 Jupyter notebook in the Cyverse environment. 

Steps:
  1- [Set up the Cyverse Discovery Environment for a Qiime2 analysis using Jupyter Labs](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson03a/setting.up.md). | [Video](https://youtu.be/zNdho4gwZ9M)
  
  2- [How to prepare the Happy Belly data for QIIME2](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson03a/organize.data.md).  
  
  3- [QIIME2 analysis example](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson03a/analysis.md). 

## Jump into the analysis

<u>If you are following along on the VICE app in Cyverse</u>, qiime2 should already be installed, check it:

From a Jupyter Python Notebook:
```
! qiime --help
```

This environment also come pre-loaded with a plugin that let's us view qiime2 graphics in the notebook directly. We need to import that:

```
import qiime2 as q2
```


<u>If you are following along in your local terminal</u>, first you need to activate qiime:  
(Remember, don't use the `!`. You may also have a different suffix if you are working with a different version)  
```
source activate qiime2-2020.2
```

And check the installation:
```
qiime --help
```

______
*If you are in the VICE app, the rest of the tutorial is also availabl in the Jupyter notebook...*


If those are working, we can start importting the fastq files:

```
! qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path qiime2_dada2_amplicon_ex_workflow/qiime_import \
  --input-format CasavaOneEightSingleLanePerSampleDirFmt \
  --output-path demux-paired-end.qza

```



**Remove primers**: we want to call the same program as in Happy Belly, cutadapt, but call it from within qiime2 (documentation [here](https://github.com/qiime2/q2-cutadapt)). The [syntax](https://docs.qiime2.org/2020.2/plugins/available/cutadapt/?highlight=cutadapt) is a little different in qiime2 but we want to do the same thing. Here, we want to use the `trim-paired` method since these sequences are paired-end and already demulitplexed.  

I also chose to use 8 cores and this went super fast. On my local computer, I only have 2 and it takes a bit longer. 



```

# from forward reads, delete forward primer from 5' end with p-front-f 
# and delete the reverse complement of reverse primer from 3' end with p-adapter-f

# from reverse reads, delete reverse primer from 5' end with p-front-r
# and delete the reverse complement of the forward primer from the 3' with p-adapter-r


! qiime cutadapt trim-paired \
--i-demultiplexed-sequences demux-paired-end.qza \
--p-cores 8 \
--p-front-f GTGCCAGCMGCCGCGGTAA \
--p-adapter-f ATTAGAWACCCBDGTAGTCC \
--p-front-r GGACTACHVGGGTWTCTAAT \
--p-adapter-r TTACCGCGGCKGCTGGCAC \
--o-trimmed-sequences demux-paired-end-trimmed.qza \
--verbose &> primer_trimming.log 

```

Scan through `primer_trimming.log` file and check that trimming makes sense. You'll see the majority of reads from all files (>99%) were trimmed and you can look at the breakdown file-by-file.

**Check quality plots**. Qiime2 works with special 'qzv' files for data visualizations. To read a much better description of these, see this [page](https://docs.qiime2.org/2020.2/concepts/#data-files-visualizations). 

```
! qiime demux summarize \
--i-data demux-paired-end-trimmed.qza \
--o-visualization demux-paired-end-trimmed.qzv \
```

**Visualize**

<u>If you are working in your local terminal</u>, you can view the above file using the command `qiime tools view demux-paired-end-trimmed.qzv`. An html will pop up with the interactive plot. You can also drag the file directly into [this website](https://view.qiime2.org/).

<u>If you are working in the VICE app</u>, one of the benefits is that qiime2 visualization tool that we imported above. We can view the interactive plot directly in this notebook:

```
q2.Visualization.load("demux-paired-end-trimmed.qzv")
```


**DADA2**
Use DADA2 to denoises paired-end sequences, dereplicate them, and filter chimeras.  

- First #make an output directory

	```
	mkdir DADA2_denoising_output
	```
	This will hold a `denoising_stats.qza` (summary of the denoising results), a `representative_sequences.qza` (the sequences of the ASVs, aka 'features') and a joined paired-end reads, `table.qza`, (the feature table or OTU table), which has the paired sequence joined.  


- Check the documentation on dada2 in qiime2:  
```!qiime dada2```  

	We are going to use the denoise-paired option. 

- Next check which input parameters we can modfiy in that option:  
	```!qiime dada2 denoise-paired ```
	
Let's choose some parameters. I am going to get as close as I can to the same parameters used in Happy Belly:

- --p-trunc-len-f: we are going to truncate the F reads at position 250 
- --p-trunc-len-r: and truncate the R reads at position 200  
- Reads shorter than these will be thrown out

- Dada2 also filters out reads using maxEE, a threshold number of expected errors in each read
    - In Happy Belly, they threw out reads if they were likely to have more than 2 erroneous base calls 
    - Here, the documentation says 2 is the default so I won't even put that parameter in. 
    - If you wanted to change it, you would use the --p-max-ee-f and --p-max-ee-r arguments

- I won't be trimming the left side of either F or R reads so I am also leaving out the --p-trim-left-f and --p-trim-left-r parameters

- --p-trunc-q can be used to truncate reads when the quality score drops below a certain threshold. 
    - The default number is 2, which is the value I want so I also won't be assigning that parameter         
- --p-chimera-method: I will also leave the default 'consensus' method

- --p-n-threads I am going to put 0 here, which means use all available threads.

- --p-n-reads-learn DADA2 tries to estimate how many errors were generated by the sequencer based on an error model. This parameters sets how many sequences go into training the model. Leave the default here as 1,000,000 for now


- --verbose will show us what's happening as it's running.


**Note**: This is the longest step and may take a while. For a larger dataset, it may take several hours or days and it may be smart to use [tmux](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) incase you lose connection. tmux allows your commands to keep running on the server even if you lose connection (I have not used it here on Cyverse so it's sometihng you would need to inquire with them about if you are having issues).


```
! qiime dada2 denoise-paired \
--i-demultiplexed-seqs demux-paired-end-trimmed.qza \
--p-trunc-len-f 250 \
--p-trunc-len-r 200 \
--p-n-threads 0 \
--output-dir DADA2_denoising_output \
--verbose
```

Just in case, while that's running, I downloaded some of my analysis files and my JupyterLabs notebook file to my local environment.  

Let's take a look at the results 

```
! qiime metadata tabulate \
  --m-input-file DADA2_denoising_output/denoising_stats.qza \
  --o-visualization DADA2_denoising_output/denoising_stats.qzv

q2.Visualization.load("DADA2_denoising_output/denoising_stats.qzv")
```

- Remember if you are working in terminal, the syntax for viewing files is different


Compare the above table to the `summary_tab` table from Happy Belly.  

It looks like we had a few more input sequences, which was probably due to slightly different cutadapt parameters (we couldn't set the max length cutoff) but then the filtering steps here removed several more reads than in Happy Belly. This could be due to the method we used (eg. 'consensus' method for chimera removal instead of 'pool') or some other parameters that we were unable to set through the qiime2 input parameters (eg. setting a min length of 175). Still, it seems that what we have here is conservative, so I am going to continue...



Take a look at a summary of the remaining sequence reads (note- in the outpute of the rep_seq visualization you can click directly on a sequence to send it to Blast)

```
! qiime feature-table tabulate-seqs \
--i-data DADA2_denoising_output/representative_sequences.qza \
--o-visualization DADA2_denoising_output/rep_seqs.qzv \

q2.Visualization.load("DADA2_denoising_output/rep_seqs.qzv")
```

And take a look at the feature table

```
! qiime feature-table summarize \
--i-table DADA2_denoising_output/table.qza \
--o-visualization DADA2_denoising_output/table.qzv \

q2.Visualization.load("DADA2_denoising_output/table.qzv")
```

### Assign taxonomy 

We are going to assign taxonomy based on a reference database. These reference databases are called "classifiers" and drawn from curated databases, such as the one from [SILVA](https://www.arb-silva.de/).

You can dowload the classifier directly or train it yourself. Since the dataset we are working with uses common primers, there are classifiers that already exist.  If you are using other primers, or want to train your own reference database for some other reason, you should follow the instructions on this [page](https://docs.qiime2.org/2020.2/tutorials/feature-classifier/).

Here we are going to use  SILVA v132 as our reference database. This is the most recent qiime2-compatible classifier and is the same one used in Happy Belly. I am just going to download the 16S rRNA v4 (515F/806R) v132 classifier from [here](https://docs.qiime2.org/2020.2/data-resources/).

```
! wget https://data.qiime2.org/2020.2/common/silva-132-99-515-806-nb-classifier.qza
```

Next, assign taxononomy using the SILVA classifier (this step also takes awhile...)

```
# Classify the representative sequences
! qiime feature-classifier classify-sklearn \
--i-classifier silva-132-99-515-806-nb-classifier.qza \
--i-reads DADA2_denoising_output/representative_sequences.qza \
--o-classification taxonomy.qza
```

That gave an error! It's because I downloaded the newest classifier but we are working in a slightly older version of Qiime2. If you go back to the [data resources page](https://docs.qiime2.org/2020.2/data-resources/) and switch the version (upper left corner) to 2019.10 (our current working version) and get the link for *that* v132 515F/806R classifier, it should work


```
! rm silva-132-99-515-806-nb-classifier.qza -r
```

```
! wget https://data.qiime2.org/2019.10/common/silva-132-99-515-806-nb-classifier.qza
```

```
# And try to assign taxonomy again

# Classify the representative sequences
! qiime feature-classifier classify-sklearn \
--i-classifier silva-132-99-515-806-nb-classifier.qza \
--i-reads DADA2_denoising_output/representative_sequences.qza \
--o-classification taxonomy.qza
```

.....to be continued
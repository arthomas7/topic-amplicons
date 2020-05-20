# QIIME2 + DADA2- Jupyter Labs Demo
## Amplicons Lesson 3a

This is a demo of running QIIME2 in a JupyterLab in the Cyverse Discovery Environment, utilizinf DADA2 to call ASVs.

This analysis replicates AstrobioMike's [amplicon analysis tutorial](https://astrobiomike.github.io/amplicon/dada2_workflow_ex#the-data) but by substituting a QIIME2 pipeline.  

Before starting this tutorial, you should:

- have a Cyverse account with the QIIME2 app set up in Discovery environment  
OR
- a local installation of [QIIME2](https://qiime2.org/)

You will also need [the modified Happy Belly data files](https://github.com/biovcnet/amplicons-lesson-3-repo) from the data store loaded into your working environment. 

The instructions for setting up these prerequirements can be found at the BVCN amplicons Lesson 3a [page](https://github.com/biovcnet/topic-amplicons/tree/master/Lesson03a).

April-May 2020   
Liz Suter  
BVCN



_____


If you have been following along with the BVCN Amplicons Lesson 3a tutorial, then you should be able to see the `qiime2_wd` folder in our current environment, which has all of our fastq files in qiime2 format.


Make a 'work' directory where all the generated files for this analysis will go. Move this notebook to that folder as well.


```python
# mkdir work/
```

Recall the difference between running normal Python commands, bash commands (using `!`), and iCommands in the Discovery Environment app:


```python
ls
```

    [0m[01;34mDADA2_denoising_output[0m/       downloads.stderr.log
    HappyBellyqiime2Demo.ipynb    downloads.stdout.log
    [01;34mclassified_sequences[0m/         [01;34mexport[0m/
    demux-paired-end-trimmed.qza  [01;34mphylogeny[0m/
    demux-paired-end-trimmed.qzv  silva-132-99-515-806-nb-classifier.qza
    demux-paired-end.qza



```python
! ls
```

    DADA2_denoising_output	      downloads.stderr.log
    HappyBellyqiime2Demo.ipynb    downloads.stdout.log
    classified_sequences	      export
    demux-paired-end-trimmed.qza  phylogeny
    demux-paired-end-trimmed.qzv  silva-132-99-515-806-nb-classifier.qza
    demux-paired-end.qza



```python
! ils
```

    /iplant/home/esuter:
      C- /iplant/home/esuter/BVCN
      C- /iplant/home/esuter/HREMicrobiomeproject2019_LargeFiles
      C- /iplant/home/esuter/analyses
      C- /iplant/home/esuter/export
      C- /iplant/home/esuter/sci_data


If you are coming back here after working on an earlier version, you can upload the previous version of your notebook and all of your files using `iget`. For example:


```python
# ! iget /iplant/home/esuter/analyses/Jupyter_Lab_QIIME2_2019.10_analysis1-2020-05-05-14-09-52.8/work -r

# the -r mean recursive, so it grabs everything in all folder levels below
```

_________

### Check the QIIME2 installation


```python
! qiime info
```

    [32mSystem versions[0m
    Python version: 3.6.7
    QIIME 2 release: 2019.10
    QIIME 2 version: 2019.10.0
    q2cli version: 2019.10.0
    [32m
    Installed plugins[0m
    alignment: 2019.10.0
    composition: 2019.10.0
    cutadapt: 2019.10.0
    dada2: 2019.10.0
    deblur: 2019.10.0
    demux: 2019.10.0
    diversity: 2019.10.0
    emperor: 2019.10.0
    feature-classifier: 2019.10.0
    feature-table: 2019.10.0
    fragment-insertion: 2019.10.0
    gneiss: 2019.10.0
    longitudinal: 2019.10.0
    metadata: 2019.10.0
    phylogeny: 2019.10.0
    quality-control: 2019.10.0
    quality-filter: 2019.10.0
    sample-classifier: 2019.10.0
    taxa: 2019.10.0
    types: 2019.10.0
    vsearch: 2019.10.0
    [32m
    Application config directory[0m
    /home/qiime2/q2cli[0m
    [32m
    Getting help[0m
    To get help with QIIME 2, visit https://qiime2.org[0m


In the Discovery Environment, we will also use a Python tool called qiime2. This is a pre-loaded plugin that let's us view qiime2 graphics in the notebook directly. We need to import this using python:


```python
import qiime2 as q2
# This is now a Python item (instead of bash) and doesn't need to be preceeded by `!`. We will use it a few lines down.
```

### Import fastq files into qiime format


```python
# Import fastq files into a qiime2 qza file format

! qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path qiime2_wd/qiime_import \
  --input-format CasavaOneEightSingleLanePerSampleDirFmt \
  --output-path work/demux-paired-end.qza
```

    [32mImported qiime2_wd/qiime_import as CasavaOneEightSingleLanePerSampleDirFmt to demux-paired-end.qza[0m


### Remove primers 
We want to call the same program as in Happy Belly, cutadapt, but call it from within qiime2 (documentation [here](https://github.com/qiime2/q2-cutadapt)). The [syntax](https://docs.qiime2.org/2020.2/plugins/available/cutadapt/?highlight=cutadapt) is a little different in qiime2 but we want to do the same thing. Here, we want to use the `trim-paired` method since these sequences are paired-end and already demulitplexed.

Check out how to set the parameters:


```python
! qiime cutadapt trim-paired
```

    Usage: [34mqiime cutadapt trim-paired[0m [OPTIONS]
    
      Search demultiplexed paired-end sequences for adapters and remove them.
      The parameter descriptions in this method are adapted from the official
      cutadapt docs - please see those docs at https://cutadapt.readthedocs.io
      for complete details.
    
    [1mInputs[0m:
      [34m[4m--i-demultiplexed-sequences[0m ARTIFACT [32m[0m
        [32mSampleData[PairedEndSequencesWithQuality][0m
                              The paired-end sequences to be trimmed.   [35m[required][0m
    [1mParameters[0m:
      [34m--p-cores[0m INTEGER       Number of CPU cores to use.
        [32mRange(1, None)[0m                                                [35m[default: 1][0m
      [34m--p-adapter-f[0m TEXT...   Sequence of an adapter ligated to the 3' end. The
        [32mList[Str][0m             adapter and any subsequent bases are trimmed. If a
                              `$` is appended, the adapter is only found if it is
                              at the end of the read. Search in forward read. If
                              your sequence of interest is "framed" by a 5' and a
                              3' adapter, use this parameter to define a "linked"
                              primer - see https://cutadapt.readthedocs.io for
                              complete details.                         [35m[optional][0m
      [34m--p-front-f[0m TEXT...     Sequence of an adapter ligated to the 5' end. The
        [32mList[Str][0m             adapter and any preceding bases are trimmed. Partial
                              matches at the 5' end are allowed. If a `^`
                              character is prepended, the adapter is only found if
                              it is at the beginning of the read. Search in
                              forward read.                             [35m[optional][0m
      [34m--p-anywhere-f[0m TEXT...  Sequence of an adapter that may be ligated to the
        [32mList[Str][0m             5' or 3' end. Both types of matches as described
                              under `adapter` and `front` are allowed. If the
                              first base of the read is part of the match, the
                              behavior is as with `front`, otherwise as with
                              `adapter`. This option is mostly for rescuing failed
                              library preparations - do not use if you know which
                              end your adapter was ligated to. Search in forward
                              read.                                     [35m[optional][0m
      [34m--p-adapter-r[0m TEXT...   Sequence of an adapter ligated to the 3' end. The
        [32mList[Str][0m             adapter and any subsequent bases are trimmed. If a
                              `$` is appended, the adapter is only found if it is
                              at the end of the read. Search in reverse read. If
                              your sequence of interest is "framed" by a 5' and a
                              3' adapter, use this parameter to define a "linked"
                              primer - see https://cutadapt.readthedocs.io for
                              complete details.                         [35m[optional][0m
      [34m--p-front-r[0m TEXT...     Sequence of an adapter ligated to the 5' end. The
        [32mList[Str][0m             adapter and any preceding bases are trimmed. Partial
                              matches at the 5' end are allowed. If a `^`
                              character is prepended, the adapter is only found if
                              it is at the beginning of the read. Search in
                              reverse read.                             [35m[optional][0m
      [34m--p-anywhere-r[0m TEXT...  Sequence of an adapter that may be ligated to the
        [32mList[Str][0m             5' or 3' end. Both types of matches as described
                              under `adapter` and `front` are allowed. If the
                              first base of the read is part of the match, the
                              behavior is as with `front`, otherwise as with
                              `adapter`. This option is mostly for rescuing failed
                              library preparations - do not use if you know which
                              end your adapter was ligated to. Search in reverse
                              read.                                     [35m[optional][0m
      [34m--p-error-rate[0m PROPORTION [32mRange(0, 1, inclusive_end=True)[0m
                              Maximum allowed error rate.           [35m[default: 0.1][0m
      [34m--p-indels[0m / [34m--p-no-indels[0m
                              Allow insertions or deletions of bases when
                              matching adapters.                   [35m[default: True][0m
      [34m--p-times[0m INTEGER       Remove multiple occurrences of an adapter if it is
        [32mRange(1, None)[0m        repeated, up to `times` times.          [35m[default: 1][0m
      [34m--p-overlap[0m INTEGER     Require at least `overlap` bases of overlap between
        [32mRange(1, None)[0m        read and adapter for an adapter to be found.
                                                                      [35m[default: 3][0m
      [34m--p-match-read-wildcards[0m / [34m--p-no-match-read-wildcards[0m
                              Interpret IUPAC wildcards (e.g., N) in reads.
                                                                  [35m[default: False][0m
      [34m--p-match-adapter-wildcards[0m / [34m--p-no-match-adapter-wildcards[0m
                              Interpret IUPAC wildcards (e.g., N) in adapters.
                                                                   [35m[default: True][0m
      [34m--p-minimum-length[0m INTEGER
        [32mRange(1, None)[0m        Discard reads shorter than specified value. Note,
                              the cutadapt default of 0 has been overridden,
                              because that value produces empty sequence records.
                                                                      [35m[default: 1][0m
      [34m--p-discard-untrimmed[0m / [34m--p-no-discard-untrimmed[0m
                              Discard reads in which no adapter was found.
                                                                  [35m[default: False][0m
    [1mOutputs[0m:
      [34m[4m--o-trimmed-sequences[0m ARTIFACT [32mSampleData[PairedEndSequencesWithQuality][0m
                              The resulting trimmed sequences.          [35m[required][0m
    [1mMiscellaneous[0m:
      [34m--output-dir[0m PATH       Output unspecified results to a directory
      [34m--verbose[0m / [34m--quiet[0m     Display verbose output to stdout and/or stderr
                              during execution of this action. Or silence output
                              if execution is successful (silence is golden).
      [34m--citations[0m             Show citations and exit.
      [34m--help[0m                  Show this message and exit.
    
    [33m                  There were some problems with the command:                  [0m
    [31m[1m (1/2) Missing option "--i-demultiplexed-sequences".[0m
    [31m[1m (2/2) Missing option "--o-trimmed-sequences".  ("--output-dir" may also be
      used)[0m



```python
# from forward reads, delete forward primer from 5' end with p-front-f 
# and delete the reverse complement of reverse primer from 3' end with p-adapter-f

# from reverse reads, delete reverse primer from 5' end with p-front-r
# and delete the reverse complement of the forward primer from the 3' with p-adapter-r

# the minimum length (set with --p-minimum-length) should be 215 
# According to Happy Belly, based roughly on 10% smaller than would be expected after trimming 
# I cannot find a parameter in the qiime2 version of cutadapt to a max legnth of 285 (like they have in Happy Belly)

#--p-discard-untrimmed states to throw away reads that don’t have the primers in them in the expected locations.

# run this as verbose so we can see the output:

! qiime cutadapt trim-paired \
--i-demultiplexed-sequences work/demux-paired-end.qza \
--p-cores 16 \
--p-front-f GTGCCAGCMGCCGCGGTAA \
--p-adapter-f ATTAGAWACCCBDGTAGTCC \
--p-front-r GGACTACHVGGGTWTCTAAT \
--p-adapter-r TTACCGCGGCKGCTGGCAC \
--p-minimum-length 215 \
--p-discard-untrimmed \
--o-trimmed-sequences work/demux-paired-end-trimmed.qza #\
#--verbose 
```

    [32mSaved SampleData[PairedEndSequencesWithQuality] to: demux-paired-end-trimmed.qza[0m


You can comment out the --verbose option to keep the notebook clean but you can run it to see the results of primer trimming. More than 99% of the sequences get trimmed for all sample files.

### Check quality of trimmed reads
Qiime2 works with special 'qzv' files for data visualizations. To read a much better description of these, see this [page](https://docs.qiime2.org/2020.2/concepts/#data-files-visualizations). 


```python
# Use the summarize function to summarize the trimmed reads
! qiime demux summarize \
--i-data work/demux-paired-end-trimmed.qza \
--o-visualization work/demux-paired-end-trimmed.qzv
```

    [32mSaved Visualization to: demux-paired-end-trimmed.qzv[0m


#### Visualize

<u>If you are working in your local terminal</u>, you can view the above file using the command `qiime tools view demux-paired-end-trimmed.qzv`. An html will pop up with the interactive plot. You can also drag the file directly into [this website](https://view.qiime2.org/).

<u>If you are working in the VICE app</u> in a Jupyter notebook, one of the benefits is that qiime2 visualization tool that we imported above. We can view the interactive plot directly in this notebook:


```python
ls
```

    [0m[01;34mDADA2_denoising_output[0m/       demux-paired-end.qza
    HappyBellyqiime2Demo.ipynb    downloads.stderr.log
    [01;34mclassified_sequences[0m/         downloads.stdout.log
    demux-paired-end-trimmed.qza  silva-132-99-515-806-nb-classifier.qza
    demux-paired-end-trimmed.qzv



```python
q2.Visualization.load("demux-paired-end-trimmed.qzv")
```




<div><img onload="(function(div, url){
if (typeof require !== 'undefined') {
    var baseURL = require.toUrl('').split('/').slice(0, -2).join('/');
} else {
    var baseURL = JSON.parse(
        document.getElementById('jupyter-config-data').innerHTML
    ).baseUrl.slice(0, -1);
}
url = baseURL + url;
fetch(url).then(function(res) {
    if (res.status === 404) {
        div.innerHTML = 'Install QIIME 2 Jupyter extension with:<br />' +
                        '<code>jupyter serverextension enable --py qiime2' +
                        ' --sys-prefix</code><br />then restart your server.' +
                        '<br /><br />(Interactive output not available on ' +
                        'static notebook viewer services like nbviewer.)';
    } else if (res.status === 409) {
        div.innerHTML = 'Visualization no longer in scope. Re-run this cell' +
                        ' to see the visualization.';
    } else if (res.ok) {
        url = res.url;
        div.innerHTML = '<iframe src=\'' + url + '\' style=\'' +
                        'width: 100%; height: 700px; border: 0;\'>' +
                        '</iframe><hr />Open in a: <a href=\'' + url + '\'' +
                        ' target=\'_blank\'>new window</a>'
    } else {
        div.innerHTML = 'Something has gone wrong. Check notebook server for' +
                        ' errors.';
    }
});
})(this.parentElement, '/qiime2/redirect?location=/tmp/qiime2-archive-z8xsels_')" src="data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==" /></div>



  
  
Click around in the above interactive plot quality scores at different positions, number of sequences in different samples, etc.


From the visualization above, we can see the same things that are laid out in the Happy Belly tutorial: that, for the forward reads, the quality really drops off at about 250bp and for the reverse reads, at about 200.

### DADA2 
Use DADA2 to denoises paired-end sequences, dereplicate them, and filter chimeras, all in one step.

Unlike in the Happy Belly environment, where using DADA2 in R let's you control many of the parameters and is multiple steps, here we just have one DADA2 step, which quality trims, generates the error model, dereplicates, removes chimeras, merges reads,  infers ASVs, and generates the count table all in one step. The drawback is that here we have much less control over the detailed parameters (for example, I couldn't figure out how to set a minimum cutoff read length of 175). Still, let's try to replicate the parameters from Happy Belly as closely as possible:


```python
# Check the documentation on dada2 in qiime2:
!qiime dada2
```

    Usage: [34mqiime dada2[0m [OPTIONS] COMMAND [ARGS]...
    
      Description: This QIIME 2 plugin wraps DADA2 and supports sequence quality
      control for single-end and paired-end reads using the DADA2 R library.
    
      Plugin website: http://benjjneb.github.io/dada2/
    
      Getting user support: Please post to the QIIME 2 forum for help with this
      plugin: https://forum.qiime2.org
    
    [1mOptions[0m:
      [34m--version[0m    Show the version and exit.
      [34m--citations[0m  Show citations and exit.
      [34m--help[0m       Show this message and exit.
    
    [1mCommands[0m:
      [34mdenoise-paired[0m  Denoise and dereplicate paired-end sequences
      [34mdenoise-pyro[0m    Denoise and dereplicate single-end pyrosequences
      [34mdenoise-single[0m  Denoise and dereplicate single-end sequences



```python
# We are going to use the denoise-paired option. 

# Next check which input parameters we can modfiy in that option:

!qiime dada2 denoise-paired 
```

    Usage: [34mqiime dada2 denoise-paired[0m [OPTIONS]
    
      This method denoises paired-end sequences, dereplicates them, and filters
      chimeras.
    
    [1mInputs[0m:
      [34m[4m--i-demultiplexed-seqs[0m ARTIFACT [32mSampleData[PairedEndSequencesWithQuality][0m
                             The paired-end demultiplexed sequences to be
                             denoised.                                  [35m[required][0m
    [1mParameters[0m:
      [34m[4m--p-trunc-len-f[0m INTEGER
                             Position at which forward read sequences should be
                             truncated due to decrease in quality. This truncates
                             the 3' end of the of the input sequences, which will
                             be the bases that were sequenced in the last cycles.
                             Reads that are shorter than this value will be
                             discarded. After this parameter is applied there must
                             still be at least a 20 nucleotide overlap between the
                             forward and reverse reads. If 0 is provided, no
                             truncation or length filtering will be performed
                                                                        [35m[required][0m
      [34m[4m--p-trunc-len-r[0m INTEGER
                             Position at which reverse read sequences should be
                             truncated due to decrease in quality. This truncates
                             the 3' end of the of the input sequences, which will
                             be the bases that were sequenced in the last cycles.
                             Reads that are shorter than this value will be
                             discarded. After this parameter is applied there must
                             still be at least a 20 nucleotide overlap between the
                             forward and reverse reads. If 0 is provided, no
                             truncation or length filtering will be performed
                                                                        [35m[required][0m
      [34m--p-trim-left-f[0m INTEGER
                             Position at which forward read sequences should be
                             trimmed due to low quality. This trims the 5' end of
                             the input sequences, which will be the bases that
                             were sequenced in the first cycles.      [35m[default: 0][0m
      [34m--p-trim-left-r[0m INTEGER
                             Position at which reverse read sequences should be
                             trimmed due to low quality. This trims the 5' end of
                             the input sequences, which will be the bases that
                             were sequenced in the first cycles.      [35m[default: 0][0m
      [34m--p-max-ee-f[0m NUMBER    Forward reads with number of expected errors higher
                             than this value will be discarded.     [35m[default: 2.0][0m
      [34m--p-max-ee-r[0m NUMBER    Reverse reads with number of expected errors higher
                             than this value will be discarded.     [35m[default: 2.0][0m
      [34m--p-trunc-q[0m INTEGER    Reads are truncated at the first instance of a
                             quality score less than or equal to this value. If
                             the resulting read is then shorter than `[4mtrunc-len-f[0m`
                             or `[4mtrunc-len-r[0m` (depending on the direction of the
                             read) it is discarded.                   [35m[default: 2][0m
      [34m--p-chimera-method[0m TEXT [32mChoices('consensus', 'pooled', 'none')[0m
                             The method used to remove chimeras. "none": No
                             chimera removal is performed. "pooled": All reads are
                             pooled prior to chimera detection. "consensus":
                             Chimeras are detected in samples individually, and
                             sequences found chimeric in a sufficient fraction of
                             samples are removed.           [35m[default: 'consensus'][0m
      [34m--p-min-fold-parent-over-abundance[0m NUMBER
                             The minimum abundance of potential parents of a
                             sequence being tested as chimeric, expressed as a
                             fold-change versus the abundance of the sequence
                             being tested. Values should be greater than or equal
                             to 1 (i.e. parents should be more abundant than the
                             sequence being tested). This parameter has no effect
                             if [4mchimera-method[0m is "none".           [35m[default: 1.0][0m
      [34m--p-n-threads[0m INTEGER  The number of threads to use for multithreaded
                             processing. If 0 is provided, all available cores
                             will be used.                            [35m[default: 1][0m
      [34m--p-n-reads-learn[0m INTEGER
                             The number of reads to use when training the error
                             model. Smaller numbers will result in a shorter run
                             time but a less reliable error model.
                                                                [35m[default: 1000000][0m
      [34m--p-hashed-feature-ids[0m / [34m--p-no-hashed-feature-ids[0m
                             If true, the feature ids in the resulting table will
                             be presented as hashes of the sequences defining each
                             feature. The hash will always be the same for the
                             same sequence so this allows feature tables to be
                             merged across runs of this method. You should only
                             merge tables if the exact same parameters are used
                             for each run.                         [35m[default: True][0m
    [1mOutputs[0m:
      [34m[4m--o-table[0m ARTIFACT [32mFeatureTable[Frequency][0m
                             The resulting feature table.               [35m[required][0m
      [34m[4m--o-representative-sequences[0m ARTIFACT [32mFeatureData[Sequence][0m
                             The resulting feature sequences. Each feature in the
                             feature table will be represented by exactly one
                             sequence, and these sequences will be the joined
                             paired-end sequences.                      [35m[required][0m
      [34m[4m--o-denoising-stats[0m ARTIFACT [32mSampleData[DADA2Stats][0m
                                                                        [35m[required][0m
    [1mMiscellaneous[0m:
      [34m--output-dir[0m PATH      Output unspecified results to a directory
      [34m--verbose[0m / [34m--quiet[0m    Display verbose output to stdout and/or stderr
                             during execution of this action. Or silence output if
                             execution is successful (silence is golden).
      [34m--citations[0m            Show citations and exit.
      [34m--help[0m                 Show this message and exit.
    
    [33m                  There were some problems with the command:                  [0m
    [31m[1m (1/6) Missing option "--i-demultiplexed-seqs".[0m
    [31m[1m (2/6) Missing option "--p-trunc-len-f".[0m
    [31m[1m (3/6) Missing option "--p-trunc-len-r".[0m
    [31m[1m (4/6) Missing option "--o-table".  ("--output-dir" may also be used)[0m
    [31m[1m (5/6) Missing option "--o-representative-sequences".  ("--output-dir" may
      also be used)[0m
    [31m[1m (6/6) Missing option "--o-denoising-stats".  ("--output-dir" may also be
      used)[0m


Let's choose some parameters:


I am going to get as close as I can to the same parameters used in Happy Belly:

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

- --p-n-threads I am going to put 0 here, which means: use all available threads.

- --p-n-reads-learn DADA2 tries to estimate how many errors were generated by the sequencer based on an error model. This parameters sets how many sequences go into training the model. Leave the default here as 1,000,000 for now


- --verbose will show us what's happening as it's running.


**Note**: This is the longest step and may take a while. Here, on the cyverse computers, it took me about 20 minutes. For a larger dataset, it may take several hours or days and it may be smart to use [tmux](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) incase you lose connection. tmux allows your commands to keep running on the server even if you lose connection (I have not used it here on Cyverse so it's sometihng you would need to inquire with them about if you are having issues).


```python
! qiime dada2 denoise-paired \
--i-demultiplexed-seqs work/demux-paired-end-trimmed.qza \
--p-trunc-len-f 250 \
--p-trunc-len-r 200 \
--p-n-threads 0 \
--output-dir work/DADA2_denoising_output \
--verbose
```

    Running external command line application(s). This may print messages to stdout and/or stderr.
    The command(s) being run are below. These commands cannot be manually re-run as they will depend on temporary files that no longer exist.
    
    Command: run_dada_paired.R /tmp/tmptldxtnrm/forward /tmp/tmptldxtnrm/reverse /tmp/tmptldxtnrm/output.tsv.biom /tmp/tmptldxtnrm/track.tsv /tmp/tmptldxtnrm/filt_f /tmp/tmptldxtnrm/filt_r 250 200 0 0 2.0 2.0 2 consensus 1.0 0 1000000
    
    R version 3.5.1 (2018-07-02) 
    Loading required package: Rcpp
    DADA2: 1.10.0 / Rcpp: 1.0.2 / RcppParallel: 4.4.4 
    1) Filtering ....................
    2) Learning Error Rates
    48037500 total bases in 192150 reads from 20 samples will be used for learning the error rates.
    38430000 total bases in 192150 reads from 20 samples will be used for learning the error rates.
    3) Denoise remaining samples ....................
    4) Remove chimeras (method = consensus)
    6) Write output
    [32mSaved FeatureTable[Frequency] to: DADA2_denoising_output/table.qza[0m
    [32mSaved FeatureData[Sequence] to: DADA2_denoising_output/representative_sequences.qza[0m
    [32mSaved SampleData[DADA2Stats] to: DADA2_denoising_output/denoising_stats.qza[0m


Let's take a look at the results 



```python
! qiime metadata tabulate \
  --m-input-file work/DADA2_denoising_output/denoising_stats.qza \
  --o-visualization work/DADA2_denoising_output/denoising_stats.qzv
```

    [32mSaved Visualization to: work/DADA2_denoising_output/denoising_stats.qzv[0m





<div><img onload="(function(div, url){
if (typeof require !== 'undefined') {
    var baseURL = require.toUrl('').split('/').slice(0, -2).join('/');
} else {
    var baseURL = JSON.parse(
        document.getElementById('jupyter-config-data').innerHTML
    ).baseUrl.slice(0, -1);
}
url = baseURL + url;
fetch(url).then(function(res) {
    if (res.status === 404) {
        div.innerHTML = 'Install QIIME 2 Jupyter extension with:<br />' +
                        '<code>jupyter serverextension enable --py qiime2' +
                        ' --sys-prefix</code><br />then restart your server.' +
                        '<br /><br />(Interactive output not available on ' +
                        'static notebook viewer services like nbviewer.)';
    } else if (res.status === 409) {
        div.innerHTML = 'Visualization no longer in scope. Re-run this cell' +
                        ' to see the visualization.';
    } else if (res.ok) {
        url = res.url;
        div.innerHTML = '<iframe src=\'' + url + '\' style=\'' +
                        'width: 100%; height: 700px; border: 0;\'>' +
                        '</iframe><hr />Open in a: <a href=\'' + url + '\'' +
                        ' target=\'_blank\'>new window</a>'
    } else {
        div.innerHTML = 'Something has gone wrong. Check notebook server for' +
                        ' errors.';
    }
});
})(this.parentElement, '/qiime2/redirect?location=/tmp/qiime2-archive-54lwsrm9')" src="data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==" /></div>




```python
q2.Visualization.load("DADA2_denoising_output/denoising_stats.qzv")
```




<div><img onload="(function(div, url){
if (typeof require !== 'undefined') {
    var baseURL = require.toUrl('').split('/').slice(0, -2).join('/');
} else {
    var baseURL = JSON.parse(
        document.getElementById('jupyter-config-data').innerHTML
    ).baseUrl.slice(0, -1);
}
url = baseURL + url;
fetch(url).then(function(res) {
    if (res.status === 404) {
        div.innerHTML = 'Install QIIME 2 Jupyter extension with:<br />' +
                        '<code>jupyter serverextension enable --py qiime2' +
                        ' --sys-prefix</code><br />then restart your server.' +
                        '<br /><br />(Interactive output not available on ' +
                        'static notebook viewer services like nbviewer.)';
    } else if (res.status === 409) {
        div.innerHTML = 'Visualization no longer in scope. Re-run this cell' +
                        ' to see the visualization.';
    } else if (res.ok) {
        url = res.url;
        div.innerHTML = '<iframe src=\'' + url + '\' style=\'' +
                        'width: 100%; height: 700px; border: 0;\'>' +
                        '</iframe><hr />Open in a: <a href=\'' + url + '\'' +
                        ' target=\'_blank\'>new window</a>'
    } else {
        div.innerHTML = 'Something has gone wrong. Check notebook server for' +
                        ' errors.';
    }
});
})(this.parentElement, '/qiime2/redirect?location=/tmp/qiime2-archive-bqkkblf6')" src="data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==" /></div>



Compare the above table to the `summary_tab` table from Happy Belly.  

It looks like we had a few more input sequences, which was probably due to slightly different cutadapt parameters (we couldn't set the max length cutoff) but then the filtering steps here removed several more reads than in Happy Belly. This could be due to the method we used (eg. 'consensus' method for chimera removal instead of 'pool') or some other parameters that we were unable to set through the qiime2 input parameters (eg. setting a min length of 175). Still, it seems that what we have here is conservative, so I am going to continue...

Take a look at a summary of the remaining sequence reads (note- in the outpute of the rep_seq visualization you can click directly on a sequence to send it to Blast)


```python
! qiime feature-table tabulate-seqs \
--i-data work/DADA2_denoising_output/representative_sequences.qza \
--o-visualization work/DADA2_denoising_output/rep_seqs.qzv \
```

    [32mSaved Visualization to: work/DADA2_denoising_output/rep_seqs.qzv[0m





<div><img onload="(function(div, url){
if (typeof require !== 'undefined') {
    var baseURL = require.toUrl('').split('/').slice(0, -2).join('/');
} else {
    var baseURL = JSON.parse(
        document.getElementById('jupyter-config-data').innerHTML
    ).baseUrl.slice(0, -1);
}
url = baseURL + url;
fetch(url).then(function(res) {
    if (res.status === 404) {
        div.innerHTML = 'Install QIIME 2 Jupyter extension with:<br />' +
                        '<code>jupyter serverextension enable --py qiime2' +
                        ' --sys-prefix</code><br />then restart your server.' +
                        '<br /><br />(Interactive output not available on ' +
                        'static notebook viewer services like nbviewer.)';
    } else if (res.status === 409) {
        div.innerHTML = 'Visualization no longer in scope. Re-run this cell' +
                        ' to see the visualization.';
    } else if (res.ok) {
        url = res.url;
        div.innerHTML = '<iframe src=\'' + url + '\' style=\'' +
                        'width: 100%; height: 700px; border: 0;\'>' +
                        '</iframe><hr />Open in a: <a href=\'' + url + '\'' +
                        ' target=\'_blank\'>new window</a>'
    } else {
        div.innerHTML = 'Something has gone wrong. Check notebook server for' +
                        ' errors.';
    }
});
})(this.parentElement, '/qiime2/redirect?location=/tmp/qiime2-archive-yznhlz0j')" src="data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==" /></div>




```python
q2.Visualization.load("DADA2_denoising_output/rep_seqs.qzv")
```




<div><img onload="(function(div, url){
if (typeof require !== 'undefined') {
    var baseURL = require.toUrl('').split('/').slice(0, -2).join('/');
} else {
    var baseURL = JSON.parse(
        document.getElementById('jupyter-config-data').innerHTML
    ).baseUrl.slice(0, -1);
}
url = baseURL + url;
fetch(url).then(function(res) {
    if (res.status === 404) {
        div.innerHTML = 'Install QIIME 2 Jupyter extension with:<br />' +
                        '<code>jupyter serverextension enable --py qiime2' +
                        ' --sys-prefix</code><br />then restart your server.' +
                        '<br /><br />(Interactive output not available on ' +
                        'static notebook viewer services like nbviewer.)';
    } else if (res.status === 409) {
        div.innerHTML = 'Visualization no longer in scope. Re-run this cell' +
                        ' to see the visualization.';
    } else if (res.ok) {
        url = res.url;
        div.innerHTML = '<iframe src=\'' + url + '\' style=\'' +
                        'width: 100%; height: 700px; border: 0;\'>' +
                        '</iframe><hr />Open in a: <a href=\'' + url + '\'' +
                        ' target=\'_blank\'>new window</a>'
    } else {
        div.innerHTML = 'Something has gone wrong. Check notebook server for' +
                        ' errors.';
    }
});
})(this.parentElement, '/qiime2/redirect?location=/tmp/qiime2-archive-lxcl260q')" src="data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==" /></div>



And take a look at the feature table


```python
! qiime feature-table summarize \
--i-table work/DADA2_denoising_output/table.qza \
--o-visualization work/DADA2_denoising_output/table.qzv \
```

    [32mSaved Visualization to: work/DADA2_denoising_output/table.qzv[0m





<div><img onload="(function(div, url){
if (typeof require !== 'undefined') {
    var baseURL = require.toUrl('').split('/').slice(0, -2).join('/');
} else {
    var baseURL = JSON.parse(
        document.getElementById('jupyter-config-data').innerHTML
    ).baseUrl.slice(0, -1);
}
url = baseURL + url;
fetch(url).then(function(res) {
    if (res.status === 404) {
        div.innerHTML = 'Install QIIME 2 Jupyter extension with:<br />' +
                        '<code>jupyter serverextension enable --py qiime2' +
                        ' --sys-prefix</code><br />then restart your server.' +
                        '<br /><br />(Interactive output not available on ' +
                        'static notebook viewer services like nbviewer.)';
    } else if (res.status === 409) {
        div.innerHTML = 'Visualization no longer in scope. Re-run this cell' +
                        ' to see the visualization.';
    } else if (res.ok) {
        url = res.url;
        div.innerHTML = '<iframe src=\'' + url + '\' style=\'' +
                        'width: 100%; height: 700px; border: 0;\'>' +
                        '</iframe><hr />Open in a: <a href=\'' + url + '\'' +
                        ' target=\'_blank\'>new window</a>'
    } else {
        div.innerHTML = 'Something has gone wrong. Check notebook server for' +
                        ' errors.';
    }
});
})(this.parentElement, '/qiime2/redirect?location=/tmp/qiime2-archive-q6jbvg9i')" src="data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==" /></div>




```python
q2.Visualization.load("DADA2_denoising_output/table.qzv")
```




<div><img onload="(function(div, url){
if (typeof require !== 'undefined') {
    var baseURL = require.toUrl('').split('/').slice(0, -2).join('/');
} else {
    var baseURL = JSON.parse(
        document.getElementById('jupyter-config-data').innerHTML
    ).baseUrl.slice(0, -1);
}
url = baseURL + url;
fetch(url).then(function(res) {
    if (res.status === 404) {
        div.innerHTML = 'Install QIIME 2 Jupyter extension with:<br />' +
                        '<code>jupyter serverextension enable --py qiime2' +
                        ' --sys-prefix</code><br />then restart your server.' +
                        '<br /><br />(Interactive output not available on ' +
                        'static notebook viewer services like nbviewer.)';
    } else if (res.status === 409) {
        div.innerHTML = 'Visualization no longer in scope. Re-run this cell' +
                        ' to see the visualization.';
    } else if (res.ok) {
        url = res.url;
        div.innerHTML = '<iframe src=\'' + url + '\' style=\'' +
                        'width: 100%; height: 700px; border: 0;\'>' +
                        '</iframe><hr />Open in a: <a href=\'' + url + '\'' +
                        ' target=\'_blank\'>new window</a>'
    } else {
        div.innerHTML = 'Something has gone wrong. Check notebook server for' +
                        ' errors.';
    }
});
})(this.parentElement, '/qiime2/redirect?location=/tmp/qiime2-archive-9wg3vd_2')" src="data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==" /></div>



### Assign taxonomy 

We are going to assign taxonomy based on a reference database. These reference databases are called "classifiers" and drawn from curated databases, such as the one from [SILVA](https://www.arb-silva.de/).

You can dowload the classifier directly or train it yourself. Since the dataset we are working with uses common primers, there are classifiers that already exist.  If you are using other primers, or want to train your own reference database for some other reason, you should follow the instructions on this [page](https://docs.qiime2.org/2020.2/tutorials/feature-classifier/).

Here we are going to use  SILVA v132 as our reference database. This is the most recent qiime2-compatible classifier and is the same one used in Happy Belly. I am just going to download the 16S rRNA v4 (515F/806R) v132 classifier from [here](https://docs.qiime2.org/2020.2/data-resources/).


```python
! wget https://data.qiime2.org/2020.2/common/silva-132-99-515-806-nb-classifier.qza
! mv silva-132-99-515-806-nb-classifier.qza work/
```

    --2020-05-05 17:42:01--  https://data.qiime2.org/2020.2/common/silva-132-99-515-806-nb-classifier.qza
    Resolving data.qiime2.org... 52.35.38.247
    Connecting to data.qiime2.org|52.35.38.247|:443... connected.
    HTTP request sent, awaiting response... 302 FOUND
    Location: https://s3-us-west-2.amazonaws.com/qiime2-data/2020.2/common/silva-132-99-515-806-nb-classifier.qza [following]
    --2020-05-05 17:42:01--  https://s3-us-west-2.amazonaws.com/qiime2-data/2020.2/common/silva-132-99-515-806-nb-classifier.qza
    Resolving s3-us-west-2.amazonaws.com... 52.218.237.56
    Connecting to s3-us-west-2.amazonaws.com|52.218.237.56|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 196091391 (187M) [application/x-www-form-urlencoded]
    Saving to: ‘silva-132-99-515-806-nb-classifier.qza’
    
    silva-132-99-515-80 100%[===================>] 187.01M  54.3MB/s    in 3.9s    
    
    2020-05-05 17:42:06 (48.2 MB/s) - ‘silva-132-99-515-806-nb-classifier.qza’ saved [196091391/196091391]
    


Next, assign taxononomy using the SILVA classifier (this step also takes awhile...)


```python
# Classify the representative sequences
! qiime feature-classifier classify-sklearn \
--i-classifier silva-132-99-515-806-nb-classifier.qza \
--i-reads DADA2_denoising_output/representative_sequences.qza \
--o-classification taxonomy.qza
```

    [31m[1mPlugin error from feature-classifier:
    
      The scikit-learn version (0.22.1) used to generate this artifact does not match the current version of scikit-learn installed (0.21.2). Please retrain your classifier for your current deployment to prevent data-corruption errors.
    
    Debug info has been saved to /tmp/qiime2-q2cli-err-xnr152ml.log[0m


That gave an error! It's because I downloaded the newest classifier but we are working in a slightly older version of Qiime2. If you go back to the [data resources page](https://docs.qiime2.org/2020.2/data-resources/) and switch the version (upper left corner) to 2019.10 (our current working version) and get the link for *that* v132 515F/806R classifier, it should work


```python
! rm work/silva-132-99-515-806-nb-classifier.qza -r
```


```python
! wget https://data.qiime2.org/2019.10/common/silva-132-99-515-806-nb-classifier.qza
! mv silva-132-99-515-806-nb-classifier.qza work/
```

    --2020-05-05 17:42:55--  https://data.qiime2.org/2019.10/common/silva-132-99-515-806-nb-classifier.qza
    Resolving data.qiime2.org... 52.35.38.247
    Connecting to data.qiime2.org|52.35.38.247|:443... connected.
    HTTP request sent, awaiting response... 302 FOUND
    Location: https://s3-us-west-2.amazonaws.com/qiime2-data/2019.10/common/silva-132-99-515-806-nb-classifier.qza [following]
    --2020-05-05 17:42:56--  https://s3-us-west-2.amazonaws.com/qiime2-data/2019.10/common/silva-132-99-515-806-nb-classifier.qza
    Resolving s3-us-west-2.amazonaws.com... 52.218.221.176
    Connecting to s3-us-west-2.amazonaws.com|52.218.221.176|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 188926431 (180M) [binary/octet-stream]
    Saving to: ‘silva-132-99-515-806-nb-classifier.qza’
    
    silva-132-99-515-80 100%[===================>] 180.17M  45.7MB/s    in 4.4s    
    
    2020-05-05 17:43:00 (41.4 MB/s) - ‘silva-132-99-515-806-nb-classifier.qza’ saved [188926431/188926431]
    



```python
! qiime feature-classifier classify-sklearn \
```

    Usage: [34mqiime feature-classifier classify-sklearn[0m [OPTIONS]
    
      Classify reads by taxon using a fitted classifier.
    
    [1mInputs[0m:
      [34m[4m--i-reads[0m ARTIFACT [32mFeatureData[Sequence][0m
                             The feature data to be classified.         [35m[required][0m
      [34m[4m--i-classifier[0m ARTIFACT
        [32mTaxonomicClassifier[0m  The taxonomic classifier for classifying the reads.
                                                                        [35m[required][0m
    [1mParameters[0m:
      [34m--p-reads-per-batch[0m INTEGER
        [32mRange(0, None)[0m       Number of reads to process in each batch. If "auto",
                             this parameter is autoscaled to min( number of query
                             sequences / [4mn-jobs[0m, 20000).              [35m[default: 0][0m
      [34m--p-n-jobs[0m INTEGER     The maximum number of concurrently worker processes.
                             If -1 all CPUs are used. If 1 is given, no parallel
                             computing code is used at all, which is useful for
                             debugging. For [4mn-jobs[0m below -1, (n_cpus + 1 + [4mn-jobs[0m)
                             are used. Thus for [4mn-jobs[0m = -2, all CPUs but one are
                             used.                                    [35m[default: 1][0m
      [34m--p-pre-dispatch[0m TEXT  "all" or expression, as in "3*n_jobs". The number of
                             batches (of tasks) to be pre-dispatched.
                                                             [35m[default: '2*n_jobs'][0m
      [34m--p-confidence[0m VALUE [32mFloat % Range(0, 1, inclusive_end=True) | Str %[0m
        [32mChoices('disable')[0m   Confidence threshold for limiting taxonomic depth.
                             Set to "disable" to disable confidence calculation,
                             or 0 to calculate confidence but not apply it to
                             limit the taxonomic depth of the assignments.
                                                                    [35m[default: 0.7][0m
      [34m--p-read-orientation[0m TEXT [32mChoices('same', 'reverse-complement', 'auto')[0m
                             Direction of reads with respect to reference
                             sequences. same will cause reads to be classified
                             unchanged; reverse-complement will cause reads to be
                             reversed and complemented prior to classification.
                             "auto" will autodetect orientation based on the
                             confidence estimates for the first 100 reads.
                                                                 [35m[default: 'auto'][0m
    [1mOutputs[0m:
      [34m[4m--o-classification[0m ARTIFACT [32mFeatureData[Taxonomy][0m
                                                                        [35m[required][0m
    [1mMiscellaneous[0m:
      [34m--output-dir[0m PATH      Output unspecified results to a directory
      [34m--verbose[0m / [34m--quiet[0m    Display verbose output to stdout and/or stderr
                             during execution of this action. Or silence output if
                             execution is successful (silence is golden).
      [34m--citations[0m            Show citations and exit.
      [34m--help[0m                 Show this message and exit.
    
    [33m                  There were some problems with the command:                  [0m
    [31m[1m (1/4) Missing option "--i-reads".[0m
    [31m[1m (2/4) Missing option "--i-classifier".[0m
    [31m[1m (3/4) Missing option "--o-classification".  ("--output-dir" may also be used)[0m
    [31m[1m (4/4) Got unexpected extra argument (\)[0m


The cell below takes a long time to run (>1 hr)...


```python
# And try to assign taxonomy again

# Classify the representative sequences
! qiime feature-classifier classify-sklearn \
--i-classifier work/silva-132-99-515-806-nb-classifier.qza \
--i-reads work/DADA2_denoising_output/representative_sequences.qza \
--output-dir work/classified_sequences \
--verbose
```

    [32mSaved FeatureData[Taxonomy] to: work/classified_sequences/classification.qza[0m



```python
# and visualize
! qiime metadata tabulate \
  --m-input-file work/classified_sequences/classification.qza \
  --o-visualization work/classified_sequences/taxonomy.qzv
```

    [32mSaved Visualization to: work/classified_sequences/taxonomy.qzv[0m





<div><img onload="(function(div, url){
if (typeof require !== 'undefined') {
    var baseURL = require.toUrl('').split('/').slice(0, -2).join('/');
} else {
    var baseURL = JSON.parse(
        document.getElementById('jupyter-config-data').innerHTML
    ).baseUrl.slice(0, -1);
}
url = baseURL + url;
fetch(url).then(function(res) {
    if (res.status === 404) {
        div.innerHTML = 'Install QIIME 2 Jupyter extension with:<br />' +
                        '<code>jupyter serverextension enable --py qiime2' +
                        ' --sys-prefix</code><br />then restart your server.' +
                        '<br /><br />(Interactive output not available on ' +
                        'static notebook viewer services like nbviewer.)';
    } else if (res.status === 409) {
        div.innerHTML = 'Visualization no longer in scope. Re-run this cell' +
                        ' to see the visualization.';
    } else if (res.ok) {
        url = res.url;
        div.innerHTML = '<iframe src=\'' + url + '\' style=\'' +
                        'width: 100%; height: 700px; border: 0;\'>' +
                        '</iframe><hr />Open in a: <a href=\'' + url + '\'' +
                        ' target=\'_blank\'>new window</a>'
    } else {
        div.innerHTML = 'Something has gone wrong. Check notebook server for' +
                        ' errors.';
    }
});
})(this.parentElement, '/qiime2/redirect?location=/tmp/qiime2-archive-ib7hyys0')" src="data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==" /></div>




```python
q2.Visualization.load("classified_sequences/taxonomy.qzv")
```




<div><img onload="(function(div, url){
if (typeof require !== 'undefined') {
    var baseURL = require.toUrl('').split('/').slice(0, -2).join('/');
} else {
    var baseURL = JSON.parse(
        document.getElementById('jupyter-config-data').innerHTML
    ).baseUrl.slice(0, -1);
}
url = baseURL + url;
fetch(url).then(function(res) {
    if (res.status === 404) {
        div.innerHTML = 'Install QIIME 2 Jupyter extension with:<br />' +
                        '<code>jupyter serverextension enable --py qiime2' +
                        ' --sys-prefix</code><br />then restart your server.' +
                        '<br /><br />(Interactive output not available on ' +
                        'static notebook viewer services like nbviewer.)';
    } else if (res.status === 409) {
        div.innerHTML = 'Visualization no longer in scope. Re-run this cell' +
                        ' to see the visualization.';
    } else if (res.ok) {
        url = res.url;
        div.innerHTML = '<iframe src=\'' + url + '\' style=\'' +
                        'width: 100%; height: 700px; border: 0;\'>' +
                        '</iframe><hr />Open in a: <a href=\'' + url + '\'' +
                        ' target=\'_blank\'>new window</a>'
    } else {
        div.innerHTML = 'Something has gone wrong. Check notebook server for' +
                        ' errors.';
    }
});
})(this.parentElement, '/qiime2/redirect?location=/tmp/qiime2-archive-l5hooocz')" src="data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==" /></div>



### Create phylogenetic tree 

Next, we are going to make a phylogenetic tree file with our representative sequences. This is useful for some downstream analyses such as UniFrac.

In QIIME2, you can make an alignment *de novo* or align to a reference tree. To see further description of each of these, see the QIIME2 tutorial [here](https://docs.qiime2.org/2019.10/tutorials/phylogeny/).  Here, I will use the de novo approach. 


```python
# make new directory for results of alignment
! mkdir phylogeny
```

Next make a multiple sequence alignment with MAFFT ([documentation](https://docs.qiime2.org/2020.2/plugins/available/alignment/mafft/))


```python
! qiime alignment mafft \
  --i-sequences DADA2_denoising_output/representative_sequences.qza \
  --o-alignment phylogeny/aligned-rep-seqs.qza
```

    [32mSaved FeatureData[AlignedSequence] to: phylogeny/aligned-rep-seqs.qza[0m


Next you want to mask the alignment, which reduces noise from ambigously aligned regions (see qiime2 link above for more detail):


```python
! qiime alignment mask \
  --i-alignment phylogeny/aligned-rep-seqs.qza \
  --o-masked-alignment phylogeny/masked-aligned-rep-seqs.qza
```

    [32mSaved FeatureData[AlignedSequence] to: phylogeny/masked-aligned-rep-seqs.qza[0m


Next is constructing the phylogeny. I will use fasttree here, but there are multiple options, all described in the linked QIIME2 page above.


```python
! qiime phylogeny fasttree \
  --i-alignment phylogeny/masked-aligned-rep-seqs.qza \
  --o-tree phylogeny/fasttree-tree.qza
```

    [32mSaved Phylogeny[Unrooted] to: phylogeny/fasttree-tree.qza[0m


Next, you must root the tree in order to be able to use it in UniFrac


```python
! qiime phylogeny midpoint-root \
  --i-tree phylogeny/fasttree-tree.qza \
  --o-rooted-tree phylogeny/fasttree-tree-rooted.qza
```

    [32mSaved Phylogeny[Rooted] to: phylogeny/fasttree-tree-rooted.qza[0m


Note that all of the steps above (mafft alignment> mask> fasttree> midpoint root) could have been run with one command, `align-to-tree-mafft-fasttree`.

### Export

Next, like in Happy Belly, we want to export the major files we generated for downstream analyses: 1) the count table (aka feature table/ ASV table/ OTU table), 2) the fasta file, and 3) the taxonomy file. In addition, I will also export the tree file.



```python
! mkdir export
```


```python
! qiime tools export \
  --input-path DADA2_denoising_output/table.qza \
  --output-path export/table
```

    [32mExported DADA2_denoising_output/table.qza as BIOMV210DirFmt to directory export/table[0m


The above file is in [BIOM format](http://biom-format.org/documentation/format_versions/biom-2.1.html). You may want to put in in tsv format is you are going to be using it in R


```python
! biom convert \
-i export/table/feature-table.biom \
-o export/table/table.tsv --to-tsv
```

2) Next export the fasta files with the representative sequences


```python
! qiime tools export \
  --input-path DADA2_denoising_output/representative_sequences.qza \
  --output-path export/rep-seqs.fasta
```

    [32mExported DADA2_denoising_output/representative_sequences.qza as DNASequencesDirectoryFormat to directory export/rep-seqs.fasta[0m


3) Export the taxonomy file


```python
! qiime tools export \
  --input-path classified_sequences/classification.qza \
  --output-path export/taxonomy
```

    [32mExported classified_sequences/classification.qza as TSVTaxonomyDirectoryFormat to directory export/taxonomy[0m


4) And lastly the tree file


```python
! qiime tools export \
  --input-path phylogeny/fasttree-tree-rooted.qza \
  --output-path export/exported-tree
```

    [32mExported phylogeny/fasttree-tree-rooted.qza as NewickDirectoryFormat to directory export/exported-tree[0m


### Save

And that's it! Our results files are all in the 'export' folder. You can dowload these directly to your local computer by right-clicking on the folder object on the left. You can also go back to the Discovery Environment and select "Complete and Save Outputs," which will save *all* of the files you generated, including this notebook, to your 'Analyses' folder in the data store. You can also add files to your data store using the iCommands. 

I will add my 'export' folder to my Data Store using iCommands as an example:


```python
! iput -r export/ 
```

    Running recursive pre-scan... pre-scan complete... transferring data...


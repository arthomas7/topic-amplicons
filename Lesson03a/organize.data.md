## Get the data and organize

Here we are downloading the dataset from Happy Belly and doing some modifications to the files so they are compatible with Qiime2. It is always important to have a backup of data somewhere that is not on the cloud (this can be on an institutional server or a hard drive). The files used here are small enough that you can likely store them temporarily on your computer. Astrobiomike already subsampled the dataset so that it's about 10% the size of the full dataset.  

I did the following locally on my computer and put them on github. If you don't want to go through this, and instead want to download the files directly, just run:  

```git clone https://github.com/biovcnet/amplicons-lesson-3-repo.git```

Later, when we go into the Discovery Environment, you will also be able to link directly to github and copy the files.

Otherwise, you can follow below to learn how to modify the files on your own to make them compatible with qiime2.

Open a terminal window and download the Happy Belly files with the following commands

```
curl -L -o dada2_amplicon_ex_workflow.tar.gz https://ndownloader.figshare.com/files/15072638
tar -xzvf dada2_amplicon_ex_workflow.tar.gz
rm dada2_amplicon_ex_workflow.tar.gz

```

Next, copy some of the files into a new working directory since we don't need all of the files from the raw directory (like the Rproject etc.)

```
mkdir qiime2_wd

# copy all fastq files using the wildcard
cp dada2_amplicon_ex_workflow/*.fq qiime2_wd/

# also copy the primers file and the the sample info file, just to have
cp dada2_amplicon_ex_workflow/primers.fa qiime2_wd/
cp dada2_amplicon_ex_workflow/sample_info.tsv qiime2_wd/

# and cd into our new folder
cd qiime2_wd
```

Next, we will modify the fastq file names to match the file format that is expected by qiime2. These files have been demulitplexed and forward and reverse reads are in separate files, so they are closest to the Cassava paired-end demultiplexed fastq format described in the qiime2 [documentation](https://docs.qiime2.org/2020.2/tutorials/importing/). However, the file names do not match the expected format. So let's change those. 

According to qiime2 documentation, the file names needs 1) the sample identifier, 2) the barcode sequence or a barcode identifier, 3) the lane number, 4) the direction of the read (i.e. R1 or R2), and the set number (ie. something like L2S357_15_L001_R1_001.fastq.gz and L2S357_15_L001_R2_001.fastq.gz, indicating forward and reverse reads respectively).  

Luckily, all these files names start with sample identifiers and end with direction of read. They also allhave `_sub_` in the middle of the name, which can be substituted, using the -s option of the `rename` command. I will move all the fastq files into a `qiime_import` subfolder, and substitute the `_sub_` in the names with some false info for fillers for barcode identifier, the lane number, and set number:

```
mkdir qiime_import

mv *.fq qiime_import/

cd qiime_import

rename 's/_sub_/_00_L001_/' *
rename 's/.fq/_001.fastq/' *

```

Next, compress the fastq files (the .gz extension is expected by qiime2):

```
gzip *.fastq
```

And move two directories up and compress the whole repo:

```
cd ../..

tar -czvf qiime2_wd.tar.gz qiime2_wd/
```


You can upload the file above to your Data Store in Cyverse (next steps in tutorial) and then decompress it using `tar -xzvf qiime2_wd.tar.gz`.

I also uploaded the tutorial files to github, if you want to download it directly without going through this excercise, they are at github in the [BVCN amplicon lesson 3 repo](https://github.com/biovcnet/amplicons-lesson-3-repo). You can download the whole repo directly in your terminal or in the Cyverse environment.

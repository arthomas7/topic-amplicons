## Upload Data to Cyverse's Data Store

Thi tutorial has also been recorded as a [video](https://youtu.be/zNdho4gwZ9M) tutorial.

With your Cyverse account credentials, follow the documentation [here](https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step1.html#download-and-first-time-configuration-of-cyberduck) to download and use Cyberduck for file transfers to/ from your Cyverse data store. 

Be sure to drag data and drop files into your username folder (ie. `iplant/home/esuter`). 

## Setting up a working environment in Cyverse's Discovery Environment using VICE
For this section, we are navigating to the Discovery Environment and using [VICE](https://learning.cyverse.org/projects/vice/en/latest/), a visual interactive environment. This component of the Discovery Environment allows for *exploratory* data analysis on the cloud (rather than submitting a job and later receiving results). This is really useful when using interactive applications like JupyterLabs or RStudio and is very helpful for bioinformatics-types of analyses. VICE is set up similar to a computer's file system, so it is easy to click around and navigate.

Another useful thing about VICE is that it already has several useful apps developed by Cyverse, including an app for running QIIME2 with JupyterLabs. These apps are like virtual machines that come pre-loaded with the packages you need for your analysis. If you were to run the analysis below in your local computer, you would first need to [install QIIME2](https://docs.qiime2.org/2020.2/install/) and run it from the Terminal. If you want to use JupyterLabs with QIIME2 locally, there's a few more [steps](https://docs.qiime2.org/2017.7/interfaces/artifact-api/).

There is some the documentation from Cyverse [here](https://cyverse-jupyter-qiime2.readthedocs-hosted.com/en/latest/) on setting up the QIIME2 Vice App. Below are the specific steps I took to upload and setup the environment for analyzing the Happy Belly data.


### Steps:
1. Log into Cyverse
2. Add the Discovery Environment to your dashboard (it may be in "Available Services" instead of "My Services") and click `Launch`.
	- This will bring you to the Discovery Environment, which looks like a computer desktop. On the lefthand side there are 3 buttons where you have access to your `Data` (which is the same data you added to your Data Store using Cyberduck), `Apps`, where we will get our JupyterLabs-Qiime2 app from, and `Analyses`, which shows you all of the analyses projects that you have completed or are currently working on.
3. Click `Apps` and start searching `Jupyter`. Several Jupyter apps will pop up. The most recent version of a Qiime2 JupyerLab app (as of this demo) is JupyterLab-Qiime2-2019.10 (which is running Qiime2 v2019.10.
	- The apps do not always have the most up-to-date version of Qiime2. For most analyses, that is probably fine but just be aware that you may be working on a version that is no longer supported by the Qiime developers.
	- The Cyverse developers are consistently making new apps to keep up with new versions of Qiime2 so keep checking.
	- You may want to add the app to your "Favorites"
4. Click on the Jupyterlabs-Qiime2 app and a window will pop up.
	- Name your analysis. I made this one `JupyterLab-Qiime2-2019.10_HappyBellyDemo`
	- Put in comments if you want
	- Select output folder for your analysis files. This can be in the 'analysis` folder that Cyverse automatically sets up in your data store, or in another subfolder specific to the project
	- Under `Input Data`, select the working directory folder that we uploaded using Cyberduck, `qiime2_wd`
	- Under `Resource Requirements`, choose the maximum under each category: 8 CPU, 64GB memory, and 512GB disk space.
	- Hit `Launch Analysis`
5. Once the analysis is running, it will appear in a list if you hit the `Analyses` button. Click on it to go to the analysis.
	- Once it's ready loading, a jupyter labs platform pops up with different notebook options.
6. <u>The first things to do</u> once your Jupyter environment is open is to connect to the data store:
	- Click on the Discovery Environment icon on the left (blue swooshy thing). The parameters for the IRODS Setup Config should be mostly filled out. You just need to enter your cyverse user name and password and hit submit. A directory listing of your files in the Data Store should appear below.
	- Next, we have to initialize. Click on `Terminal` in the launcher to open a terminal window.
		- Type `iinit` 
		- Enter your password again (Note: no text will appear as you type) and press Enter.
7. Now we are ready to start our first notebook.
	
	
## Setting up a QIIME2 JupyterLab in VICE

<u>Some notes for this section</u>:  

- This section assumes you have a basic knowledge of Bash (or Command Line) and the Jupyter environment. If you are not somewhat comfortable with these, check out the [Unix Crash Course]( https://astrobiomike.github.io/unix/unix-intro) from Happy Belly. There are also tutorial videos available through the [BVCN Youtube channel](https://www.youtube.com/channel/UC5qVqcvUPfgPQWOhBaR_Low).
- If you have never used Jupyter before, the [Atmosphere-Qiime2 tutorial](https://github.com/joslynnlee/qiime2-workflow-cyverse/wiki/Module:-Introduction-to-Jupyter-Notebook#welcome-to-the-jupyter-notebook) has some nice explanations of what the cells are and how to navigate around a bit.


### Steps

1. Close the terminal and return to the launcher.
2. You'll notice that the data folder we directed the app to use is there!
3. Click on a Python 3 notebook.
	- Another note: qiime2 is written to be implemented in bash (which is what Terminals use). You can run bash commands in a Python environment by preceeding with a `!`. We are going to do that and use a Python notebook instead of a Bash notebook because the graphics work better. But all the commands we use here should still work in a Bash notebook.
4. Name your notebook and start coding!



**Navigating around:**

- Like I mentioned above, the bash commands work in a Python environment as long as you use `!`. Try some standard ones like `ls` or `pwd`
- We can also interact with the Data Store using <u>iCommands</u>, which are bash commands preceeded with an `i`. Compare output of `! ls` to `! ils`.
	- This is useful for transferring files from your data store to you temporary environment using `iget` (or `!iget` in Python) or from your temporary environment to your Data Store using `iput` (`!iput`). Check the Jupyter lab notebook to see examples.

**Backing up your work:**  

*This is important* The Vice environment is *temporary* and will hold your files only while you are working. It is important to save your files by either 1) downloading them to your local computer or 2) transferring them to your Data Store (which is in the cloud but is a more permanent place). There are multiple ways of doing this:

- You can transfer files to and from your Data Store using iCommands (described above in "Navigating Around").
- You can right click on files and download them directly to your computer.
- From the Discovery Environment, in the `Analyses` window, there is a 3-dotted button on the right side of each of your analyses. When you click that button, there is an option for `Complete and Save Outputs`. This will end your analysis session and send all files from the temporary environment to the 'Analysis' folder in your Data Store (or whichever folder you chose in step 4 above.



**Next steps:** *Let's start using QIIME2!!!*

[QIIME2+DADA2 tutorial](https://github.com/biovcnet/topic-amplicons/blob/master/Lesson03a/analysis.md). 





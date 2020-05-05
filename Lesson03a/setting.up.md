## Upload Data to Cyverse's Data Store

With your Cyverse account credentials, you will be able to log into your data store, which is like a cloud storage space. 

I followed the documentation [here](https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step1.html#download-and-first-time-configuration-of-cyberduck) and used Cyberduck as my client. There are other free options out there, such as Filezilla.

Using Cyberduck, drag the entire `qiime2_wd` folder into your Cyverse username folder (ie. mine is `iplant/home/esuter`. The data transfer takes a few minutes.  

## Setting up a working environment in Cyverse's Discovery Environment using VICE
For this section, we are navigating to the Discovery Environment and using [VICE](https://learning.cyverse.org/projects/vice/en/latest/), a visual interactive environment. This component of the Discovery Environment allows for *exploratory* data analysis on the cloud (rather than submitting a job and later receiving results). This is really useful when using interactive applications like JupyterLabs or RStudio and is very helpful for bioinformatics-types of analyses. VICE is set up similar to a computer's file system, so it is easy to click around and navigate.

Another useful thing about VICE is that it already has several useful apps developed by Cyverse, including an app for running Qiime2 with JupyterLabs. These apps are like virtual machines that come pre-loaded with the packages you need for your analysis. If you were to run the analysis below in your local computer, you would first need to [install qiime2](https://docs.qiime2.org/2020.2/install/) and, if you want to use JupyterLabs with qiime2 locally, that's even more complicated [finagling](https://docs.qiime2.org/2017.7/interfaces/artifact-api/).

To set up my working environment in Discovery Environment, I followed the documentation and video [here](https://cyverse-jupyter-qiime2.readthedocs-hosted.com/en/latest/), but modified to use the the Happy Belly data that I uploaded to the Data Store in the previous steps (instead of using Cyverse's gut-microbiome tutorial).


###Steps:
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
	- Under `Parameters`, select the folder that we uploaded using Cyberduck, `qiime2_wd`
	- Hit `Launch Analysis`
5. Once the analysis is running, it will appear in a list if you hit the `Analyses` button. Click on it to go to the analysis.
	- Once it's ready loading, a jupyter labs platform pops up with different notebook options.
6. <u>The first things to do</u> once your Jupyter environment is open is to connect to the data store:
	- Click on the Discovery Environment icon on the left (blue swooshy thing). The parameters for the IRODS Setup Config should be mostly filled out. You just need to enter your cyverse user name and password and hit submit. A directory listing of your files in the Data Store should appear below.
	- Next, we have to initialize. Click on `Terminal` in the launcher to open a terminal window.
		- Type `iinit` 
		- Enter your password again (Note: no text will appear as you type) and press Enter.
7. Now we are ready to start our first notebook.
	
	
## Setting up a qiime2 JupyterLab in VICE

<u>Some notes for this section</u>:  

- This section assumes you have a basic knowledge of Bash (or Command Line) and the Jupyter environment. If you are not somewhat comfortable with these, check out the [Unix Crash Course]( https://astrobiomike.github.io/unix/unix-intro) from Happy Belly. There are also tutorial videos available through the [BVCN Youtube channel](https://www.youtube.com/channel/UC5qVqcvUPfgPQWOhBaR_Low).
- If you have never used Jupyter before, the [Atmosphere-Qiime2 tutorial](https://github.com/joslynnlee/qiime2-workflow-cyverse/wiki/Module:-Introduction-to-Jupyter-Notebook#welcome-to-the-jupyter-notebook) has some nice explanations of what the cells are and how to navigate around a bit.


###Steps

1. Close the terminal and return to the launcher.
2. You'll notice that the data folder we directed the app to use is there!
3. Click on a Python 3 notebook.
	- Another note: qiime2 is written to be implemented in bash (which is what Terminals use). You can run bash commands in a Python environment by preceeding with a `!`. We are going to do that and use a Python notebook instead of a Bash notebook because the graphics work better. But all the commands we use here should still work in a Bash notebook.
4. Name your notebook and start coding!

- **This is important** The vice environment is *temporary* and will hold your files only while you are working. It is important to save your files by either 1) downloading them to your local computer or 2) transferring them to your Data Store (which is in the cloud but is more permanent). You can transfer them to your Data Store using iCommands (described below).


Navigating around:

- Like I mentioned above, the bash commands work in a Python environment as long as you use `!`. Try some standard ones like `ls` or `pwd`
- We can also interact with the Data Store using <u>iCommands</u>, which are bash commands preceeded with an `i`. Compare output of `! ls` to `! ils`.
	- This is useful for transferring files from your data store to you temporary environment using `iget` (or `!iget` in Python) or from your temporary environment to your Data Store using `iput` (`iput`).

Let's start using qiime2!!!


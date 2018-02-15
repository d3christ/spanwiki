# Group T-test

## Contents
  1. [Within-group Analyses](#within-group-analyses)
  2. [Between-group Analyses](#between-group-analyses)
  3. [Within-group Sample Script](#within-group-sample)
  4. [Between-group Sample Script](#within-group-sample)
  5. [ANOVA](#anova)
  6. [Viewing Your Contrast Data in AFNI and Thresholding Your Data](#viewing)
  
<a name='within-group-analyses'></a>
## Within-group Analyses
__SUMMARY:__ Calculates t-tests on specific model coefficients – that is to say, on your contrasts of interest. 3dttest can be used for within and between group comparisons. You’ll need two separate 3dttest scripts for this purpose.\
__INPUT:__ the z*reg+orig.* output from the reg script 
You’ll also need to pull coefficient tags from the z*reg+orig file. Find the correct coefficient tag by using the 3dinfo command on the z*reg+orig output file. __OUTPUT:__	.BRIK & .HEAD files – one pair for each contrast of interest.\
__RUN FROM:__ the scripts folder\
__RUN ON:__ everyone within your group at the same time\
__NOTE:__ Before you run the script you’ll need to create a folder called “ttests” in the experiment root directory (that is, in the expt’s broadest directory level where subject folders are located).

The 3dttest script used for within group analyses is sufficiently similar to that used for between-group analyses that we will only cover a few lines of code in this section. (To learn more about the 3dttest script, see discussion in the between-group analyses section that follows.)

<a name='between-group-analyses'></a>
## Between-group Analyses
__SUMMARY:__ Calculates t-tests on specific coefficient maps – that is to say, on your contrasts of interest. 3dttest can be used for within and between group comparisons. You’ll need two separate 3dttest scripts for this purpose. 
__INPUT:__ the z*reg+orig.* output from the reg script 
__OUTPUT:__ .BRIK & .HEAD files – one pair for each contrast of interest. 
__RUN FROM:__ the scripts directory 
__RUN ON:__	everyone from both groups 
__NOTE:__ Before you run the script you’ll need to create a folder called “ttests” in the experiment root directory (that is, in the expt’s broadest directory level where subject folders are located). 

The 3dTtest script can be found in the “scripts” folder in most experiment home directories. It calculates t-tests on fit coefficients of specific regressors – that is to say, on your contrasts of interest, represented now by the output of your reg script (the z*reg+orig.* file). The 3dttest script pulls the coefficients from the reg output and compares with zero (within-group) or the other group’s data (between-group). For example, it can effectively look at whether there was greater activation at anticipation for high win (usually tagged with 1 in the lookup tables) trials versus low win trials (usually tagged with -1 in the lookup tables).

The 3dttest script outputs .BRIK & .HEAD files – one pair for each contrast. These are what you view in AFNI. They’ll be in your ttests folder a format that looks something like:
```
$ pvnant10c9s+tlrc.BRIK
$ pvnant10c9s+tlrc.HEAD
```
That is
```
$ contrastname_#subjs+tlrc.*
```
You will have a different 3dttest script for each group (within group statistics), and usually only one for between-group analyses. These scripts will call up all related model coefficients (within group or between group). On occasion for less stable models you’ll have different 3dttest scripts for different between-group contrasts (eg, for endowment and risk).

The script begins by warping each subject’s z\*reg+orig file into Talairach space with adwarp. It does this by taking the set of rules you created when you warped your anatomical data, and applying the exact same warp to all of your functional data. The adwarp function can also resample voxels into a new resolution \[the –dxyz 2.0 flag, for example, resamples the input into 2mm cubed voxels]. You can edit the resample size in the 3dttest script by finding that flag and editing the number that follows.

[Discussion of resampling – ask Brian.]()

3dttest then moves on to ttests on regressors using the command 3dttest.

Here, the 3dttest script is assessing how much of an effect each regressor has on the brain activation of interest. The t-score is derived by dividing the beta value of each regressor by its standard error term. We can then assess if these t-scores are significant or not. In other words, we can now know whether our regressors are significant predictors of the brain activation.

After completing the ttests, the 3dttest script applies the 3dmerge command to convert your t-stats to z-scores, thus creating a more intuitive stat to view in AFNI.

Before you run the 3dttest script, go to the parent experiment folder and create a ttests subfolder. The script will look for this folder and put its output files there. AFNI, in turn, looks for the folder when you select the overlay. If your files are present, you’ll be able to select your contrasts of interest!
```
span@dmthal riskfmri]$ mkdir ttests
```
Now edit your 3dTtest script appropriately.
```
span@dmthal riskfmri]$ cd scripts
span@dmthal riskfmri]$ emacs 3dttest
```
You’ll be able to pull the appropriate sub-brick # of the listing for the coefficient of your contrasts from zriskbasicreg+orig or whatever your reg script output file in the format z\*reg+orig is named. You can do this by using the 3dinfo command on the file (in this case a zfile output from the risk task):
```
span@dmthal riskfmri]$ 3dinfo zriskbasicreg+orig.*
```

<a name='within-group-sample'></a>
## Within-group Sample Script
Here is a commented within-group 3dttest script to peruse. To see an example of a between-groups 3dttest script, go to dmthal:/data3/bipomid/scripts/3dttestantoutreg#b.

_Note: Unfortunately, these code samples are images and cannot be easily copy-pasted. Even more unfortunately, the directory for the script no longer seems to exist. For copyable code, please see the between-group code sample below._

![3dttest1.jpg]()\
![3dttest2.jpg]()

Here the bracketed number [10] after each subject is the coefficient tag we’ve pulled for that particular contrast from the z\*reg+orig output of the reg script. Each contrast will have only one coefficient tag.

The –prefix flag syntax basically means that whatever entry (argument) follows it is the name of the output of that contrast’s t-test. In this case, our contrast is reward versus neutral anticipation, so our output name is trvnant3 (t for t-test, 3 for number of subjects).

![3dttest3.jpg]()

-base1 is what you’re comparing all the coefficients to (here 0) -set2 is your data -session flags where you want to put your output

![3dttest4.jpg]()\
![3dttest5.jpg]()

Here we’ve commented out (#) some contrasts we’re probably not going to use in this experiment (ant and out).

![3dttest6.jpg]()

Note: be sure to edit the coefficients appropriately for each new experiment. Note that 3dttest is also run FROM the scripts directory (in the program there are lines that instruct the script to look for the subject directories).
```
span@dmthal scripts]$ ./3dttest
```
Remember to run your 3dttest scripts each time you add a new subject or a number of subjects to the group. You’ll need to ratchet up the number in the contrast file names, as well as add the subject to the list of subjects whose directories are searched by the scripts. It’s worth keeping those old files around, though: you may want to see how your data changes as you add more subjects. Keep in mind that you won’t need to re-run your reg script on everybody; only on the subject who’s been added.

<a name='between-group-sanmple'></a>
## Between-group Sample Script
_This is a sample script taken from dmthal.stanford.edu:/data3/bipomusic/scripts/3dttestantoutreg1b3c_

_This script is uncommented (except by the original author) and is just for additional reference._

\<source lang="bash">

  1. ! /bin/csh
  2. 
  3. The bin command above must be at the beginning of each script, as it tells the computer
  4. where to look for instructions on how to read and run this particular kind of file (a csh script).
  5.
  6. NOTE: Script to perform voxelwise t-tests on two sets of subjects to explore group differences in
  7. activation (between groups)
  8. There's no real need to run ant, out, or antmvn in here.
  9. Grab your [] coefficients using 3dinfo on the z-output of the reg script.
  10. Also note that initials of subjs commented out are just placeholders from the script used
  11. as a template. They're not bipo subjects; replace them as you get new, real subjs.
  12.
  13. Also note: make a ttests directory before running a 3dttests script. The script will look
  14. to put its output there.
  15.
  16. INDIVIDUALIZE: add regressors as needed
  17. subject initials; if you've already run a subj, no need to run again
  18. 
  19. FOR MORE INFO
  20. ON A COMMAND: Type the command name (e.g., grecons) at the command line.
  21. 
  22. LAST EDITED: JP from an EW script | 05/30/07
  23. 
  24. BETWEEN-GROUP 3DTTEST (BPD vs CTR)

Set regfile (the name of the file that contains the coefficients you'll be inputting into your program)
to be equal to your reg script output, here called "zmusicvscramreg".
Also set your voxel resample size. If you don't want to resample set it to the native resolution (for us, 3.75).
Be careful about resampling -- talk to Brian first, as it has consequences.
set regfile=zmusicvscramreg set resampdim=3.75
For each subject: change into the subject's directory, look for a zmusicvscramreg file that's already been tlrc
warped. If it exists, delete, since we're about to make it again. [Echo tells the program to print to our
screen the initials of the subject being processed so that we can troubleshoot if the script chokes.]
foreach subject ( ed mk jv th tk )# ad ec

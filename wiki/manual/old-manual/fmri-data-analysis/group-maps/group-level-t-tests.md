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
```
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

  1. Set regfile (the name of the file that contains the coefficients you'll be inputting into your program)
  2. to be equal to your reg script output, here called "zmusicvscramreg".
  3. Also set your voxel resample size. If you don't want to resample set it to the native resolution (for us, 3.75).
  4. Be careful about resampling -- talk to Brian first, as it has consequences.

set regfile=zmusicvscramreg set resampdim=3.75

  1. For each subject: change into the subject's directory, look for a zmusicvscramreg file that's already been tlrc
  2. warped. If it exists, delete, since we're about to make it again. [Echo tells the program to print to our
  3. screen the initials of the subject being processed so that we can troubleshoot if the script chokes.]

foreach subject ( ed mk jv th tk )# ad ec

  cd ../${subject}*
  echo processing ${subject}
  if ( -e ${regfile}+tlrc.BRIK ) then
    rm -rf ${regfile}+tlrc.*
  endif
 
  1. The adwarp command takes each individual's zmusicvscramreg+orig file and warps it into Talairach space using the
  2. sag116+tlrc file 'roadmap'. Resample if instructed, then output the result as zmusicvscramreg+tlrc.
  3. CD into ttests directory.
  
  adwarp -apar sag116+tlrc -dpar ${regfile}+orig -dxyz ${resampdim} -prefix ${regfile}
  cd ../ttests
 
end 

  1. Remove contrast files if they already exist since we're about to create them.
  2. !!!THIS SEEMS REDUDANT!!!

foreach subject ( ds ed mk th ) # ec ad
rm -rf tmusvscram1b3c+tlrc*

  2. Now we're going to do the same process for each regressor:#
  3. check for & delete old files, run 3dttest between-groups, #
  4. create new t-test output files. #

  1. 3dttest musvscram (reward vs neutral anticipation) group differences. Set 1 are the CTR subjects, set 2 are the BPD subjects.
  2. The "t" at the beginning of the file names refers to the fact that the output is a t-stat.
  3. So "tmusvscram1b3c" =
  4. t for t-stat
  5. musvscram for reward vs neutral anticipation
  6. 1b for 1 BPD subjects
  7. 3c for 3 CTR subjects
  
if ( -e tmusvscram1b3c+tlrc.BRIK ) then

  rm -rf tmusvscram1b3c+tlrc.*
 
endif
3dttest -session . -prefix tmusvscram1b3c \ -set1 \ ../mk050807/${regfile}+tlrc'[20]' \ ../ed051607/${regfile}+tlrc'[20]' \ ../ds051107/${regfile}+tlrc'[20]' \
  1. ../ad /${regfile}+tlrc'[20]' \
-set2 \ ../th052307/${regfile}+tlrc'[20]' \
  1. ../ec /${regfile}+tlrc'[20]' \
  2. ../ /${regfile}+tlrc'[20]' \
  1. Remove z_contrastname+tlrc* if it already exists because we're about
  2. to make new z files when we converts the t-stat maps to z-score maps
  3. using 3dmerge.
rm -f zmusvscram1b3c+tlrc*
  1. Convert t-stat maps to z-score maps with 3dmerge. Name output z*
3dmerge -doall -1zscore -prefix zmusvscram1b3c tmusvscram1b3c+tlrc
  1. Remove old t-stat maps.
rm -f tmusvscram1b3c+tlrc*
  1. Erase tlrc-warped files
foreach subject ( ed mk jv th tk )# ad ec

  cd ../${subject}*
  echo Deleting tlrc-warped ${regfile} files for ${subject}
  rm -rf ${regfile}+tlrc.*
  cd ../ttests
  
end </source>
```

<a name='anova'></a>
## ANOVA

An ANOVA tests for significant differences between means from two or more groups.

<a name='viewing'></a>
## Viewing Your Contrast Data in AFNI and Thresholding Your Data

_(NOTE: THIS IS APPLICABLE TO BOTH IN- AND BETWEEN-SUBJECT ANALYSES.)_

To view your group contrasts (aka group maps) in AFNI, you’ll first need to select an individual’s brain to use as the underlay. Here we’ve chosen subject kp and we copy his axi and sag anatomical files from his folder into the ttests folder for the risk experiment:
```
span@dmthal riskfmri]$ cp kp012207/sag* ttests/.
```
Now change to the ttests directory (where you’ve placed your underlay subject’s files) in your experiment home directory and open afni:
```
span@dmthal riskfmri]$ cd ttests
span@dmthal ttests]$ afni
```
In AFNI, go to Define Overlay, and set Olay to #1 and set Ulay to #1. Set ** to 1, and # to 9. Then deselect autoRange, and enter 10.

Don’t think about why you have to set Olay and Ulay to the same thing in the Define Overlay window. There is a reason, but it is hard to explain. Briefly, using it you can threshold one dataset by another dataset.

Note that the color range bar and the slider bar that you use to select your threshold for significance are NOT coupled/related in any way, though you can go in yourself and set certain colors on the color bar to correspond to certain stat ranges.

To do so, on the color range bar note that each marking corresponds to a z-score. Right now these z scores will be rather arbitrary, but if you’ve deselected autorange and set your range to 10 (so that the brightest color equals a z of 10 and the coolest a z of -10) you will now be able to relate these bars to real z-scores (and therefore p values) of interest. As you move the slider to p values you care about (.01, .001, etc), note the z scores changing to the right. Now move your color markings so that each corresponds to a z score (for example, for a z score of 3.3 you move the marking to .33, which is simply the z-score divided by the range).

Note that ULay and Olay at the bottom RH corner of the GUI (Graphical User Interface) tell you what the z-score is at the crosshair point.

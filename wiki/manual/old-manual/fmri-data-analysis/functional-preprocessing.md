# Functional Preprocessing

## Contents
  1. [Quick Reference](#quick-reference)
  2. [Preprocess Script Detailed Explanation](#script-explanation)
  3. [Code Sample](#code-sample)
  
<a name='quick-reference'></a>
## Quick Reference
Here is a basic preprocessing script to work with: preprocess

In order to tailor the script to your dataset, you need to change three things:
  1. The names of your subject folders
  2. The name of your functional dataset: in the script above, the name of the dataset is "facegive", but let's say that you're aiming to preprocess a classic "mid" dataset. you would simply replace each instance of "facegive" with "mid"
  3. The lead-in / lead-out of your dataset: run 3dinfo on one of your raw functional files (e.g., "mid.nii.gz") and calculate the lead-in / lead-out of your dataset (for more detailed instructions on calculating the lead-in / lead-out, see below)

Make sure your script is executable by running: "chmod +x preprocess", and then run it from your scripts directory by entering the following command:
```
$ ./preprocess
```
This script will take your raw anatomical (i.e., "anat.nii.gz") and functional data (e.g., "mid.nii.gz"), run it through the preprocessing pipeline (described below), and output:
  1. A warped anatomical ( anat+tlrc )
  2. A preprocessed functional ( facegive_dmbnf )
  3. A motion file ( 3dmotion.1D[1..6] )
After preprocessing, you will be in good shape to start the Quality Assurance process.

<a name='script-explanation'></a>
## Preprocess Script Detailed Explanation
The preprocess script is responsible for whipping your data into usable shape. Take a look at the one attached above (or included in "Code Sample" below), and I'll take you through comment by comment to show you what each section does...
  - __Copying anatomical (3dcopy):__ Takes the anatomical from nifti space (anat.nii.gz) into AFNI space (anat+orig)
  - __Warping anatomical (@auto_tlrc):__ Warps the anatomical from original space (anat+orig) into Talairach space (anat+tlrc). The reference brain we use is called TT_N27+tlrc, and it can be located on all of our lab machines in the directory /Users/span/abin/TT_N27+tlrc
  - __Cut off lead-in / lead-out of the functional (3dTcat):__ 3dTcat simply cuts off the first few volumes (the lead-in) and the last few volumes (the lead-out) of your dataset. if you look over at the input of "3dTcat" you'll see two numbers -- in the case of the sample script, you'll see 6 (the lead-in) and 644 (the lead-out). This function tells AFNI to only use the TRs between the lead-in and lead-out (inclusive). In order to find the lead-in / lead-out of your dataset, go to your functional -- in the case of the sample script, facegive.nii.gz -- and run the following command:
```
$ 3dinfo facegive.nii.gz
```
Read the output to find "Number of time steps". In the case of the "facegive" functional, the number is 649, meaning that there are 649 TRs in your trial. It's important to remember, however, that AFNI is 0-indexed, meaning that TR = 0 counts. In other words, if you were using all of the TRs for the "facegive" functional, your input for "3dTcat" would contain the numbers 0 and 648. In our lab, we exclude the first 6 TRs and the last 4 TRs. Therefore, you arrive at the correct numbers in the "facegive" task: lead-in = 6 and lead-out = 644 (648 - 4). Follow the same process to find the correct lead-in / lead-out for your dataset.
  - __Correcting the Slice Timing (3dTshift):__ Corrects for the fact that slices are taken at slightly different times within each TR. (It does this by interpolating the response curve for each voxel.) This is critical for event-related designs, where the assumption is that neural activity will occur for short and discrete intervals. For block designs, which generally gather data on timescales of 20 seconds or more, being off by less than 2s won’t make such a significant difference.
  - __Despiking (3dDespike):__ Removes spikes from the input dataset and writes a new dataset where the spikes are replaced to fit a smoothed curve.
  - __Coregistering the volumes(3dvolreg):__ Makes sure each acquired full volume (full brain) is lined up (‘registered’) with every other volume. This is the main method of motion correction. This is the process that creates output files that allow you to check to see how much the subject moved on a graph (3dmotion.1D)
  - __Applying spacial smoothing (3dmerge):__ Applying a slight Gaussian blur to your data. Note that most volumes have something on the order of 100,000 voxels that will be evaluated for significance. Even with a cutoff of < 0.05, you would expect to see around 5,000 spurious ‘activations.’ By blurring your data a bit, you minimize these false alarms, as surrounding voxels in these regions will not show similar ‘activation.’ Regions of real interest will tend to have clusters of significant voxels, so true activations won’t be smoothed out assuming you’ve chosen an intelligent amount of blurring – too much and you could attenuate true activation below the statistical threshold. Keep in mind that applying a Gaussian blur that is too large (e.g., a FWHM of >10mm) can cause bilateral midbrain activation to pool together into the ventricles down the center, obscuring the fact that you had bilateral activation to begin with.
  - __Normalizing your data (3dTstat / 3dcalc):__ creates data with units of percent-signal-change rather than ‘mag’ units, which are difficult for us to interpret. When you normalize your data you also specify a baseline. In imaging your baseline activation is to some degree arbitrary, so choice of baseline is not trivial. In our lab we average the activation of each voxel over the entire task and use that number as a baseline for that voxel. This doesn’t matter for our regressions, but it does matter for our timecourse for the entire task. A voxel in a brain region that is frequently recruited over the course of a task will have a higher baseline than a baseline in the physiological sense on account of the fact that you are averaging a voxel that is frequently ‘on’ during that time period. Another way of saying this is that this type of average is higher over the period of time in which you’re doing the task than it would be if you averaged over 24 hours of just sitting there.
  - __Applying a temporal filter (3dFourier):__ Filters your data in frequency space (to remove variations caused by heart rate, pressing buttons, breathing, et cetera) and removing linear trends (as might be caused by your subject’s head slowly settling into the pillow). There’s a really worthwhile discussion of this starting on page 274 of Huettel’s Functional Magnetic Resonance Imaging textbook.
  
  
<a name='code-sample'></a>
## Code Sample
Here is an example of a full preprocess script (you can download this script [here]()):
```
<source lang="bash">

! /bin/csh
----------------------------------------------------------------------#
Preprocessing Script (hacked by hm 081914)
This script preprocesses the raw anatomical / functional data
----------------------------------------------------------------------#
foreach subject ( ak090414 ) #ad090114 ag082014 ak082014 ay090214 be082714 bk081914 bt082014 cc090114 dz082614 el081914 es082814 ju081914 jm082814 kk082714 ks090314 ln082814 ls090314 lx090114 mb082714 mh081914 nh082714 oc082614 ph090114 so090314 tk082114 uc082614 za082014

       cd ../${subject}*

#------------	# copy anatomical: #

3dcopy -overwrite anat.nii.gz anat

#------------	# warp anatomical: #

@auto_tlrc -warp_orig_vol -suffix NONE -base /Users/span/abin/TT_N27+tlrc. -input anat+orig

#------------	# cut off leadin/leadout and tshift datasets: #

3dTcat -overwrite -prefix epi0 'facegive.nii.gz[6..644]' 3dTshift -overwrite -slice 0 -tpattern altplus -prefix epi0_ts epi0+orig

#------------	# tcat functional datasets together: #

3dTcat -overwrite -prefix facegive epi0_ts+orig

#------------	# cleanup epi datasets: #

rm -rf epi*orig*

#------------	# despiking: #

3dDespike -overwrite -NEW -prefix facegive_d facegive+orig

#------------	# motion correct: #

3dvolreg -Fourier -twopass -overwrite -prefix facegive_dm -base 3 -dfile 3dmotion.1D facegive_d+orig

#------------	# gaussian blur: #

3dmerge -overwrite -1blur_fwhm 4 -doall -prefix facegive_dmb facegive_dm+orig

#------------	# normalize to percent signal change: #

3dTstat -overwrite -prefix average facegive_dmb+orig 3dcalc -overwrite -datum float -a facegive_dmb+orig -b average+orig -expr "((a-b)/b)*100" -prefix facegive_dmbn

#------------	# fourier highpass filter: #

rm -rf facegive_dmbnf+orig* 3dFourier -highpass 0.011 -prefix facegive_dmbnf facegive_dmbn+orig

#------------	# refit functional to anatmomical: #

3drefit -apar anat+orig facegive_dmbnf+orig

end </source>
```

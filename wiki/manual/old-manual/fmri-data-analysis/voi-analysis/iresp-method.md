# IRESP Method

## Contents
  1. [About IRESP](#about)
      1. [How to run IRESP](#how-to)
  2. [Example IRESP Script](#script)
  3. [Dumping out the data with 3dmaskave](#dumping)
  
<a name='about'></a>
## About IRESP

_Note: Fitted timecourses depend on the specification of your model. If your model changes, so could the timecourses. Raw timecourses do not. Therefore, we tend to rely on raw rather than fitted timecourses (BK 082914). If you still want to use fitted timecourses, read on:_

The IRESP method of dumping out trial-type averaged timecourses was developed to deal with random ITIs. The lab’s original timecourse dumping method, using the python script reorder_regress_lag.py, was designed to dump out data from the MID task, which has a fixed – but jittered near 2 sec – trial length (\*note that this also creates problems for the generation of subject mastervectors via txt2master.py, but it can handle – with a few modifications – random 2, 4, and 6 sec ITIs). Recent tasks have very random ITIs (e.g., futself) or random within a range (e.g., deldis, facetrait, endow, simplerisk). Because most tasks now have pseudorandom ITIs, one new method for looking that them involves using the -iresp flag in 3dDeconvolve. For more on comparing the iresp versus the raw averaging methods, please see [this tech report]().

This method takes advantage of AFNI’s built in impulse response (iresp) estimation capacity in 3dDeconvolve. 3dDeconvolve is used to estimate the fit of brain activity to an idealized model, but it also has the capacity to estimate iresps as well, if given a list of even onsets instead of the usual HRF-convolved regressor files.

<a name='how-to'></a>
### How to run IRESP

To use iresp, you'll first need to make sure you have the "starter" files your scripts will need:

#### Onset Vectors

Before you can begin making your IRESP scripts, you need to create the necessary onset vectors that correspond to conditions of interest from your task. These are the conditions that you want to visualize in the timecourses. For example, the following onset vector script for the smokeshop task marks all trials in which a subject purchased an item:

BEGIN_VEC INPUT: "smokerSHOPMATRIX.csv" OUTPUT: "allBought_onsets.1D"

MARK TR = 1 AND Bought = 1 WITH 1

END_VEC

Use the makeVec.py command or the runBeh script to create these onset vectors from .txt vector description files. Make sure you create onset vectors for every subject in the study.

#### Motion Files

Each subject should have .1D motion file/s from the preprocessing scripts.

<a name='script'></a>
## Example IRESP Script

3ddeconvolve will remove the contribution of motion to your data, and create, for each subject, an average response to each specified trial type. "Impulse response" refers to the fact you flagged the _beginning_ of each trial type in the onset vectors. The function will dump the response from that point in time forward as you specify. 3dDeconvolve takes as input the motion .1D files created earlier, as well as your onset vector file (which specifies block length) and your normed, filtered data.
```
3dDeconvolve -float -input ${dataset}n+orig -nfirst 0 -num_stimts 7 \ 	-polort 2 \
		-stim_file 1 "3dmot_${run}.1D[1]" -stim_label 1 "roll"\
		-stim_file 2 "3dmot_${run}.1D[2]" -stim_label 2 "pitch"\
		-stim_file 3 "3dmot_${run}.1D[3]" -stim_label 3 "yaw"\
		-stim_file 4 "3dmot_${run}.1D[4]" -stim_label 4 "dS"\
		-stim_file 5 "3dmot_${run}.1D[5]" -stim_label 5 "dL"\
		-stim_file 6 "3dmot_${run}.1D[6]" -stim_label 6 "dP"\
		-stim_file 7 "${dataset}_Donsets.1D" -stim_label 7 "Dec" \
		-iresp 7 dec_${dataset} -stim_maxlag 7 14
	# remove default bucket dataset and xmatrix
	rm -f Decon.x1D
	rm -f Decon+orig*
 
rm –f Decon+*
rm –f Decon*x1D
```
Input stim_files 1-6 are motion files necessary so that AFNI can regresss out the effects of motion on the iresp estimation. Input stim_file 7 simply tags the onsets for "Dec" or Decrease trials within the block. The iresp flag tells AFNI that this is a stimfile to use for impulse response estimation and not for the normal whole-brain regression. Finally the stim_maxlag flag tells AFNI how long we would like to average activity after the event onset. The 7 value in stim_maxlag indicates the maximum time lag for the input stimulus. The next value indicates the length of TRs for the timecourse file. This should be the length of your trial. The last lines of code remove the Decon files that are created by default by 3dDeconvolve.

The script above demonstrates the usage for a single trial type within a single block of the experiment. A complete IRESP script typically loops through a set of 3dDeconvolve commands for each block of the experiment. The number of 3dDeconvolve commands within a set depends on the number of different trial types. Please see the full [NeurofeedbackIRESP]() script for a better idea.

The output will be an AFNI 3d+time dataset containing the average response to one trial type within one block of the experiment, separately computed for each voxel in the brain. A complete IRESP script will output several datasets depending on the number of trials types and blocks within the experiment. For ease of timecourse dumping, the datasets are combined using 3dTcat, shown in the example below.
```
3dTcat -prefix IncDectcs inc_nfbkOFF1+orig dec_nfbkOFF1+orig \
			 inc_nfbkON2+orig dec_nfbkON2+orig\
			 inc_nfbkON3+orig dec_nfbkON3+orig\
			 inc_nfbkOFF4+orig dec_nfbkOFF4+orig
```
Notes:
  - This script can be run on individuals alone as you add them.
  - The IRESP script usually lives in the scripts folder
  - The IRESP script is doing FULL BRAIN dumps -- there's no ROI timecourse data yet. These are volumes with information about the average response in each and every individual voxel in the brain for that individual during the specified time points.
  
<a name='dumping'></a>
## Dumping out the data with 3dmaskave

The IRESP script factors in the motion vectors along with the exact timing for trial onsets. The ouput is a single dataset that can now be masked and dumped. Before continuing with this step, you must have created a mask within afni and resampled the mask so that it matches your functional dataset. Instructions to do so are found [here](making-voi-masks.md).

3dmaskave takes your mask and the dataset created with the IRESP script and dumps out the data only for the voxels designated by your maskfile. The values for these voxels are then averaged across the VOI.
```
3dmaskave -mask [mask, resampled and warped to +orig] \ 
-quiet -mrange 1 1 [functionaldataset+orig] > [outputname.tc]

e.g.
 
3dmaskave -mask amVOImaskr+orig \
-quiet -mrange 1 1 IncDectcs+orig > IncDectcs_NAcc.tc
```
Notes:
  - Your mask file usually will have multiple VOIs. You must specify which mask you would like to dump from. This is done in the -mrange option. Mrange takes two values (a, b) that tell the function to dump only the voxels that have values between a and b (inclusive). You can specify a single VOI by repeating it's index number.
  - Using the mask option you will specify the +orig, resampled mask. Voxel dimensions and coordinate space must match the file you are dumping from.
  - The output is a '.tc' file which is a column of averaged values and can be viewed with a simple text editor.
  - Always be sure the output name includes the correct VOI name.
  
--Code examples taken from the Neurofeedback NAcc Study.

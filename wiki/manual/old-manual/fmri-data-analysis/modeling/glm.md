# The GLM

## Contents
  1. [Quick reference](#quick-reference)
  2. [Running a GLM regression in AFNI](#running)
      1. [Making Regressors](#making-regressors)
      2. [Convolving your regressors](#convolving)
      3. [Running the GLM](#running-the-glm)
      4. [Converting to z-scores](#converting)
      5. [The Whole Script](#the-whole-script)
      
<a name='quick-reference'></a>
## Quick Reference
Here is an [example regression script]() to download.

Run this script from your scripts directory using the command:
```
$ ./midReg
```
The script will automatically loop through your subjects and run individual regressions on each subjects. You only need to run this script once per subject. So, if you are running the regression on new subjects you can take the old subjects out of the subjects list.

The script will create two files:
  - zmidReg+orig.BRIK
  - zmidReg+orig.HEAD
  
You will find these in your subject's directories after you run the script.

If the script won't run, make sure it is executable and if not change the permissions using:
```
$ chmod +x midReg
```

<a name='running'></a>
## Running a GLM regression in AFNI
This section is specifically about how to run a GLM in afni. For details about the theory behind running a GLM see the [concepts and theory](background.md) section. That section will also give you information about how to set up your regressors and how to avoid the dreaded colinearity problem.

__Running a regression requires the following steps:__

<a name='making-regressors'></a>
### Making Regressors
For detailed information about making regressors see the [regressors](making-regressors.md) section of the manual. Once you have your txt2matrix.py and vector description files set up you can run them from your regression script using these lines:
```
<source lang="bash">
     ../scripts/txt2matrixMID.py
    makeVec.py ../scripts/midVecs.txt
</source>
```

<a name='convolving'></a>
### Convolving your regressors
After you run makeVec.py you will have boxcar regressors that mark the important phases of your task. However, these regressors do not take into account the hemodynamic delay. Therefore you need to run "waver" on all of you regressors in order to model the hymodynamic delay (more details).

Waver example:
```
<source lang="bash">
waver -dt 2.0 -GAM -input "ant.1D" > "antc.1D"
</source>
```
Inputs:
  - dt 2.0
    - the TR length in seconds (Usually 2)
  - GAM
    - means to use a gamma function
  - input "ant.1D"
    - the boxcar regressor
    
Output:
  - "antc.1D"
    - a convolved regressor (remember to add the "c" before .1D, this stands for convolved)
   
<a name='running-the-glm'></a>
### Running the GLM
We use the afni function 3dDeconvolve to run the regression model. Here is an example:
```
<source lang="bash">
3dDeconvolve -input MIDAlln+orig -concat runs.1D -nfirst 0 -num_stimts 12\
-polort 2 \ -stim_file 1 "antc.1D" -stim_label 1 'ant' \ -stim_file 2 "rvn_antc.1D" -stim_label 2 'rvn_ant' \ -stim_file 3 "pvn_antc.1D" -stim_label 3 'pvn_ant' \ -stim_file 4 "outc.1D" -stim_label 4 'out' \ -stim_file 5 "rvn_outc.1D" -stim_label 5 'rvn_out' \ -stim_file 6 "nvp_outc.1D" -stim_label 6 'nvp_out' \ -stim_file 7 '3dmotionAll.1D[1]' -stim_label 7 'roll' \ -stim_file 8 '3dmotionAll.1D[2]' -stim_label 8 'pitch' \ -stim_file 9 '3dmotionAll.1D[3]' -stim_label 9 'yaw' \ -stim_file 10 '3dmotionAll.1D[4]' -stim_label 10 'dS' \ -stim_file 11 '3dmotionAll.1D[5]' -stim_label 11 'dL' \ -stim_file 12 '3dmotionAll.1D[6]' -stim_label 12 'dP' \ -tout -fout -bucket midReg </source>
```
Inputs:
  - input MIDAlln+orig
    - This is the output of the process script that is normalized to % signal change. It is the brain data that is the input to the GLM.
  - concat runs.1D
    - runs.1D is file marking the starts of each of you blocks (example). This is used for modeling nuisance trends in your data separately for each block.
    - If your task only contains one block, you should delete this input from your Reg script. If you don't, AFNI will show a bunch of errors and you won't be able to run 3DDeconvolve.
  - nfirst 0
  - num_stimts 12
    - the number of stimulus files you are about to list. Remember to update this if you change you model.
  - polort 2
    - means to model linear and quadratic nuisance trends. Higher numbers than 2 would model higher order trends.
  - stim_file 1 "antc.1D"
    - Means use antc.1D as the first stimulus file. This will model the anticipation period of the task.
  - stim_label 1 'ant'
    - Means label the first stimulus file "ant" in the output.
  - tout
    - Means output t statistics
  - fout
    - Means output f statistics
  - bucket midReg
    - Means put everything in one afni file called "midReg"

<a name='converting'></a>
### Converting to z-scores
After running the GLM you want to convert the output to zscores to make them easier to interpret. You do this using:
```
<source lang="bash">
    3dmerge -doall -1zscore -prefix zmidReg midReg+orig
</source>
```

<a name='the-whole-script'></a>
### The Whole Script
```
<source lang="bash">
! /bin/csh
last modified by smg 010708
smokemid
set normdata = MIDAlln set motionname = 3dmotionAll.1D set outfile = midReg set subjects = (ab as aj bh bt)
foreach sub (${subjects})
cd ../${sub}*
# make regressors ../scripts/txt2matrixMID.py makeVec.py ../scripts/midVecs.txt
# convolve regressors waver -dt 2.0 -GAM -input "ant.1D" > "antc.1D" waver -dt 2.0 -GAM -input "rvn_ant.1D" > "rvn_antc.1D" waver -dt 2.0 -GAM -input "pvn_ant.1D" > "pvn_antc.1D" waver -dt 2.0 -GAM -input "out.1D" > "outc.1D" waver -dt 2.0 -GAM -input "rvn_out.1D" > "rvn_outc.1D" waver -dt 2.0 -GAM -input "nvp_out.1D" > "nvp_outc.1D"
# regress and convert to z-scores
if ( -e ${outfile}+orig.BRIK ) then rm -rf ${outfile}+* endif
3dDeconvolve -input ${normdata}+orig -concat runs.1D -nfirst 0 -num_stimts 12\ -polort 2 \ -stim_file 1 "antc.1D" -stim_label 1 'ant' \ -stim_file 2 "rvn_antc.1D" -stim_label 2 'rvn_ant' \ -stim_file 3 "pvn_antc.1D" -stim_label 3 'pvn_ant' \ -stim_file 4 "outc.1D" -stim_label 4 'out' \ -stim_file 5 "rvn_outc.1D" -stim_label 5 'rvn_out' \ -stim_file 6 "nvp_outc.1D" -stim_label 6 'nvp_out' \ -stim_file 7 ${motionname}'[1]' -stim_label 7 'roll' \ -stim_file 8 ${motionname}'[2]' -stim_label 8 'pitch' \ -stim_file 9 ${motionname}'[3]' -stim_label 9 'yaw' \ -stim_file 10 ${motionname}'[4]' -stim_label 10 'dS' \ -stim_file 11 ${motionname}'[5]' -stim_label 11 'dL' \ -stim_file 12 ${motionname}'[6]' -stim_label 12 'dP' \ -tout -fout -bucket ${outfile}
if ( -e z${outfile}+orig.BRIK ) then rm -f z${outfile}+* endif
3dmerge -doall -1zscore -prefix z${outfile} ${outfile}+orig
rm ${outfile}+orig.* end
</source>
```
The final outputs of this script will be:
  - zmidReg+orig.BRIK
  - zmidReg+orig.HEAD
  
You will find these in your subject's directories after you run the script.

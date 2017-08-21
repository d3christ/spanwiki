# Introduction

This section covers ROI (region-of-interest) based analyses. Given an experimental paradigm in which we measure BOLD activity, we often wish to study the activity within a region or set of regions that we already know of. 

Our lab is most often focused on the striatum, anterior insula, and mPFC, but we might focus on many different areas depending on the specific analytical goal we have in mind. 

## Benefits of ROI based analyses. 
There are two connected primary benefits of ROI based analyses that we take advantage of. 

### They are *a priori* 
First, ROI analyses are (preferably) specified *a priori*, i.e. we should know which regions we are interested in before we are even collecting data. (In some cases, findings might suggest a *a posteriori*/*post hoc* analysis, but these analyses should be specified as such when writing up analyses.)

Remember that we're doing _science_, after all, and so we should have specific hypotheses about what's happening in the brain during our experiment, and we want to provide confirmatory or disconfirmatory evidence for them. ROI analyses often let us test those hypotheses in the simplest way.

### Power 
Second, given a single ROI and a single hypothesis, we do not have to correct for multiple comparisons when we perform a single statistical test (building a linear model, running a t-test, etc.). This is not the case in whole-brain analyses, in which we often construct a linear model for every single voxel in the brain, and thus must perform some sort of cluster-correction to account for multiple comparisons.

Specifying our hypotheses before collecting data allows us to provide a small number of hypotheses to test while letting us avoid multiple comparisons correction, _provided that our hypotheses are independent_. If we believe that a certain region of the brain might be responsible for a certain behavior, but keep changing our minds about which one (ie, we keep the behavior variable model in our constant while swapping out the ROI), our analyses aren't independent unless we have *a priori* reasons to think that those regions are responsible for separate reasons.

The increased power claim is also true of any ROI analysis post-hoc (we might only look at 10 ROIs, and not 160,000 voxels), but we must be still be careful to correct for multiple comparisons at the ROI-level to avoid p-hacking. [Head15]

[Head15]: Head, M. L., Holman, L., Lanfear, R., Kahn, A. T., & Jennions, M. D. (2015). The extent and consequences of p-hacking in science. PLoS biology, 13(3), e1002106.

# Performing ROI analyses

## Creating ROI masks. 
An ROI mask is an fmri data file (such as a niftii file or AFNI BRIK/HEAD pair) that defines which voxels belong to the area we're interested in. 

Our lab has a number of previously defined masks based on meta-analyses or anatomical atlases, but if for some reason you need to create your own, go ahead and check out the section or [making rois](making_rois.md)

## Creating ROI timecourse files. 

Given a mask and an EPI dataset, we create a timecourse file (a single column vector of length N where N is the number of TRS in the EPI) in two steps. 

First, we warp and resample the TLRC roi mask to a given subjects original space using `3dfractionize` by using the inverse anatomical tlrc warp (this assumes we have already calculated the tlrc warp--see preprocessing.)

Second, we use `3dmaskave` to create a the column file given our EPI. 

A simple bash script for performing the step is given below. Note that most of our lab-standard ROIs include regions coded with 1s and 2s to denotate laterality, so we actually perform 3dmaskave three times to capture both sides seperately and together. 


```csh
#!/bin/csh

# create a new folder to hold all of our timecoures files
mkdir processed_tcs
set regfiles = ( epi_mbnf )
set masks = ( mask_name )
set topdir='/Users/span/projects/sample'
set maskdir = '/Users/span/masks'
set subjects = (sub1 sub2 )

foreach subject ( ${subjects} )
    cd ${topdir}/${subject}

    foreach regfile (${regfiles} )
        
        # first we create a version of the mask in the subject's original space
        foreach maskname ( ${masks} )
            cp ${maskdir}/maskname+tlrc* .
            
            if( -e ${maskname}r+orig.HEAD) then
                rm ${maskname}r+orig.*
            endif
            3dfractionize -overwrite -template ${regfile}+orig -input ${maskname}+tlrc -preserve -warp anat+tlrc -clip 0.1  -prefix ${maskname}r+orig
        end

        # then we create the tc file
        foreach mask ( $masks )

            3dmaskave -overwrite -mask ${mask}r+orig -quiet -mrange 1 1 ${regfile}+orig > processed_tcs/l${regfile}_${mask}.1D
            3dmaskave -overwrite -mask ${mask}r+orig -quiet -mrange 2 2 ${regfile}+orig > processed_tcs/r${regfile}_${mask}.1D
            3dmaskave -overwrite -mask ${mask}r+orig -quiet -mrange 1 2 ${regfile}+orig > processed_tcs/b${regfile}_${mask}.1D

        end
    end
end
```


## Collecting timecourse files into a csv
Below is a simply python script that collects all subjects ROI data into a single csv assuming that we have behavioral data in format... 

TODO- add more commentary, clean up code...

```python
import os
import numpy as np
import pandas as pd

SUBJECTS  = 'sub1, sub2'
TC_FOLDER = 'processed_tcs'
MASKS = ['nacc8mm', 'finger_tapping_ns']
SIDES = ['b','l','r']

# we'll get a column for TR_1 through TR_16 of every trial
TR_DELAY = xrange(16)

for Subject in SUBJECTS:
    for i, functional in enumerate(['epi_mbnf']):
        MASTER = []

        task = functional.split('_')[0]
        OUTFILE = task + '_fbproc.csv'
        for mask in MASKS:
            for side in SIDES:
                censor_name = functional.split('_mbnf')[0] + '_censor.1D'
                censor = pd.Series(np.genfromtxt(censor_name))
                print(censor.shape[0])
                print pd.Series(np.tile(['rosie'], (censor.shape[0])))
                Subject = pd.Series(np.tile([Subject], (censor.shape[0])), name='Subject')
                ROI = pd.Series(np.tile( [mask + side], (censor.shape[0]) ), name='ROI')
                tcdf = pd.concat([Subject, ROI], axis=1)
                tcdf['Censor'] = censor

                tcdf['TrialType'] = pd.Series(np.genfromtxt('20tr_tc.1D'))
                # data_matrix = data_matrix.rename(columns = {data_matrix.columns[2] : 'Timestamp'})
                # tc_file = s + '_' + side + '_' + mask + '_raw.tc'
                tc_file = side + functional + '_' + mask + '.1D'
                tc_file_path = os.path.join(TC_FOLDER, tc_file)
                print tc_file_path

                tc = np.genfromtxt(tc_file_path)
                if TC_FOLDER == 'raw_tcs':
                    tc = (tc-tc.mean())/tc.std(ddof=1) #z-score

                for tr in TR_DELAY:
                    tcdf['TR_' + str(1 + tr)] = pd.Series(tc).shift(-tr)
                # tcdf['Next_Result'] = pd.Series(tcdf['Slope_Result']).shift(-1)

                tcdf = tcdf[tcdf['TrialType'] != 0]

                if isinstance(MASTER, list):
                    MASTER = tcdf
                else:
                    MASTER = MASTER.append(tcdf)


        MASTER.to_csv(OUTFILE)



```










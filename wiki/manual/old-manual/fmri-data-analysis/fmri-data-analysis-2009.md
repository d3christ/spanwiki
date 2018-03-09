# FMRI Data Analysis (2009)

## Contents
  1. [Preprocessing](#preprocessing)
  2. [Modeling](#modeling)
  3. [Group Maps](#group-maps)
  4. [Volume of Interest Analysis](#VOI-analysis)
  5. [AFNI command help](#AFNI)
  6. [General Program Guides](#program-guides)
  7. [Exercise for newbies](#newbies)

<a name='preprocessing'></a>
## Preprocessing
  - [Functional preprocessing](preprocessing/functional-preprocessing.md)
  - [Anatomical preprocessing (nudging and warping by hand)](preprocessing/anatomical-preprocessing.md)
  - [Checking for motion](preprocessing/checking-for-motion.md)
  - [Troubleshooting](preprocessing/troubleshooting.md)

<a name='modeling'></a>
## Modeling
  - [Background](modeling/background.md)
  - [Making regressors](modeling/making-regressors.md)
  - [Runnings a general linear model (GLM)](modeling/glm.md)
  - [Calculating Mean Squared Successive Deviation (MSSD -- an optional measure of temporal variance)](modeling/mssd.md)

<a name='group-maps'></a>
## Group Maps
  - [Group anatomical (optional)](group-maps/group-anatomical.md)
  - [Group level t-tests](group-maps/group-level-t-tests.md)
  - Individual differences
  - [Making activation tables](group-maps/making-activation-tables.md)

<a name='VOI-analysis'></a>
## Volume of Interest Analysis
  - [Making VOI Masks](voi-analysis/making-voi-masks.md)
  - [Warping VOI Masks]()
  - [Plotting Timecourses]()
      - [Raw Timecourses]()
      - [Generating Fitted Timecourses (iresp)]()
      - [Regression Coefficients]()
  - [Logistic regression]()
  - [Mediation Analysis]()

<a name='AFNI'></a>
## AFNI command help
[AFNI Programs and Plugins](): Here is a list of useful AFNI commands and their functions. You can also google commands on the AFNI discussion board, or type in the afni command at the command prompt.

<a name='program-guides'></a>
## General Program Guides
  - [E-Prime]()
  - [MATLAB]()
  - [Stata]()

<a name='newbies'></a>
## Exercise for newbies
If you're new to FMRI data analysis, you could first try the following: 
  1. Find a recent neuroimaging paper (for example Knutson et. al. 2005 or Knutson et. al. 2007 would work well). First give the paper a quick read through to get the main idea of the paper. 
  2. Next, slowly and carefully read through the methods section. 
  3. Finally, try to identify all of the steps required to run the analyzes in the paper. See if you can match the steps you found in the methods section to the steps in the above pipeline. 
This should give you a good sense of the overall flow of data processing from start to finish.

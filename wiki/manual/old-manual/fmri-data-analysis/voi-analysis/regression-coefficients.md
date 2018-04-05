# Dumping Regression Coefficients

## Contents
  1. [Overview](#overview)
  2. [Organizing the Group Coefficient Data](#organizing)
  
<a name='overview'></a>
# Overview
Coefficients are dumped to do a continued analysis of the betas used in the creation of maps. These values are often put up against other variables (ex. demographics, task performance) to analyze predictive power. Before you dump coefficients, you must have a functional dataset with values converted to z-scores (ex. zmilantoutreg+orig) for each participant. Additionally, you need a maskfile that is resampled and warped to match the functional dataset. For instructions on creating this maskfile click [here](making-voi-masks.md).
```
cd ../[participant]
mkdir [coeffdirname]
cd coeff
3dmaskave -mask [mask, resampled and warped to +orig] \ 
-quiet -mrange 1 1 [functionaldataset+orig][coeffNums] > [outputname.coef]

e.g.

cd ../ap*
mkdir mil_coeff
cd coeff
3dmaskave -mask apVOImaskr+orig \
-quiet -mrange 1 1 'zmilantoutrlareg+orig[4,7,13,16]' > ap_mil_lmpfc.coef
```
Notes:
  - In addition to specifying the correct functional file, you must also specify the coefficients that you would like to dump from. These values are taken from within the afni Define Overlay pane, in the dropbox where you would specify which overlay contrast to view.
  - Your mask file usually will have multiple VOIs. You must specify which mask you would like to dump from. This is done in the -mrange option. Mrange takes two values (a, b) that tell the function to dump only the voxels that have values between a and b (inclusive). You can specify a single VOI by repeating it's index number.
  - Using the mask option you will specify the +orig, resampled mask. Voxel dimensions and coordinate space must match the file you are dumping from.
  - The output is a '.coef' file which is a column coefficient values and can be viewed with a simple text editor.
  - Always be sure the output name fits the following format 'subj_studystem_voi.coef'. This will ensure that the files can be easily combined into an organized table.
  - Make sure that you are dumping into a folder in the participant's directory. The name of this folder should be the same within each of the participant's directories.
  
--Code examples taken from the FINRA MIL Study.

<a name='organizing'></a>
## Organizing the Group Coefficient Data
After dumping coefficients, you will end with a file for each voi. For each subject, these files should be in a folder within the subject's main directory. This folder should have the same name for all participants. Each file should have a single column with a row value for each coefficient you listed. Make sure the dump files use the convention: subj_studystem_voi.coef (ex. cw_nfbk_nacc.coef). To continue the analysis, you will need to combine the files for all participants into a single table; makecoeftable.py will do this.

The makecoeftable.py script is for creating an organized table for the dumped coefficient data. For each voi specified in a coeff dump, you will have an output file contained in a folder within each participant's directory. This script will copy those files into a new directory in your scripts folder and create a table with all the coefficient data. The script must be run from your scripts directory. The table will list subject data by row. Columns will be listed and labeled by VOI then by coefficient number. Ordering specfied in the User Control Variables will be preserved in the final .csv file. Below is an example for the User Control Variables. As long as all the conventions are utilized, the script should be optimized to work with any study. To see the entire script click [here](makecoeftable.md).
```
User Control Variables: makecoeftable.py 

participants= ['ap', 'as', 'ba', 'bg', 'bh', 'bp', 'cd', 'ch', 'cn', 'dj', 'er', 'fl', 'jb', 'jj', 'js', 'kc']

coefficientNums=[4, 7, 13, 16] #MUST be in the order listed in the input files (order also specified in the dump script)

vois=['linsu','rinsu', 'rmpfc', 'lmpfc', 'lrcau', 'rrcau', 'lmcau', 'rmcau', 'lnacc', 'rnacc', 'linsu2', 'rinsu2', 'ramyg', 'lamyg']

studystem='mil' #study name

sub_coefdir='mil_coeff' # folder in subject's directory where .coef files are

coeffdir='grpcoeff'  #the directory you want your coeff table and copied coeffs to end up (in your scripts directory)
```
Another method would be to use the coefpaste command that can be found in the biac2b/data1/design/timcourses folder. This command uses the coefPaste script (modified from the tcPaste script) and organizes .coef files across subjects into columns for each mask you specify.

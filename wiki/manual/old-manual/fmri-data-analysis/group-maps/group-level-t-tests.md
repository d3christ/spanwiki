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
SUMMARY: Calculates t-tests on specific model coefficients – that is to say, on your contrasts of interest. 3dttest can be used for within and between group comparisons. You’ll need two separate 3dttest scripts for this purpose. 
INPUT: the z*reg+orig.* output from the reg script 
You’ll also need to pull coefficient tags from the z*reg+orig file. Find the correct coefficient tag by using the 3dinfo command on the z*reg+orig output file. OUTPUT:	.BRIK & .HEAD files – one pair for each contrast of interest. 
RUN FROM: the scripts folder 
RUN ON: everyone within your group at the same time 
NOTE: Before you run the script you’ll need to create a folder called “ttests” in the experiment root directory (that is, in the expt’s broadest directory level where subject folders are located). 
The 3dttest script used for within group analyses is sufficiently similar to that used for between-group analyses that we will only cover a few lines of code in this section. (To learn more about the 3dttest script, see discussion in the between-group analyses section that follows.)

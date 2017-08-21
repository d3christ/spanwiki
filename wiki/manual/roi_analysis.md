# Introduction

This section covers ROI (region-of-interest) based analyses. Given an experimental paradigm in which we measure BOLD activity, we often wish to study the activity within a region or set of regions that we already know of. 

Our lab is most often focused on the striatum, anterior insula, and mPFC, but we might focus on many different areas depending on the specific analytical goal we have in mind. 

# Benefits of ROI based analyses. 
There are two connected primary benefits of ROI based analyses that we take advantage of. 

### *a priori* 
First, ROI analyses are (preferably) specified *a priori*, i.e. we should know which regions we are interested in before we are even collecting data. (In some cases, findings might suggest a *post hoc* analysis, but such analyses should be specified as such when writing up analyses.)

Remember that we're doing _science_, after all, and so we should have specific hypotheses about what's happening in the brain during our experiment, and we want to provide confirmatory or disconfirmatory evidence for. ROI analyses often let us test those hypotheses in the simplest way.

### * power * 
Second, given a single ROI and a single hypothesis, we do not have to correct for multiple comparisons when we perform a single statistical test (building a linear model, running a t-test, etc.). This is not the case in whole-brain analyses, in which we often construct a linear model for every single voxel in the brain, and thus must perform some sort of cluster-correction to account for multiple comparisons.

Specifying our hypotheses before collecting data allows us to provide a small number of hypotheses to test while letting us avoid multiple comparisons correction, _provided that our hypotheses are independent_. If we believe that a certain region of the brain might be responsible for a certain behavior, but keep changing our minds about which one (ie, we keep the behavior variable model in our constant while swapping out the ROI), our analyses aren't independent unless we have *a priori* reasons to think that those regions are responsible for separate reasons.

The increased power claim is also true of any ROI analysis post-hoc (we might only look at 10 ROIs, and not 160,000 voxels), but we must be careful to correct for multiple comparisons at the ROI-level to avoid p-hacking. [^fn1]



[^fn1]: Head, M. L., Holman, L., Lanfear, R., Kahn, A. T., & Jennions, M. D. (2015). The extent and consequences of p-hacking in science. PLoS biology, 13(3), e1002106.

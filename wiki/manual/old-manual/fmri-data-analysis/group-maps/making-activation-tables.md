# Activation Tables

## Contents
  1. [Setting thresholds: Voxelwise](#voxelwise)
  2. [Setting thresholds: Incorporating cluster criteria](#cluster)
  3. [Making activation tables](#activation-tables)
      1. [3dclust](#3dclust)
      2. [tableDump.py](#tabledump)
      3. [Visual inspection](#visual)
      
<a name='voxelwise'</a>      
## Setting thresholds: Voxelwise

In order to make a table, you first must set a threshold. One way to do this is to run a "Bonferroni correction" on every voxel tested. 

Pro: it's simple and impervious to reviewer criticism. 

Con: it's extremely conservative. 

Start with a standard overall threshold (e.g., p<.05), and then divide that by the number of tests (i.e., in this case, the number of voxels). You could reduce the number of voxels by only testing a subset of them (e.g., in a predefined volume of interest or "VOI").

__1. Whole brain example:__ Assuming a voxel size of about 4 mm cubic (e.g., 64 ml), and that extrabrain and white matter voxels have been masked out, you will still end up testing many gray matter voxels. For instance, if there is a gray matter average of 550000 ml per brain (cf. Knutson et al., 2001, Biological Psychiatry), then 550000 ml / 64 ml = 8594 4 mm cubic voxels. A Bonferroni correction on all this yields an extremely punitive threshold of .05 / 8594 = 5 x 10^-6 = .000006 per voxel!

__2. VOI example:__ If we already know which region should be activated, we could still use a "small volume correction" (SVC) to set the Bonferroni correction to set a more liberal threshold within that region. For instance, suppose we focus on six bilateral 8 mm diameter spheres in MPFC, NAcc, and VTA (regions commonly activated in reward tasks), and again assume a standard voxel size of 4 mm cubic. Then we would calculate a total volume of 4/3pi(4)^3 * 6 = 687 ml / 64 ml = 10 (ish) voxels; and .05 / 10 = .005 -- a much more reasonable correction. Thus, we often use .005 as a threshold in restricted volume of interest analyses.

<a name='cluster'></a>
## Setting thresholds: Incorporating cluster criteria

Since nearby activations tend to cluster, we can use this assumption to reduce the stringency of overall thresholds. However, you should *not* specify a cluster limit bigger than the structures you are querying (this may be why many papers fail to detect NAcc activity). Here's how...

AFNI program AlphaSim can be used to determine omnibus thresholds after correcting for cluster criteria (e.g., via monte carlo 
simulation).

Example command based on the slice prescription for the gamefmri project:
```
AlphaSim -nxyz 64 64 24 -dxyz 3.75 3.75 4 -iter 100000 -pthr .001 -fwhm 4 -quiet -fast
```
generates...

++ AlphaSim: AFNI version=AFNI_2008_02_01_1144 (Jul 3 2008) [32-bit] ++ Authored by: B. Douglas Ward ++ default NN connectivity being used

Program: AlphaSim Author: B. Douglas Ward Initial Release: 18 June 1997 Latest Revision: 10 Jan 2008

Data set dimensions: nx = 64 ny = 64 nz = 24 (voxels) dx = 3.75 dy = 3.75 dz = 4.00 (mm)

Gaussian filter widths: sigmax = 1.70 FWHMx = 4.00 sigmay = 1.70 FWHMy = 4.00 sigmaz = 1.70 FWHMz = 4.00

Cluster connection = Nearest Neighbor Threshold probability: pthr = 1.000000e-03

Number of Monte Carlo iterations = 100000
```
 Cl Size   Frequency    CumuProp     p/Voxel   Max Freq       Alpha
     1       8754479    0.942821  0.00100385        482    1.000000
     2        484826    0.995034  0.00011330      62539    0.995180
     3         40988    0.999449  0.00001466      31987    0.369790
     4          4503    0.999934  0.00000215       4377    0.049920
     5           543    0.999992  0.00000032        542    0.006150
     6            60    0.999999  0.00000005         60    0.000730
     7            10    1.000000  0.00000001         10    0.000130
     8             2    1.000000  0.00000000          2    0.000030
     9             0    1.000000  0.00000000          0    0.000010
    10             1    1.000000  0.00000000          1    0.000010
```
(what does this mean and how do you select?)

You should also use a mask file to limit your voxel count to voxels inside the brain, e.g.,
```
AlphaSim -mask group_mask_43a+tlrc. -dxyz 3.75 3.75 4 -iter 1000 -pthr 0.001 -fwhm 4 -quiet -fast
```
You can also set the p-value threshold you want (e.g. to allow separation of clusters/regions) and then use AlphaSim to determine the appropriate cluster size (Adcock 2006; but see the above warning about using clusters that are bigger than your region of interest).

AlphaSim Manual Link: [PDF]()

<a name='activation-tables'></a>
## Making activation tables

After setting a threshold, you can dump out a table by combining: (1) an AFNI program that dumps out information (3dclust) with (2) a python program that extracts and orders the relevant bits (tabledump.py).

<a name='3dclust'></a>
### 3dclust

You can use the "3dclust" command to dump out tables, for example:
```
$ 3dclust -1Dformat -nosum -1dindex 1 -1tindex 1 -2thresh -3.29 3.29 -dxyz=1 1.75 2 zinput+tlrc.HEAD > output.1D
```
Above, 3dclust takes the first sub-brik file, thresholds it at Z=+/-3.29, and kicks out clusters which share no less than 1.75 voxel edges and include at least two voxels.

__Summary:__ 3dclust is an afni function that finds clusters of an experimenter-specified size. 3dclust tests those clusters for statistical strength at a p value specified by the experimenter, then outputs regions meeting both of these criteria to a .txt file. We use these .txt files to create our tables of significant activation for publications.\
__Input:__ 1 sub-brick (aka, contrast of interest) with z-scores, e.g., zgvnant12c, as output by the 3dttest script\
__Output:__	.txt files with a table of clusters of activation that reach your specified significance\
__Run from:__ the ttests directory; other studies may have a shell script called 3dclust already in the ttest directory which you can use for reference.\
__Run on:__	group A versus group B output for between-group, or just group A output for within group

If your analysis requires using cluster criteria, you’ll need to copy the 3dclust (e.g., from another directory) script into your experiment’s ttests directory. Then, open it in gedit or emacs to edit.
```
$ gedit 3dclust &
```

<a name='tabledump'></a>
### tableDump.py

This script can be found on dmthal in usr/local/bin or on the scripts page of the lab manual.

It creates tables in csv format from 3dclust output. To run it from the command line in your ttests directory, type
```
$ tableDump.py 3dclustOut.txt output
```
where 3dclustOut.txt is your input file, and output is the name you want to give the output.

__Optional Input:__ a base filename for your output. The default is to use your cluster file name.

__Output:__ a file called table_[yourfilename].csv which contains a csv file with the following headers and data for each cluster;
```
TTRegion: Closest Talairach Region to the coordinate focus point
TTmm: mm of suggested region to coordinate focus point
CARegion: Closest CA_N27 Region to the coordinate focus point
CAmm: mm of suggested region to coordinate focus point
X: x coordinate already corrected for publication (i.e., reverse of AFNI)
Y: y coordinate already corrected for publication (i.e., reverse of AFNI)
Z: z coordinate already corrected for publication 
zScore: max z Score for the cluster
Voxels: size of the cluster
You will also get a file called wai_[yourfilename].csv which contains 
the output from the afni whereami command."""
```
After you get the output of the tableDump script you can view it in excel. Here is a word template for a typical table (i.e., group comparisons with the MID task).

<a name='visual'></a>
### Visual inspection

_You MUST check over the regions manually._

Although the scripts are handy for generating tables, many things can and will go wrong, such as:

1. Activation foci are outside the brain.

Obviously, you do not want to include these in your table. But the scripts do not know the difference, so you must visually inspect the foci and delete those which do not fall within the skull. We are working on a cranial mask to solve this problem.

2. The first two coordinates have the wrong sign.

AFNI's graphical user interface (and 3dclust) reports Talairach coordinates (x,y,z) in Left-Posterior-Superior (LPS), whereas publishing convention is to report them Right-Anterior-Superior (RAS). For instance, if the AFNI GUI reports x = -2, y = 3, z = 7 you should report these as Talairach coordinates x = 2, y = -3, and z = 7. For publications, this means that the first two coordinates have the wrong sign. 
tableDump.py should solve this problem, but you should also double-check.

3. The z-score has the wrong sign.

For some reason, tableDump.py sometimes assigns the wrong sign to z-scores. We are working on fixing this bug.

4. Regions are misnamed.

AFNI gets names from the "where am i" function, which often gets things wrong. Be particularly careful about the NAcc...AFNI thinks that the NAcc is much much smaller than it is and will almost always say that the NAcc activation is in the Caudate (for guidance on localization see Knutson, Delgado, and Phillips (2008)).

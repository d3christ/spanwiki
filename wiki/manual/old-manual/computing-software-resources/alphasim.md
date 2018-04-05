# AlphaSim

[Does anybody know about this?]() Helps with whole-brain correction.

AlphaSim is used to do a monte carlo simulation to estimate false discovery rates.

The command we use based on the slice prescription for the gamefmri project is:
```
 AlphaSim -nxyz 64 64 24 -dxyz 3.75 3.75 4 -iter 100000 -pthr .001 -fwhm 4 -quiet -fast
```
++ AlphaSim: AFNI version=AFNI_2008_02_01_1144 (Jul 3 2008) [32-bit] ++ Authored by: B. Douglas Ward ++ default NN connectivity being used
 
<br>

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
You can also use a mask file to calculate the voxel count
```
 AlphaSim -mask group_mask_43a+tlrc. -dxyz 3.75 3.75 4 -iter 1000 -pthr 0.001 -fwhm 4 -quiet -fast
```
One method to use AlphaSim is to set the p-value threshold you want (e.g. to allow separation of clusters/regions) and then use AlphaSim to determine the appropriate cluster size (Adcock 2006).

### AlphaSim Manual
Simultaneous Inference for AFNI Data

B. Douglas Ward

Biophysics Research Institute, Medical College of Wisconsin

[File:AlphaSim Ward.pdf](https://web.stanford.edu/group/spanlab/cgi-bin/wiki/index.php?title=File:AlphaSim_Ward.pdf)

[Download PDF](https://web.stanford.edu/group/spanlab/cgi-bin/wiki/images/AlphaSim_Ward.pdf)

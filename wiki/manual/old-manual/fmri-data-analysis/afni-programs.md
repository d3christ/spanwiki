# Roundup of Useful AFNI Programs and Plugins

## Contents
  1. [Dataset Creation and Conversion](#1)
  2. [Auxiliary Programs for Dataset Creation from Images](#2)
  3. [Quality Checks for 3D+time Datasets](#3)
  4. [3D+time Pre-Processing Programs](#4)
  5. [3D+time Analysis Programs](#5)
  6. [Model 1D Time Series Generators](#6)
  7. [Dataset Histogram and Segmentation Programs](#7)
  8. [Group Dataset Statistical Analysis Programs](#8)
  9. [Programs for Manipulating Information in the Dataset Header](#9)
  10. [Programs for Changing Dataset Spatial Structure](#10)
  11. [Programs for Assembling Sub-bricks into 4D Datasets](#11)
  12. [Programs for Changing Slice Structure](#12)
  13. [Spatial Transformations of Dataset Geometry](#13)
  14. [Dataset File Manipulation](#14)
  15. [ROI Generation and Usage Programs](#15)
  16. [Simple Calculations on Datasets, Producing New Datasets](#16)
  17. [Computation of Various Numbers from Datasets](#17)
  18. [Simulated Dataset Generators](#18)
  19. [Programs for Dealing with 1D Time Series](#19)
  20. [Image Registration Programs](#20)
  21. [Miscellaneous File Manipulations](#21)
  22. [Miscellaneous Utilities](#22)
  23. [Image File Header Printouts](#23)
  24. [Miscellaneous Visualization Tools](#24)
  25. [Surface mapping tools](#25)
  26. [Miscellaneous Scripts and Script Tools](#26)
  
<a name='1'></a>
## Dataset Creation and Conversion
  - [to3d](http://afni.nimh.nih.gov/pub/dist/doc/program_help/to3d.html)<br>
    Read image files, write AFNI format datasets
  - [3dAFNIto3D](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dAFNIto3D.html)<br>
    Convert AFNI format dataset to .3D format (ASCII lists)
  - [3dAFNItoANALYZE](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dAFNItoANALYZE.html)<br>
    Convert AFNI format dataset to ANALYZE format
  - [3dAFNItoMINC](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dAFNItoMINC.html)<br>
    Convert AFNI format dataset to MINC format
  - [3dANALYZEtoAFNI](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dANALYZEtoAFNI.html)<br>
    Convert ANALYZE format dataset to AFNI format
  - [3dMINCtoAFNI](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dMINCtoAFNI.html)<br>
    Convert MINC format dataset to AFNI format
  - [3dThreetoRGB](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dThreetoRGB.html)<br>
    Convert 3 scalar datasets to 1 RGB AFNI format dataset

<a name='2'></a>
## Auxiliary Programs for Dataset Creation from Images
  - [Ifile](http://afni.nimh.nih.gov/pub/dist/doc/program_help/Ifile.html)<br>
    Read GE realtime EPI files and runs to3d
  - [Imon](http://afni.nimh.nih.gov/pub/dist/doc/program_help/Imon.html)<br>
    Read GE realtime EPI files as they are created
  - [Dimon](http://afni.nimh.nih.gov/pub/dist/doc/program_help/Dimon.html)<br>
    Read DICOM files on disk or as they are created
  - [rtfeedme](http://afni.nimh.nih.gov/pub/dist/doc/program_help/rtfeedme.html)<br>
    Dissect one dataset, sends images to AFNI realtime plugin
  - __plugin RT Options__<br>
    Control options for AFNI realtime image input
  - [from3d](http://afni.nimh.nih.gov/pub/dist/doc/program_help/from3d.html)<br>
    Write dataset slices into image files
  - [abut](http://afni.nimh.nih.gov/pub/dist/doc/program_help/abut.html)<br>
    Create zero-filled slices to put into dataset gaps   
 
<a name='3'></a>
## Quality Checks for 3D+time Datasets
  - [3dToutcount](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dToutcount.html)<br>
    Check voxel time series for quality (temporal outliers)
  - [3dTqual](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dTqual.html)<br>
    Check dataset sub-bricks for quality (spatial outliers)
    
<a name='4'></a>
## 3D+time Pre-Processing Programs
  - [3DTshift](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dTshift.html)<br>
    Shift slices to a common time origin (temporal interpolation)
  - [3dDespike](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dDespike.html)<br>
    Remove spikes from voxel time series
  - [3dDetrend](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dDetrend.html)<br>
    Remove trends from voxel time series
  - [3DFourier](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dFourier.html)<br>
    FFT-based lowpass and highpass filtering
  - [3dTsmooth](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dTsmooth.html)<br>
    Smooth time series in the time domain
    
<a name='5'></a>
## 3D+time Analysis Programs
  - [3dDeconvolve](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dDeconvolve.html)<br>
    Multiple linear regression and deconvolution
  - [3dSynthesize](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dSynthesize.html)<br>
    Compute 3d+time dataset from partial model
  - __plugin Deconvolution__<br>
    Interactive deconvolution
  - [3ddelay](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3ddelay.html)<br>
    Single regressor linear analysis with time shifting
  - [3dNLfim](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dNLfim.html)<br>
    Nonlinear regression
  - __plugin Nlfit & Nlerr__<br>
    Interactive nonlinear regression
  - [3dTcorrelate](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dTcorrelate.html)<br>
    Correlate two input datasets, voxel-by-voxel
  - [3dAutoTcorrelate](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dAutoTcorrelate.html)<br>
    Correlate each voxel with every other voxel
  - [3dpc](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dpc.html)<br>
    Principal component analysis
    
<a name='6'></a>
## Model 1D Time Series Generators
  - [sqwave](http://afni.nimh.nih.gov/pub/dist/doc/program_help/sqwave.html)<br>
    Generate a square wave (a very old program)
  - [waver](http://afni.nimh.nih.gov/pub/dist/doc/program_help/waver.html)<br>
    Generate hemodynamic responses to stimulus time series

<a name='7'></a>
## Dataset Histogram and Segmentation Programs
  - [3dAnhist](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dAnhist.html)<br>
    Create and plot histogram of dataset, print peaks
  - [3dhistog](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dhistog.html)<br>
    Create histogram of dataset to a file
  - __plugin Histogram__<br>
    Interactively graphs histogram of a dataset (or ROI)
  - __plugin ScatterPlot__<br>
    Interactively graphs 1 sub-brick vs. another (or ROI)
  - [3dClipLevel](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dClipLevel.html)<br>
    Find value to threshold off outside-the-brain voxels
  - [3dUniformize](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dUniformize.html)<br>
    Correct T1-weighted dataset for non-uniform histogram
  - [3dIntracranial](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dIntracranial.html)<br>
    Strip off outside-the-brain voxels
  - [3dSkullStrip](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dSkullStrip.html)<br>
    Enhanced skull stripping
  - __plugin Gyrus Finder__<br>
    Interactively segment gray and white matter
    
<a name='8'></a>
## Group Dataset Statistical Analysis Programs
  - [3dttest](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dttest.html)<br>
    Paired and unpaired t-tests
  - [3dANOVA](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dANOVA.html)<br>
    1-way ANOVA (fixed effects)
  - [3dANOVA2](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dANOVA2.html)<br>
    2-way ANOVA (fixed, random, mixed effects)
  - [3dANOVA3](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dANOVA3.html)<br>
    3-way ANOVA (fixed, random, mixed effects)
  - __GroupAna__<br>
    n-way (1-5) ANOVA (MatLab script)
  - [3dFriedman](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dFriedman.html)<br>
    Nonparametric Friedman test
  - [3dKruskalWallis](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dKruskalWallis.html)<br>
    Nonparametric Kruskal-Wallis test
  - [3dWilcoxon](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dWilcoxon.html)<br>
    Nonparametric Wilcoxon test
  - [3dMannWhitney](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dMannWhitney.html)<br>
    Nonparametric 3dMannWhitney test
  - [3dRegAna](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dRegAna.html)<br>
    Voxel-wise linear regression analyses
  - [3dFDR](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dFDR.html)<br>
    False Discovery Rate analysis
  - [AlphaSim](http://afni.nimh.nih.gov/pub/dist/doc/program_help/AlphaSim.html<br>
    Monte Carlo simulation for multiple comparison correction
  - [1dSEM](http://afni.nimh.nih.gov/pub/dist/doc/program_help/1dSEM.html)<br>
    Structural Equation Modeling (path analysis)
 
 <a name='9'></a>
 ## Programs for Manipulating Information in the Dataset Header
  - [3dinfo](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dinfo.html)<br>
    Print out information from the header
  - [3dAttribute](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dAttribute.html)<br>
    Print out a single header attribute
  - [3dnewid](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dnewid.html)<br>
    Assign a new ID code to a dataset
  - [3drefit](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3drefit.html)<br>
    Lets you change attributes in a dataset header
  - [3dNotes](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dNotes.html)<br>
    Lets you put text notes into a dataset header
  - __plugin Dataset NOTES__<br>
    Interactive header notes editor
  - [nifti_tool](http://afni.nimh.nih.gov/pub/dist/doc/program_help/nifti_tool.html)<br>
    Displays, modifies, copies nifti structures in datasets
 
<a name='10'></a>
## Programs for Changing Dataset Spatial Structure
  - [3daxialize](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3daxialize.html)<br>
    Rewrite dataset with slices in different direction
  - [3dresample](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dresample.html)<br>
    Rewrite dataset in new orientation, with new voxel size
  - [3dLRflip](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dLRflip.html)<br>
    Flip dataset Left ↔ Right

<a name='11'></a>
## Programs for Assembling Sub-bricks into 4D Datasets
  - [3dTcat](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dTcat.html)<br>
    Assemble a 3D+time dataset from multiple input sub-bricks
  - [3dbucket](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dbucket.html)<br>
    Assemble a bucket dataset from multiple input sub-bricks

<a name='12'></a>
## Programs for Changing Slice Structure
  - [3dZcat](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dZcat.html)<br>
    Glue multiple sub-bricks together along the z-axis
  - [3dZcutup](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dZcutup.html)<br>
    Cut slices out of a dataset to make a ‘thinner’ dataset
  - [3dZeropad](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dZeropad.html)<br>
    Add zero slices around the edges of a dataset
  - [3dZregrid](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dZregrid.html)<br>
    Interpolate a dataset to a different slice thickness

<a name='13'></a>
## Spatial Transformations of Dataset Geometry
  - [3drotate](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3drotate.html)<br>
    Rigid body rotation of dataset in 3D
  - [3dWarp](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dWarp.html)<br>
    Non-rigid transformation of 3D coordinates
  - [3dAnatNudge](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dAnatNudge.html)<br>
    Try to align EPI and structural volumes automatically
  - __plugin Nudge Dataset__<br>
    Align EPI and structural volumes manually
  - [3dTagalign](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dTagalign.html)<br>
    Align datasets by matching manually placed ‘tags’
  - __plugin Edit Tagset__<br>
    Place ‘tags’ in a dataset interactively
  - [adwarp](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dTagalign.html)<br>
    Transform dataset using warp from dataset header
  - [Vecwarp](http://afni.nimh.nih.gov/pub/dist/doc/program_help/Vecwarp.html)<br>
    Transform 3-vectors using warp from dataset header
    
<a name='14'></a>
## Dataset File Manipulation
  - [3dcopy](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dcopy.html)<br>
    Copy a dataset to make new files
  - [3drename](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3drename.html)<br>
    Rename dataset files
  - [3ddup](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3ddup.html)
    Make an ‘empty’ duplicate (warp-on-demand) of a dataset
  - [3dTwotoComplex](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dTwotoComplex.html)<br>
    Create complex dataset from two sub-bricks
  - [3dEmpty](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dEmpty.html)<br>
    Create header file only for specified dimensions
    
<a name='15'></a>
## ROI Generation and Usage Programs
  - __plugin Draw Dataset__<br>
    Manually draw ROI mask datasets
  - [3dAutomask](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dAutomask.html)<br>
    Generate a brain and skull-only mask
  - [3dAutobox](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dAutobox.html)<br>
    Automatically crop a dataset to remove empty space
  - [3dmaskave](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dmaskave.html)<br>
    Calculate dataset values averaged over a ROI
  - [3dmaskdump](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dmaskave.html)<br>
    Output all dataset values in a ROI
  - [3dROIstats](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dROIstats.html)<br>
    Calculate dataset values from multiple ROIs
  - [3dUndump](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dUndump.html)<br>
    Create dataset from text (inverse of 3dmaskdump)
  - [3dOverlap](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dOverlap.html)<br>
    Create mask that is overlap of nonzero voxels from multiple datasets
  - [3dfractionize](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dfractionize.html)<br>
    Resample a mask dataset to a different resolution
  - [whereami](http://afni.nimh.nih.gov/pub/dist/doc/program_help/whereami.html)<br>
    Get atlas region name for coordinates
    
<a name='16'></a>
## Simple Calculations on Datasets, Producing New Datasets
  - [3dcalc](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dcalc.html)<br>
    Voxel-by-voxel general purpose calculator
  - [3dmerge](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dmerge.html)<br>
    Various spatial filters, thresholds, and averaging
  - [3dTstat](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dTstat.html)<br>
    Various statistics of multi-brick datasets, voxel-by-voxel
  - [3dMean](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dMean.html)<br>
    Average datasets together, voxel-by-voxel, for each timepoint
  - [3dWinsor](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dWinsor.html)<br>
    Nonlinear order statistics filter for spatial smoothing
  - [3danisosmooth](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3danisosmooth.html)<br>
    Edge preserving filter for spatial smoothing:
  - [3dLocalstat](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dLocalstat.html)<br>
    Find simple statistical values for neighborhoods around each voxel
  - [3dLocalBistat](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dLocalBistat.html)<br>
    Compute various bivariate statistics for neighborhoods around each voxel
  - [3dmatcalc](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dmatcalc.html)<br>
    Applies matrix to datasets
 
<a name='17'></a>
## Computation of Various Numbers from Datasets
  - [3ddot](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3ddot.html)<br>
    Dot product (correlation coefficient) of 2 sub-bricks
  - [3dclust](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dclust.html)<br>
    Find spatially connected clusters of nonzero voxels
  - [3dStatClust](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dStatClust.html)<br>
    Find statistically connected clusters
  - [3dExtrema](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dExtrema.html)<br>
    Find local maxima (or minima) of datasets
  - [3dFWHM](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dFWHM.html)<br>
    Estimate Full Width Half Max of dataset spatial correlation
  - [3dFWHMx](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dFWHMx.html)<br>
    Estimate FWHM for all sub-bricks of dataset
  - [3dBlurToFWHM](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dBlurToFWHM.html)<br>
    Spatially variable blurring for uniform FWHM
  - [3dBrickStat](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dBrickStat.html)<br>
    Simple statistics (max, min, mean) for scripts
  - [3dGetrow](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dGetrow.html)<br>
    Output voxel values for a row/column in x,y,z space
  - [3dDWItoDT](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dDWItoDT.html)<br>
    Compute diffusion tensor,eigenvalues from DWI data
  - [3dDTeig](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dDTeig.html)<br>
    Compute eigenvalues from diffusion tensor data
    
<a name='18'></a>
## Simulated Dataset Generators
  - [3dTSgen](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dTSgen.html)<br>
    Generate 3D+time dataset from 1D model and noise
  - [AlphaSim](http://afni.nimh.nih.gov/pub/dist/doc/program_help/AlphaSim.html)<br>
    Simulate datasets and estimate statistical power
  - [3dConvolve](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dConvolve.html)<br>
    Simulate datasets via convolution
  - [3dInvFMRI](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dInvFMRI.html)<br>
    Compute stimulus time series given activation map and 3D+time dataset
    
<a name='19'></a>
## Programs for Dealing with 1D Time Series
  - [1dcat](http://afni.nimh.nih.gov/pub/dist/doc/program_help/1dcat.html)<br>
    Catenate them horizontally
  - [1deval](http://afni.nimh.nih.gov/pub/dist/doc/program_help/1deval.html)<br>
    1D calculator (like 3dcalc for 1D files)
  - [1dplot](http://afni.nimh.nih.gov/pub/dist/doc/program_help/1dplot.html)<br>
    Graph values from columns in a file
  - [1dgrayplot](http://afni.nimh.nih.gov/pub/dist/doc/program_help/1dgrayplot.html)<br>
    Show values from columns in a file as bands of gray levels
  - [1dtranspose](http://afni.nimh.nih.gov/pub/dist/doc/program_help/1dtranspose.html)<br>
    Transpose 1D files (interchange rows and columns)
  - [1dmatcalc](http://afni.nimh.nih.gov/pub/dist/doc/program_help/1dmatcalc.html)<br>
    Matrix calculator for 1D files
  - [1dMarry](http://afni.nimh.nih.gov/pub/dist/doc/program_help/1dMarry.html)<br>
    Combine ragged 1D files for use with 3dDeconvolve's -stim_times_AM2 option
    
<a name='20'></a>
## Image Registration Programs
  - [3dvolreg](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dvolreg.html)<br>
    Volumetric registration (rigid body in 3D)
  - [3dWarpDrive](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dWarpDrive.html)<br>
    Enhanced volumetric registration, includes warping
  - [Allineate](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dAllineate.html3d)<br>
    Cross-modality affine volume registration
  - [2dImReg](http://afni.nimh.nih.gov/pub/dist/doc/program_help/2dImReg.html)
    Slice-by-slice registration (rigid body in 2D)

<a name='21'></a>
## Miscellaneous File Manipulations
  - [2swap](http://afni.nimh.nih.gov/pub/dist/doc/program_help/2swap.html)<br>
    Byte pair swap: ab ba
  - [4swap](http://afni.nimh.nih.gov/pub/dist/doc/program_help/4swap.html)
    Byte quad swap: abc dcba
  - [24swap](http://afni.nimh.nih.gov/pub/dist/doc/program_help/24swap.html)<br>
    Mixed 2 and 4 byte swaps in same file
  - [strblast](http://afni.nimh.nih.gov/pub/dist/doc/program_help/strblast.html)<br>
    Find a string in a file and replace it with junk
    
<a name='22'></a>
## Miscellaneous Utilities
  - [byteorder](http://afni.nimh.nih.gov/pub/dist/doc/program_help/byteorder.html)
    Report the byteorder of the current CPU
  - [ccalc](http://afni.nimh.nih.gov/pub/dist/doc/program_help/ccalc.html)<br>
    A command line calculator (like 3dcalc)
  - [cdf](http://afni.nimh.nih.gov/pub/dist/doc/program_help/cdf.html)<br>
    Compute probabilities, thresholds for standard distributions
  - [count](http://afni.nimh.nih.gov/pub/dist/doc/program_help/count.html)<br>
    Generate numbered strings for command line scripts

<a name='23'></a>
## Image File Header Printouts
  - [dicom_hdr](http://afni.nimh.nih.gov/pub/dist/doc/program_help/dicom_hdr.html)<br>
    Print information from a DICOM file
  - [ge_header](http://afni.nimh.nih.gov/pub/dist/doc/program_help/ge_header.html)<br>
    Print information from a GE I. file
  - [mayo_analyze](http://afni.nimh.nih.gov/pub/dist/doc/program_help/mayo_analyze.html)<br>
    Print information froman ANALYZE .hdr file
  - [siemens_vision](http://afni.nimh.nih.gov/pub/dist/doc/program_help/siemens_vision.html)<br>
    Print information from a Siemens Vision .ima file

<a name='24'></a>
## Miscellaneous Visualization Tools
  - [aiv](http://afni.nimh.nih.gov/pub/dist/doc/program_help/aiv.html)<br>
    AFNI Image Viewer program
  - __plugin Render\[new]__<br>
    Interactive volume rendering
  - __plugin Dataset#N__<br>
    Graph extra dataset time series in AFNI graph viewer
    
<a name='25'></a>
## Surface mapping tools
  - [SUMA](http://afni.nimh.nih.gov/afni/suma)<br>
    Surface Mapping display
  - [DriveSuma](http://afni.nimh.nih.gov/pub/dist/doc/program_help/DriveSuma.html)<br>
    Send commands to SUMA program from script
  - [@SUMA_Make_Spec_FS](http://afni.nimh.nih.gov/pub/dist/doc/program_help/@SUMA_Make_Spec_FS.html)<br>
    Convert Freesurfer surfaces to SUMA spec files
  - [@SUMA_Make_Spec_SF](http://afni.nimh.nih.gov/pub/dist/doc/program_help/@SUMA_Make_Spec_SF.html)<br>
    Convert SureFit surfaces to SUMA spec files
  - [3dSurf2Vol](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dSurf2Vol.html)<br>
    Compute volume equivalent from surface or pair of surfaces
  - [3dVol2Surf](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dVol2Surf.html)<br>
    Assign values to surface nodes from volumetric data
  - [3dSurfMask](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dSurfMask.html)<br>
    Generate volumetric mask for inside of surface
  - [CompareSurfaces](http://afni.nimh.nih.gov/pub/dist/doc/program_help/CompareSurfaces.html)<br>
    Compute distances between two surfaces at each node
  - [ConvertSurface](http://afni.nimh.nih.gov/pub/dist/doc/program_help/ConvertSurface.html)<br>
    Convert surface files among various formats
  - [IsoSurface](http://afni.nimh.nih.gov/pub/dist/doc/program_help/IsoSurface.html)<br>
    Extract isosurface from a volume
  - [SurfClust](http://afni.nimh.nih.gov/pub/dist/doc/program_help/SurfClust.html)<br>
    Find clusters on surfaces
  - [SurfDsetInfo](http://afni.nimh.nih.gov/pub/dist/doc/program_help/SurfDsetInfo.html)<br>
    Display information about surface dataset
  - [SurfInfo](http://afni.nimh.nih.gov/pub/dist/doc/program_help/SurfInfo.html)<br>
    Show information on surface
  - [SurfMeasures](http://afni.nimh.nih.gov/pub/dist/doc/program_help/SurfMeasures.html)<br>
    Compute various measurements for surface or pair of surfaces
  - [SurfMesh](http://afni.nimh.nih.gov/pub/dist/doc/program_help/SurfMesh.html)<br>
    Reduce number of points in surface mesh
  - [SurfPatch](http://afni.nimh.nih.gov/pub/dist/doc/program_help/SurfPatch.html)<br>
    Extract patch of surface or compute volume from specified nodes
  - [SurfQual](http://afni.nimh.nih.gov/pub/dist/doc/program_help/SurfQual.html)<br>
    Quality check for surfaces
  - [SurfSmooth](http://afni.nimh.nih.gov/pub/dist/doc/program_help/SurfSmooth.html)<br>
    Smooth surfaces
  - [SurftoSurf](http://afni.nimh.nih.gov/pub/dist/doc/program_help/SurfToSurf.html)<br>
    Interpolate data from one surface onto mesh of another surface
  - [SurfaceMetrics](http://afni.nimh.nih.gov/pub/dist/doc/program_help/SurfaceMetrics.html)<br>
    Provides information on surface mesh
  - [MapIcosahedron](http://afni.nimh.nih.gov/pub/dist/doc/program_help/MapIcosahedron.html)<br>
    Create new version of surface mesh using mesh of icosahedron
    
<a name='26'></a>
## Miscellaneous Scripts and Script Tools
  - [afni_proc.py](http://afni.nimh.nih.gov/pub/dist/doc/program_help/afni_proc.py.html)<br>
    Python program to generate tcsh script for processing single subject FMRI data
  - [@auto_tlrc](http://afni.nimh.nih.gov/pub/dist/doc/program_help/@auto_tlrc.html)<br>
    Automatic transformation of dataset to match Talairach template
  - [@CommandGlobb](http://afni.nimh.nih.gov/pub/dist/doc/program_help/@CommandGlobb.html)<br>
    Execute AFNI commands for multiple datasets
  - [@make_stim_file](http://afni.nimh.nih.gov/pub/dist/doc/program_help/@make_stim_file.html)<br>
    Make stim file for 3dDeconvolve from user input or file
  - [@UpdateAfni](http://afni.nimh.nih.gov/pub/dist/doc/program_help/@UpdateAfni.html)<br>
    Sample script for updates (also AFNI_UPDATER)

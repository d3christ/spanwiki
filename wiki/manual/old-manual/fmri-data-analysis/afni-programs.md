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

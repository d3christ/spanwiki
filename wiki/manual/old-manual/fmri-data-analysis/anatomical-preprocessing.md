# Anatomical Preprocessing

## Contents
  1. [Reconstructing Anatomicals](#reconstructing)
      1. [Quick Reference](#quick-reference)
      2. [Detailed Directions](#detailed-directions)
  2. [Nudging](#nudging)
      1. [Set up](#set-up)
      2. [Aligning](#aligning)
  3. [Warping](#warping)
      1. [Automatic Warping](#automatic-warping)
      2. [Manual Method](#manual-method)
      
<a name='reconstructing'></a>
## Reconstructing Anatomicals
The scanner gives you the anatomical images in dicom format, so you have to transform the images into afni format to use them. You can reconstruct the anatomical at any time, but you must complete [functional preprocessing](functional-preprocessing.md) before [nudging](#nudging) and [warping](#warping) the anatomicals.

<a name='quick-reference'></a>
### Quick Reference
Example of transforming high resolution anatomical scans to afni format:
```
$ cd subjectFolder/SCANEXAM#_MMDDYEAR/006/
$ to3d -prefix sag116 I*
$ mv sag116* ../../
```
<a name='detailed-directions'></a>
### Detailed Directions
To reconstruct the anatomical scans, go into the subject’s folder for their specific scan and list the files. (This is the folder you copied using the X-11 program at the scanner).
```
$ cd subjectFolder/SCANEXAM#_MMDDYEAR
$ ls
```
You should see three directories. You can tell the difference between them based on the number of files they contain:
  1. Three plane localizer scan (? files) - no need to reconstruct these
  2. Axial in-plane scan (24 files) - name these axi24
  3. 3d Volume scan (116 files) - name these sag116
  
You will only need the Axial in-plane (low resolution anatomical) and the 3d Volume (high resolution antomical) data. Here's how to get them:

First change to the Axial in-plane directory. For example:
```
$ cd 005 
```
Next use this command:
```
$ to3d -prefix axi24 I*
```
"to3d" is an afni command for transforming files into afni format. The "-prefix axi24" argument means "name the output axi24" and the "I*" argument means "Use all the files that start with I in this directory".

Finally, move your new afni files into your subject's top directory:
```
$ mv axi24* ../../
```
Then do the same thing for the 3dvolume scan, but this time name the output 'sag116'. For example:
```
$ cd ../006/
$ to3d -prefix sag116 I*
$ mv sag116* ../../
```
Remember, use ‘sag116’ to name the high-resolution anatomical scans, and ‘axi24’ to name the low-res in-plane scans.

You should check that the anatomical images have been created successfully by opening the AFNI interface. Go back to you subject's regular directory and open afni. Then set sag116 as your underlay. Bring up and scroll through images in all three planes.

This is what the brain viewing part of the AFNI interface looks like. The left image is the "axial" view. The middle image is the "sagittal" view. And the image on the right is the "coronal" view.
![Afninav.jpg]()

<a name='nudging'></a>
## Nudging
During [functional preprocessing](functional-preprocessing.md), all of your functional data should have been [coregistered](functional-preprocessing.md) to a brain early in your experiment in order to correct for motion. Thus, you will have to correct for any drifting over the course of your experiment or any sudden movements right before the anatomical scan by literally 'nudging' your anatomical scans to match your functional scans.

Nudging is a bit of an art, so find someone in the lab to actually show you how it's done. Here is an overview to give you the idea and to serve as a reference later.


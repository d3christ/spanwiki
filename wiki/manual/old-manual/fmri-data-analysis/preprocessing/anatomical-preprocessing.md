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

<a name='set-up'></a>
### Set up
  - First you must have already completed your functional preprocessing.
  - Open afni and Select the anat (sag116) file as Underlay and the blurred (non-normalized) functional data as Overlay (e.g. midAllb or mid12b).
  - Use "Define Overlay" to change the following settings:
    - Set the box at the bottom labeled ** to 4
    - Set the other box at the bottom labeled # to **
    - Click the box next to "AutoRange"
    - Then smooth the overlay using:
      - Define Datamode -> Warp OLay on Demand -> OLay resam mode -> Cu
  - Now you should be seeing a vaguely brain looking thing in a color scale.
    - (use the slider to restrict the color to inside the brain.)
  - In each window where the brain slice is shown, click the down arrow next to the number 9 on the right side of the window. This will make the overlay transparent.
  - Next open the Nudge plug in using:
    - Define Datamode -> Plugins -> Nudge Dataset
  - Next, in the "AFNI Nudger" window click "Choose Dataset" (in the top left corner) and select "sag116" (DO NOT select the functional data!)
  
<a name='aligning'></a>
### Aligning
Now you're ready to start nudging. This is the part you'll need help with initially. The idea of nudging is to align the brain structures in the functional image (the colored overlay) with those on the anatomical image (the black and white underlay). To do this you will move the anatomical underlay to match the functional overlay.

We primarily concentrate on the location of the superior corpus callosum, anterior corpus callosum, the membrane between the cerebellum and occipital lobe, and the left and right ventricles. It’s also useful when you first begin the nudge to look at the position of the functional data relative to the brain in the axial view – from here you can correct gross movements like the head settling in the pillow (requiring a nudge of the anatomical in the anterior direction) or side-to-side movement (align the functional data between the two sides of the skull).

Once you've determined a direction to nudge your anatomical image, here's how to nudge the data in afni:
  - To nudge, you will ONLY be changing the values in the row labeled “Shifts.” Do not change any of the values in the row to the right of “Angles,” as these will create angular nudges.
    - (This interface is not intuitive, as the angular row entries are named with letters that seem to complement the S, L, and P in the Shifts row below.)
  - Here's how to move the anatomical data in each of the 6 possible directions:
    - Superior - type 1 in the "Shifts: S" box
    - Inferior - type -1 in the "Shifts: S" box
    - Left - type 1 in the "Shifts: L" box
    - Right - type -1 in the "Shifts: L" box
    - Posterior - type 1 in the "Shifts: P" box
    - Anterior - type -1 in the "Shifts: P" box
  - To complete the move hit "Nudge" in the bottom left corner
  - Use the clear button to remove the previous values and get ready for the next nudge.
  
When you’re happy with how you’ve aligned the datasets, record the distance in mm of all three nudges (S L P) in the subject’s note file. This info appears to the RH side of the nudge plugin box. Then click “DO ALL.” Be careful! This will write over your anatomical (+orig.\*) dataset! Your new orig file will be your anatomical data, but in its new nudged coordinates.
  - Make sure you clicked __DO ALL__ before closing the nudge plug in.
  
If you forgot to record the transformations them before clicking ‘Do All,’ you can type
```
$ 3dinfo anatfilename+orig 
```
which will tell you any transformations that have been performed on the dataset.

<a name='warping'></a>
## Warping
Warping is the process we use to align each subject's brain to the standard Talairach brain. This allows us to compare brains of individuals across a group. You must do this before making any group maps. As of July 2010, we will be slowly switching to the automatic method for warping datasets. For a tech report on this click here. If you are new to warping, it is advised that you understand what is done for manual warping. This will ensure that you will know what to look for when inspecting automatically warped datasets.

<a name='automatic-warping'></a>
### Automatic Warping
_Under construction_

<a name='manual-method'></a>
### Manual Method

#### AC-PC alignment
First you will need to create an AC-PC aligned brain by placing five markers (AC = Anterior Commissure, PC = Posterior Commissure).

##### Set Up
  - First, open afni and set the underlay to be "sag116"
  - Then click DEFINE MARKERS and select ALLOW EDITS.
  - You will see the following list of landmarks:
    - AC superior edge
    - AC posterior margin
    - PC inferior edge
    - First mid-sag pt
    - Another mid-sag pt
  - You will identify each marker using the instructions bellow
  - When you find the location click "Set" and move on to the next marker
  
##### AC superior edge
This is the “moustache,” which you can initially locate in the sagittal view (find the white dot in the center of the brain below the corpus callosum and just slightly below the striatum) before fine-tuning placement using the axial and coronal views. The superior edge marker should be set at the top slice where the ac is visible in the axial view, and in the coronal view you will want to find the slice where the ac is thickest.

Anterior Commissure: ![Ac.png]()

Superior Edge: ![Ac sup1.png]() ![Ac sup2.png]() ![Ac sup3.png]()

##### PC inferior edge
This structure doesn’t show up well at this resolution; it’s at the top of the cerebral aqueduct, so if you can’t see it, set the location at the mid-sagittal top of the cerebral aqueduct. Use the axial and coronal views to locate it; the best way to learn is to ask a lab member to point it out to you.

Posterior Commissure: ![pc.png]()

Inferior Edge: ![Pc inf1.png]() ![Pc inf2.png]()

##### First mid-sag pt
Use a spot in the middle of the hemispheres anterior to Corpus Callosum. You should place the marker on the falx cerebri (the meninge tissue that extends down between the hemispheres). Zoom in on the axial and coronal images to find the dark separation between the hemispheres; in those areas you should also be able to see the grayish tissue in the sagittal view.

![FirstMS1.png]() ![FirstMS2.png]() ![FirstMS3.png]()

##### Another mid-sag pt
Use a spot in the middle of the hemispheres superior and posterior to Corpus Callosum. Again, place the marker somewhere on the falx cerebri.

(to maximize subcortical coregistration, we usually select a spot somewhere above the middle of the corpus callosum...bk)

##### Transforming the data
  - Press QUALITY after you’ve set your last point. This will let you know whether you’ve set the points in such a way that they translate to a reasonable plane. (you’re aiming for less than 2% deviation in the plane)
    - If you get an error message (which you often will), play around with moving the two mid-sagittal points until AFNI is happy and gives you a number less than 2.
  - When you get no error message, hit TRANSFORM DATA, which will close the panel and create “+acpc” datasets, which you can view.
  - Click "AC-PC Aligned" in afni's middle panel
  - Make sure your x-axis is zeroed out ( = 0). Now scroll through from the front of the brain to the back of the brain in the coronal view. Your z-axis crosshair, centered on the x-axis at 0, should pass between the two hemispheres at least as far back as the somatosensory cortex. If this is not the case (that is, if gyri are slopping over from one side of the 0 line to the other), go back and adjust your markers until this no longer happens.
  
#### Final Talairach Alignment
This part lets you set 6 markers to define the Talairach box for the Talairach transformation. You’ll need to begin in the ac-pc view to set these markers.

##### Set Up
  - Select AC-PC view
  - Select Define Markers and click Allow Edits.
  - Now you will be placing the following six markers to define the edges of the brain.
  - For each marker you want to pick the first or last slice where actual brain tissue can be seen. Be sure to view each marker in all three window views to make sure you’ve placed it correctly. (This is particularly true of the inferior marker, which is difficult since the temporal lobes swoop down the sides further than you’d expect)
  
##### Anterior and Posterior points
Use the coronal view to place the ANTERIOR and POSTERIOR markers. You want to place these markers at the most anterior and posterior parts of the brain … that is, the last place in the front and back and of the head that actual brain tissue is present.

##### Superior and Inferior points
Use the axial and the coronal view to place the SUPERIOR marker and the INFERIOR marker (which is the hardest marker to place, at the base of the temporal lobe, since it’s near other non-brain tissue).

##### Left and Right points
Use the sagittal view to place the LEFT and RIGHT markers, but use the coronal to know which side you’re on (the images are as though the patient’s facing you, so reverse R and L).

##### Transforming the data
After you’ve placed the markers, QUALITY will tell you the size of the box you’ve created, so you can see if it’s a good fit for the Talairach box; this step will also let you know if you’ve reversed L and R.

Finally, press TRANSFORM DATA, which will create the “+tlrc” datasets, which you can view by clicking "Talairach View" in afni's middle panel.

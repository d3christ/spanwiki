# Troubleshooting

## Contents
  1. [3dinfo](#3dinfo)
  2. [Sanity-checking your data](#sanity-checking)
  3. [My data looks funny](#data-looks-funny)
    1. [Structural artifacts](#structural-artifacts)
    2. [Functional artifacts](#functional-artifacts)
    
<a name='3dinfo'></a>
## 3dinfo
The 3dinfo command can be applied to .BRIK and .HEAD files. It is useful because it will tell you when the file was made and, with anatomical .BRIK and .HEAD files, what your subject’s I and S values are. On group maps the command will allow you to see which subjects went into that particular analysis as well as which particular model was used.

3dinfo will tell you how much you’ve nudged your anatomical orig file if you fail to record that information when you nudge.

You should use 3dinfo on the z-file output of your reg script to find out what the beta (regressor) coefficients are for each contrast of interest. These are necessary for the 3dttest scripts.
```
$ 3dinfo sag116+orig.HEAD | more
```
Here, using a pipe | and the more command will allow us to view the 3dinfo page-by-page.

<a name='sanity-checking'></a>
## Sanity-checking your data
Setting overlay as underlay at this stage in data processing is a tool for checking your data for weirdness, like movement and artifacts. Later on it will also be useful for looking at group-averaged timecourses.

For now, however, check your data by setting the functional, blurred data as the underlay (this is the blurred data output by the process script – for example, “mid12b”). Then hit a graph button next to the Image buttons to display the time series of the crosshair-selected voxel. You can use C – and C shift + to resize; to move through time use the keyboard arrows.

Note that the “Graph” buttons are located to the right of the Axial, Sagittal, and Coronal view options.

As you move through your data in time using the keyboard arrow keys (make sure the mouse is over the graphed data, AFNI won’t let you do this otherwise), look for weird jumps or another graphical anomalies. The jumps are probably due to movement, but could be due to other issues, like data processing problems or even missing slices. Be on the lookout for large spikes in the graphed data – these definitely need to be investigated further if found.

<a name='data-looks-funny'></a>
## My data looks funny
Data can look scary because of problems with artifacts (material or scanner-related), processing problems, or modeling problems.

One of the most important things to do when checking processed data is to set the blurred data as the underlay in AFNI (which will display grayscale views of the functional data), and then hit the ‘graph’ button next to one of the image buttons. This will plot the MR signal throughout the whole scan. With the cursor over the graph window, arrows will scroll through time. Look for jumps in the graph and also strange patterns in the brain views.

<a name='structural artifacts'></a>
### Structural artifacts
The structural scan can reveal tumors, etc. Usually large problems are visible at the scanner, but some may turn up in pre-processing. These must be reported if significant, ask the lab manager or Anne Sawyer about reporting. The scan below revealed what was determined to be a non-clinical potential meningioma. It looks very large and possibly dangerous to the naked eye, but a neurologist determined it was not harmful after examination of the images.

![Artifact1.jpg]()

In the structural view, if the subject moved a lot during the sag116 scan, a ‘ringing’ artifact may appear. This is not a problem beyond making the landmarks hard to find.

<a name='functional-artifacts'></a>
### Functional artifacts

#### I. Minor Problems
  - Dropout: this is evident when you don’t have signal in a given region (e.g. NAcc). Happens in lower parts of the brain, but it shouldn’t here at Stanford at the 1.5T with spiral-in-out acquisition. This can be quantified by running a signal-to-noise analysis.
  - Hair pins, necklaces, etc: you will see these as big holes in the initial localizer scans on the scanner computer. It is a good practice to flip through the localizer and inplane scans to see if there are any holes. If so, you didn’t screen your subject for metal correctly! Take them out and remove the metal (unless it is implanted, of course, in which case they shouldn’t be in the scanner in the first place!).
  - Dreadlocks: we screen for these because years back a subject with dreadlocks had a huge dropout surrounding the skull. This resulted from accumulated material in the hair.
  
#### II. Spiral Artifact

![Artifact2.jpg]()

The above spiral artifact results from unspecific problems with the spiral acquisition sequence (using our normal 24-slice, 2sec TR scan). This example was late in a night, so perhaps the scanner was heating up, however, the following run looked fine. It is important to check data for this type of artifact, but it may be hard to characterize at first – primarily, every z-score map (whether it is of the motion regressors or task regressors) will have ‘striping’ outside the head (above left). When looking at ‘activation’ inside the brain, these stripes will often curve inwards along gyri, as visible in coronal views. A spiral artifact is obviously problematic for detecting activations. Answer: this happens randomly, so just beware (possible dub runs replacing the bad one with a good one).

This artifact can be easily confused with the slice artifact shown below. You may want to try the troubleshooting techniques listed there.

After you've double checked you processing script for errors, this is the right type of artifact to try asking Gary or Anne about. It may have resulted from a scanner malfunction or a problem with shimming. Be sure to tell them where (i.e. 1.5 or 3T magnet) and when the scan took place.

#### III. Slice Artifact

![Artifact3.jpg]()

Possible causes:
  - Novel scan parameters – 12 slices, 1sec TR.
  - Bad reconstruction during preprocessing which may be due to specifying the wrong number of TRs in the process script.
  
Diagnosing/fixing the problem:
  - Go back and rerun the process script. Be sure to either watch the output of grecons12 and spiralioadd carefully or record the output in a file. Check for any unusual output/errors. These scripts are usually perfectly happy to give you garbage if there is an error during processing (garbage in, garbage out).
  - If there is an error, double check and then triple check to make sure that the number of TRs and the number of slices you pass to spiralioadd is correct.
  - Another thing to check is the size of the .mag file. Use "ls -l" to find out the number of bytes in your mag file.
```
correct number of bytes in a .mag file = 8192 (size of one slice) x number-of-slices x 2 (for spiral in and out) x number-of-TRs
```

#### IV. Ring Artifact

![Artifact4.jpg]()

Another odd artifact on a rapid 1.5T scan. What looks like nice NAcc activation in the coronal view (not shown) is revealed to be artifactual in the axial view – real activation never forms nice rings like this. This is probably due to a combination of motion and the spiral out pulse sequence. Diagnosing the problem:
  - Try looking through the motion regressors (particularly pitch) to see if this ring is showing up in one of them.
  - Use the matlab function inspectMotion to test to see if any motion vectors are correlated with your task regressors.
  
Possible fixes:
  - Try reprocessing your data using only the sipral in pulse sequence (you can add an argument to grecons12 to do this). This will probably get rid of the ring but it may decrease your general signal to noise ratio.
  
For future scans:
  - Problem may be improved by changing tilt of the head or other scan parameters.

#### V. Dropped Slices

![Artifact5.jpg]()

This artifact results when the scanner for some reason doesn’t acquire data from a whole slice for a given TR. The above example is from a 12slic, 1sec TR scan, and the dropped slice artifact moves over time from the base of the brain to the top. Answer: Temporary solution is to drop the offending early scans, long-term solution is to change parameters or ask Gary to tell you how to avoid this.

You should also try the fixes listed in the slice artifact section.

#### VI. Dropout due to field strength

![Artifact6.jpg]()

Data may be better in some regions on one scanner or the other. The above example is from a 14-slice, 1sec TR scan on the 1.5T scanner (left) and 3T scanner (right). Note the ring of dropout/artifact in the ventral striatum for the 3T. Answer: change scanners.

#### VII. Dropout due to scan plane – sagittal is bad

![Artifact7.jpg]()

Colors represent signal-to-noise in a 14-slice sagittal scan (as a percent of max achieved in a 24-slice axial) – basically <20% of max throughout the whole brain. Areas with no color have the worst drop-out. Answer: do not use a sagittal acquisition.

#### VIII. Processing issues
If experimenting with a new pulse sequence, acquisition plane (sagittal is a horrible acquisition plane!), number of slices, or TR length, the resulting data from to3d (or the output of gary’s makebrick script, which runs to3d) may be upside down, flipped, etc. This can be fixed by running a few commands (e.g. 3drotate –rotate -90L 0A oI –zpad 20 …), or by making sure you have the right parameters in to3d.

#### IX. Modeling
The whole point of learning to model, and science in general, is to produce unfunny data. If your maps look funny or aren’t giving the expected result, try try again.

#### X. Float vs Short

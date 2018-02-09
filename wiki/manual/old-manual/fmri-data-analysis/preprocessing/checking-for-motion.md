# Checking for Motion

## Contents
  1. [Checking motion using matlab](#matlab)
  2. [Checking motion using afni](#afni)
  3. [Creating a Note File](#note-file)
  
<a name='matlab'></a>
## Checking motion using matlab
There is a matlab function called "inspectMotion" that you can use to view all 6 motion directions at once and get an exact measure of the largest jumps in your data. (Note: inspectMotion is not a built in function. See the lab manual scripts page if matlab doesn't recognize this command)

To use it, open matlab and run:
```
>>inspectMotion('3dmotionAll.1D')
```
This will result in a plot like the one below. The highlighted values are used to point out questionable jumps. The orange highlights mean the jump is < 1mm but > .5mm (which is ok) and the red highlights mean the jump is > 1mm (which may or may not be ok depending on the study). The title of each plot tells you the direction of that motion vector as well as the corresponding afni index that you would use with 1dplot.

![InspectMotionplot.jpg]()

Note: The default is for inspectMotion to look across 3 TRs at a time when it looks for "jumps" in your data. If you would like to include more or fewer TRs (for example 5 TRs) you can use a second argument which would look like this:
```
>>inspectMotion('3dmotion12.1D', 5)
```
You can also use inspectMotion to test for correlations between your 6 motion vectors and a regressor from your model. For more details, see the help file for inspectMotion.

<a name='afni'></a>
## Checking motion using afni
There are two possible commands to use to view the motion for the subject using afni from the command line.

(a) 1dplot
```
$ 1dplot 3dmotion12.1D[4] 			lets you see the z dimension alone 
$ 1dplot 3dmotion12.1D[5] 			lets you see the x dimension alone 
$ 1dplot 3dmotion12.1D[6] 			lets you see the y dimension alone 
```
(b) 1dplot -volreg
```
$ 1dplot -volreg 3dmotion12.1D[1-6\] 	
```
Lets you see all the dimensions at once, and if you want to print this you should save ps file as “XXmotion.ps,” close the window, and type “lpr XXmotion.ps”

![1dplot.jpg]()

Record the largest movement in each direction (z, x, y) in the note file.

Note that if you set the functional data (output by your script into the subject’s main directory in a format that looks something like mid12b in the case of the mid task) as underlay in AFNI, you can graph it as a time series using the graph buttons. We’ll go into more detail on this with respect to the AFNI interface shortly. [INSERT UNDERLAY EXAMPLE SCREEN CAPTURE]

<a name='note-file'></a>
## Creating a Note File
Having looked at your subject’s motion using 1dplot and AFNI, create a “note” file in their directory. When you “emacs filename” emacs will create that file if it does not already exist:
```
span@dmthal jp120406]$ gedit note
```
A typical note file will look something like this:

![Notefile.jpg]()

Make sure to include subject initials, date run, experiment name, ethnicity, gender, winnings (if applicable), the I & S values, information on movement obtained through the 3dmotion plot (here “ns” for “not significant”), date/initials of Nudge (including S,L,P values), date/initials of warp, and any notes (including information on subject behavior, strategies, technical problems with the scan or eprime, issues or questions regarding data processing, et cetera).

# File Naming Conventions

## Contents
  1. [Scanner files](#scanner-files)
  2. [AFNI files](#afni-files)
  3. [General SPAN lab file-naming](#general-spanlab)
  4. [OLDER DATA (NIH) on nac](#older-data)
      1. [Standard contrasts](#old-standard-contrasts)
      2. [Computational modeling](#computational-modeling)
  5. [New (Stanford) Data](#new-data)
      1. [Standard contrasts](#new-standard-contrasts)
      2. [Other modeling](#other-modeling)
      
<a name='scanner-files'></a>
## Scanner files
Files in the following formats
```
P#####.7 
P#####.7.mag 
P#####.7.hdr 
B##
```
are raw scanner functional image and header files (the .mag suffix indicates reconstructed raw images). These files are output by the scanner computer and contain your functional data. The header (‘hdr’) files contain what information?. They are the files you will need to transfer from the scanner computer to the Lucas Ctr. 1.5 T side room computer, and from there to the lab to be constructed into files that can be read and interpreted by AFNI.

Files in the format
```
I###.dcm 
```
are __dicom__ raw structural images from scanner. These images are also output by the scanner computer, and must also be transferred back to the lab so that you can process them with the functional data.

Dicom is a file structure used in the radiological sciences. It's like a gif or jpg in the sense that it's a simply a standardized way of encoding image information. Without such standardization no one would be able to open anyone else's files.

<a name='afni-files'></a>
## AFNI files
Once you process the data back here in the lab (more on this later), you’ll end up with files in AFNI-friendly format. They will look like this
  - __Filename+orig.HEAD:__ AFNI header file (scanner orientation – the orientation of the subject’s head in the scanner)
  - __Filename+orig.BRIK:__ AFNI data file (scanner orientation)
  - __Filename+orig[1..5]:__ specifies sub-bricks in an AFNI BRIK. A sub-brick is one brain volume of data (usually from a 2 second acquisition). An AFNI BRIK is the entire collection of volumes taken over time. (While it might make more sense for each volume to be a brick and for those bricks to make, say, a house, that isn’t the case, so deal)
  
After you’ve nudged and acpc-warped your data in AFNI (again, more on all of this later), a new file will appear in your data folder
  - __Filename+acpc.\*:__ AC-PC aligned file
  
Finally, when you’ve transformed your data to Talairach space, you’ll get a Talairach-transformed data file
  - __Filename+tlrc.\*:__ Talairach space oriented file
  
<a name='general-spanlab'></a>
## General SPAN lab file-naming
After you’ve processed your data, you’ll be applying what we call a “model” to look at data at specific points in the task or for specific comparisons (correct vs incorrect answers, win vs. loss trials, et cetera). The word ‘model’ in this sense means something a little different from the usual definition, though consistent with it. A lot of content referred to in this list will be discussed later on, so don’t worry about terms you don’t understand.

For now, though, here are file naming conventions for AFNI and other analysis data as they relate to specific experiments or common comparisons within an experiment.
  - __note__: every subject needs a note file! - record important info like I&S, L&R values, amount won/purchased, notes on behavior and maps
  - __log__: every study needs a log file! – record important stuff like useable subjects, modeling discoveries, errors, bugs, etc (because you will forget it later – and you might not even be the person looking at the study later)
  - __mid__: MID task, generally used to name a data file containing a full scan session (e.g. mid12+orig.* = AFNI format full functional MID run, sessions 1&2 concatenated)
  - __b__ (as in mid12b+orig.\*): spatially blurred data (voxels have been averaged to one degree or another)
  - __f__ (as in mid12f+orig.\*): filtered data (generally bandpass filtered – if the data has only been low or high pass filtered, then the filename should reflect the different filtering, e.g. mid12hpf indicates data filtered with a highpass filter only) \*this data type has been almost completely replaced with normalized data
  - __reg__ (as in incantreg+orig.\*): multiple regression analysis, file contains statistics for each regressor in its sub-bricks
  - __z__ (as in zincantreg+orig.\*): converted to z-scores
  - __n__ (or norm): normalized data, that is to say, data that has been converted to percent signal change
  - __master###.txt__: master vector file describing trial order and behavior for a subject
  - __??.vec__ (where ?? refers to a subjects’ initials): another name for the master vector (newer naming convention)
  - __.edat__: eprime behavioral file (accompanies a \*.txt file with same extension) – this file contains your subject’s responses for any given task. It might be useful to convert the .edat to an excel-readable \*.csv or \*.txt
  - __.csv__: spreadsheet of percent signal change averaged by trial type in a region of interest, usually for a group of subjects
  
<a name='older-data'></a>
## OLDER DATA (NIH) on nac

<a name='old-standard-contrasts'></a>
### Standard contrasts
  - __ant__: anticipation
  - __fbk__: feedback/outcome
  - __r__: reward
  - __p__: punishment
  - __n__: non/neutral (as in non gain)
  - __v__: versus (e.g. rvnant = reward versus nonreward anticipation)
  - __rewvsneu__: reward versus neutral anticipation (if time interval is not specified assume anticipation)
  - __punvsneu__: punishment versus neutral anticipation
  - __rewfbk__: reward outcomes versus failures to obtain rewards
  - __punfbk__: escaped punishment versus punishment outcomes
  - __zantfbkreg__: ??
  - __zantvsfbkreg__: regression w/ standard contrasts
  
<a name='computational-modeling'></a>
### Computational modeling
  - __g/gp__: gain prediction (distance from expected value)
  - __l/lp__: loss prediction (distance from expected value)
  - __gainvsneu__: gain prediction
  - __lossvsneu__: loss prediction
  - __gainvsnonout__: gain prediction error (correction based on outcome)
  - __lossvsnonout__: loss prediction error
  - __zantoutreg__: computational model regression
  
<a name='new-data'></a>
## New (Stanford) Data

<a name='new-standard-contrasts'></a>
### Standard contrasts
  - __ant__: anticipation
  - __out__: outcome
  - __g__: reward/gain
  - __l__: punishment/loss
  - __n__: non/neutral
  
<a name='other-modeling'></a>
### Other modeling
  - __gp/lp__: gain/loss prediction
  - __gpe/lpe__: gain/loss prediction error
  - __exputl__: expected utility
  - __ort__: orthogonalized
  - __x/int__: interaction
  - __m2v*__: lookup table (create regressors out of the master vector)

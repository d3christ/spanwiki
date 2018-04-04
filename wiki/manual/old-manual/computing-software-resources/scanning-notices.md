# Scanning Notices

under construction... for now see [Protocols]()

## Important! Higher Order Shim

_A note on a higher order shim bug from Gary Glover_
_______________________________________________________
Forwarded message ----------\
Date: Thu, 17 Jul 2008 09:27:23 -0700\
From: Gary Glover <gary.glover@stanford.edu>\
To: lucasusers1@mailman.Stanford.EDU\
Subject: more on shimming

Ladies and/or Gentlemen:

Recently I cautioned about turning OFF the shim button on the 15T and 3T1 scanners after performing a high order shim and said that this problem was fixed in 3T2. That turns out to have been a lie! This disturbing pernicious evil nasty terrible horrible awful gruesome discombobulating annoying cruel bug persists. The problem is that, behind your back and despite whatever you have saved in your protocol, the scanner will \*sometimes* turn ON the shim button, and that can
  - sometimes* destroy the shim settings. Even when it does not turn on
  
the shim button, if that button is set to AUTO, running that series can also destroy the shim setting. When the settings are thusly mis- set, any rapid image acquisitions (e.g. fmri or DTI) will likely be useless. Some users have encountered horribly disfigured functional scans; the disfigurement is because of this pernicious, etc. bug. The problem seems to have started around January of this year on 3T1.

Here is my recommendation. On all scanners, edit your protocols to make sure the Shim button is set to OFF for *every* series except the localizer and the high order shim scan. If you do high order shim (and you should at 3T), NEVER set the shim button to ON or AUTO for any other scan. But do not be complacent! Be always VIGILANT, for the wily scanner software can mess with you!! Be sure to check that button while you are prescribing and before you download the next series. Despite countless hours of non-academically-productive time, I have not been able to determine the algorithm by which the scanner decides to misbehave, but there appear to be several modes. It may be that running manual prescan has something to do with one mode.

For functional imaging I have found the following works:
  1. loc Shim button is on
  2. prescribe inplane T1 or T2, but do not scan. Shim button is off
  3. shim Shim button is on
  4. run inplane (merely download)
  5. adjust projector focus
  6. copy Rx from #2. run functional scans Shim button is off
  7. run DTI, ASL, etc Shim button is off
  8. run 3D anat Shim button is off
  
__Notes:__
  - If no inplane is collected, prescribe functional scan. By doing so, the table will move to its proper scan location, which is important when running shim scan.
  - It is not all that important to do #3 before #4, because the shim is not that critical for these anatomic scans, but hey, why not?
  - After #2, the table will move and the focus will not be the same as when you started.
  
__Bottom line:__ If you verify that the shim button is OFF for all scans performed following the shim series, all will be well. (Well, some things may not be, but that is possibly the subject for another, quieter time during which music and wine or cole slaw may be involved.)

Sigh.

If I have confused you, please ask questions. Thanks...

-- gary

## Lucas Center Lobby Camera
[Lucas Center Lobby Web Cam](http://radcam-p162.stanford.edu/CgiStart?page=Single&Direction=TiltUp&Resolution=640x480&Quality=Standard&Mode=JPEG&RPeriod=3&Size=STD&PresetOperation=Move&Language=0)

If you are in the scanning suite waiting for a subject, you can see if they arrived using the [Lucas Center Lobby Web Cam](http://radcam-p162.stanford.edu/CgiStart?page=Single&Direction=TiltUp&Resolution=640x480&Quality=Standard&Mode=JPEG&RPeriod=3&Size=STD&PresetOperation=Move&Language=0)

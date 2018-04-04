# Computing Resources

## Contents
  1. [Lab Computers](#lab-computers)
      1. [Computer Names and Locations](#computer-names)
      2. [Server Computers](#server-computers)
      3. [Connecting to the Linux computers](#connecting)
  2. [Lab Scripts](#lab-scripts)
  3. [Purchasing Software](#purchasing-software)
  4. [Stanford Unix Computing Resources](#stanford-unix-computing-resources)
  5. [Psychology Department Resources](#psychology-department-resources)
      1. [Using BIAC](#using-biac)
          1. [Mounting BIAC](#mounting-biac)
          2. [Using Symbolic Links to BIAC Space](#using-symbolic-links)
  6. [SPM](#spm)
  7. [MATLAB](#matlab)
  8. [Python](#python)
  9. [Adobe Illustrator](#adobe-illustrator)
  10. [Adobe Photoshop](#adobe-photoshop)
  11. [R](#r)
  12. [STATA](#stata)
      1. [Running STATA on Stanford Servers](#running-stata)
  13. [SPSS](#spss)
  
<a name='lab-computers'></a>
## Lab Computers

<a name='computer-names'></a>
### Computer Names and Locations
__Name	&	Room__
vta.stanford.edu 465 (the main Linux station)\
dmthal.stanford.edu 469 (second Linux station)\ 
amygdala.stanford.edu 469 (second Linux station)\
insula.stanford.edu 465 (Macbook; knutson@)\
dlpfc.stanford.edu	469 (Mac; RA computer) \
dlpfc.stanford.edu 465 (Macbook; Dual-boot Experiment Computer)\
behavior.stanford.edu 467 (Windows machine for the behavioral room)\
putamen.stanford.edu 469 (Mac; RA computer)\
caudate.stanford.edu 465 (Mac; Lab Coordinator)\
mpfc.stanford.edu	470\
nacc.stanford.edu	478 (Linux)\

<a name='server-computers'></a>
### Server Computers
dmthal and amygdala are the primary data servers in the lab. Both computers connect to the biac which is the psychology department data server. Data stored on the biac can be found in the biac2 folders in the root menus of both dmthal and amygdala. Data stored in biac2 is backed up nightly along with the rest of biac.

IMPORTANT: If dmthal and amygdala are rebooted all of the data drives including the connection to biac must be remounted.

You can find scripts in the root directories of both machines to remount the biac and all drives.

<a name='connecting'></a>
### Connecting to the Linux computers
__On a Windows computer use Xmanager:__
  - Click the Xstart button on the toolbar (lower left)
  - Select a session (span@dmthal usually)
  - Click “Run”
  - If a license window pops up, cancel it, and click Run again. The program isn’t registered but can run free forever.
  
__On a MacOS X computer use X11:__
  - Launch the X11 utility (in the launch bar at the bottom of the screen or in the Applications folder)
  - Open a terminal window if one does not open automatically (select Applications → terminal from the menu bar)
  - Type “ssh –X span@dmthal.stanford.edu” to connect to dmthal, then enter password -- note that it is best to use “ssh –Y” when on the insula MacBook, lest emacs will crash.

<a name='lab-scripts'></a>
## Lab Scripts
visit the [scripts]() page

<a name='purchasing-software'></a>
<a name='stanford-unix-computing-resources'></a>
<a name='using-symbolic-links'></a>
<a name='spm'></a>
<a name='matlab'></a>
<a name='python'></a>
<a name='adobe-illustrator'></a>
<a name='adobe-photoshop'></a>
<a name='r'></a>
<a name='stata'></a>
<a name='running-stata'></a>
<a name='spss'></a>

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
## Purchasing Software
Stanford has some great deals for both departmental and personal purchase.

[http://cgi.stanford.edu/dept/its/cgi-bin/software_portal/](http://cgi.stanford.edu/dept/its/cgi-bin/software_portal/) - Visit here and choose the software you want to find out if Stanford has a deal on it.

[http://www.stanford.edu/group/bookstore/SUprices/software.html](http://www.stanford.edu/group/bookstore/SUprices/software.html) - You can see what is available at the Stanford Bookstore here.

<a name='stanford-unix-computing-resources'></a>
## Stanford Unix Computing Resources
Stanford has some nice powerful servers you can use to run Unix/Linux programs, such as STATA.

This page has information on which computer to connect to: [Choosing the Right Computer http://www.stanford.edu/services/unixcomputing/which.html](http://www.stanford.edu/services/unixcomputing/which.html). bramble.stanford.edu is a good bet. Just connect with your SUnet ID and password.

<a name='psychology-department-resources'></a>
## Psychology Department Resources

<a name='using-biac'></a>
### Using BIAC
BIAC is the psychology department server. It is a good place to store data, because it has a nice backup routine. There are nightly backups as well as less often (weekly?) backups which are stored off site. BIAC lives at biac2.stanford.edu

<a name='mounting-biac'></a>
#### Mounting BIAC
To get access to BIAC for your computer, you will need to be connected to the Psychology Department network, and may need to send Martin Frost an email with your IP Address.

__Mount BIAC in Mac OS X__

You can mount it to your mac using the command:
```
sudo mount_nfs -P biac2:/biac2/knutson <destination folder>
```
Its a little funky because it seems that no mater where you mount it, a link to it called "knutson" shows up on your desktop. Also I'm still working out the permissions issues, so you may have trouble changing the files on there at the moment.

~Stephanie 2008/06/13

__Mount BIAC in Linux__

In order to mount BIAC, you must first sign on as a "super user." To do this, use the command:
```
su
```
When prompted, enter the spanlab password. You are now logged in as a super user.

You can mount to linux using the command:
```
mount biac2:/biac2/knutson <destination folder>
```
e.g.
```
mount biac2:/biac2/knutson /biac2
```
and
```
mount biac2:/biac2/knutson6 /biac2b
```

<a name='using-symbolic-links'></a>
#### Using Symbolic Links to BIAC Space
If you use full directory paths in your scripts instead of relative paths, you might find some of your scripts broken when your data gets moved to BIAC. You can do a search and replace with 'sed' or you can create a symbolic link to the new location of the data.

Here's an example:
```
ln -s /path/to/biac/mount /path/to/symbolic/link
```

<a name='spm'></a>
## SPM
__help:__ Jeff Cooper

<a name='matlab'></a>
## MATLAB
__help:__ Jeff Cooper, Stephanie Greer

__open source alternative:__ Octave

__computers:__ dmthal, amygdala

__Troubleshooting:__

If matlab does not start, you might need to go into the matlab73/etc directory and run './lmstart'

<a name='python'></a>
## Python
Basic and good introduction to coding in [Python](https://web.stanford.edu/group/spanlab/cgi-bin/wiki/images/Thinkpython.pdf).

<a name='adobe-illustrator'></a>
## Adobe Illustrator
__help:__ Kacey Ballard, Dan Yoo

__open source alternative:__ Inkscape

__resources:__ [Kacey Ballard's Illustrator Cheat Sheet](https://web.stanford.edu/group/spanlab/cgi-bin/wiki/images/Illustratorcheatsheet.pdf)

<a name='adobe-photoshop'></a>
## Adobe Photoshop
__help:__ Dan Yoo

__open source alternative:__ GIMP

<a name='r'></a>
## R
__help:__ Dan Yoo, Logan

<a name='stata'></a>
## STATA
__help:__ Greg Larkin

__resources:__

[Resources for learning Stata - http://www.stata.com/links/resources1.html](http://www.stata.com/links/resources1.html)

[UCLA Tutorial on Stata - http://www.ats.ucla.edu/stat/stata/](http://www.ats.ucla.edu/stat/stata/)

<a name='running-stata'></a>
### Running STATA on Stanford Servers
These Stanford servers have stata installed
  1. bramble.stanford.edu
  2. hedge.stanford.edu
  3. elaine.stanford.edu
  4. myth.stanford.edu
  
You can start stata with the commands
```
stata
```
```
xstata
```
```
stata-se
```
```
xstata-se
```

_xstata_ is the graphical version, while plain _stata_ is the text version. _stata-se_ and _xstata-se_ are the special editions which allow you to have bigger data sets.

Here is a link to information on which Stanford servers to connect to for which task: [http://www.stanford.edu/services/unixcomputing/which.html](http://www.stanford.edu/services/unixcomputing/which.html)

<a name='spss'></a>
## SPSS
__Lab Copies:__

# Unix/Linux Primer

Most of the lab's computers use Unix (our Macs running OS X) or Linux (amygdala, dmthal, nacc, etc.). Linux and Unix are nice because they are stable, secure (if you know what you are doing), efficient, and play well with most of our lab software.

## Contents
  1. [About Linux/Unix](#about)
  2. [More Useful UNIX Commands](#more-useful-commands)
  3. [UNIX Commands Reference](#unix-commands)
  4. [X11 Tips and Tricks](#x11)
      1. [X11 Scroll Bar](#scroll-bar)
      2. [X11 Window Focus Follows Mouse](#window-focus-follows-mouse)
      
<a name='about'></a>
## About Linux/Unix
Commands in Linux take the form
```
$ command  –flag  –argument
```
Any given Linux command can take more than one flag and may take more than one argument. Flags allow you to set certain parameters under which you would like the command to operate. For example, the ls command lists the contents of your current directory and can take over a dozen flags that modify the way in which the directory contents are presented. So
```
$ ls –lr 
```
will list the files in the directory with extra information about file size, etc. (specified by the “l” flag) and will do so in reverse order (specified by the “r” flag), while
```
$ ls –t
```
will list the files in the order in which they were modified, and any combination (lrt, rtl, tlr, trl, etc)
```
$ ls –lrt
```
will do all of the above. As you can see, flags are listed all together after the first dash and may be entered in any order.

Arguments allow you to specify files on which the command should be operate, or files to which the command should send an output, or both. It varies by command.

Linux commands may strike you as confusing and counter-intuitive. That’s because they are. The shorthand names chosen for commands and flags are all different degrees of arbitrary (from the more intuitive “cp” for “copy,” to “grep,” which supposedly stands for “search globally for lines matching the regular expression, and print them” or in the ‘ed’ editor “g/re/p” but you really don’t care). And flags that specify one thing for one command usually mean something completely different for another command. To figure out how any given command can be used, it’s helpful to try one of the following. (A find-more-information command called ‘man’ that takes the command name in question as the argument, or through the use of a flag that indicates you want more information on the command given.)
```
$ man COMMANDNAME
```
or
```
$ COMMANDNAME -help
```
To find text whose location in the directory is unknown, you can utilize the grep command:
```
$ grep -l "text you want to find" *
```
where the star indicates you want to look in the current directory. Adding a –c flag (grep –c A1 XX.vec) will count the number of A trials in XX.vec. You could also type a directory name there, instead:
```
$ grep –l “text you want to find” ../data2
```

<a name='more-useful-commands'></a>
## More Useful UNIX Commands
To check disk usage (i.e., is the disk full?):
```
$ df -h *
```
To find a file:
```
$ locate searchstring dopa2PA
```
Or (from the root directory, you may be prompted to enter the root password):
```
$ sudo find / -name FILENAME
```
To set the path (i.e., the place the afni command will look for afni -- you need to be in the tcsh shell to do this):
```
$ setenv PATH {$PATH}:/abin    (or appropriate path name in place of /abin)
```
To set permissions... In the folder whose permissions you're setting, type
```
$ chmod 775 *
```
If not in the folder, type
```
$ chmod 775 foldername
```
For an individual executable (eg a script), you can set permissions with the chmod command. Each number in the three digit code sets permissions for different levels of user. "775," for example, sets R/W/Ex abilities to the user and group, but only 5 to all others.
```
$ chmod 775 filename
```
To check on # of trial types, go to each subject's folder and type the grep command, subbing in for D1 as need (this is the trial type argument)
```
$ grep -c D1 bb.vec
```
To determine which version you're running:
```
$ cat /proc/version
```
So, for example:
```
[span@mpfc /]$ cat /proc/version
```
will return
```
Linux version 2.4.20-19.9smp (bhcompile@stripples.devel.redhat.com)(gcc version 3.2.2 20030222 (Red Hat Linux 3.2.2-5))
#1 SMP Tue Jul 15 17:04:18 EDT 2003
```
```
$ is the BASH shell - to return to BASH, type 'bash'
% is the TCSH shell - to return to TCSH, type 'tcsh'
```

<a name='unix-commands'></a>
## UNIX Commands Reference
Here is a nice short reference on unix commands: [1](http://files.fosswire.com/2007/08/fwunixref.pdf)

Some more Unix/Linux Resources from Stanford Computing: [UNIX Documentation at Stanford http://www.stanford.edu/services/unix/](http://www.stanford.edu/services/unix/)

<a name='x11'></a>
## X11 Tips and Tricks
X11 is the x window terminal of choice for Mac OS X and should come preinstalled.

<a name='scroll-bar'></a>
### X11 Scroll Bar
X11 terminal windows by default have no scroll bar. You can open a new X11 window with the command
```
 > xterm -sl 5000 -sb &
```
-sl determines how many lines to keep and -sb adds a scroll bar.

Or you can create a new executable file to do this automatically:
```
 > emacs xtermscroll
```
Add the line
```
 xterm -sl 5000 -sb &
```
Make the file executable
```
 > chmod 755 xtermscroll
```
In the FInder, make the file open with X11.

Select the file, Command-I, or File > Get Info; and set _Open With:_ to X11.

Now you can double click on the new program to get a X11 window with a scroll bar.

<a name='window-focus-follows-mouse'></a>
### X11 Window Focus Follows Mouse
When using AFNI with X11 window forwarding, you might find it more efficient to set your mouse to focus on the screen it is rolled over.

You can set this in the command line by typing the command:
```
 > defaults write com.apple.x11 wm_ffm -bool true
```
You can turn this off with the command:
```
 > defaults write com.apple.x11 wm_ffm -bool false
```

# Moving and Saving Files

If you work on anything in the lab you’re going to be moving files between different computers.

There are five computers in the lab that run SFTP (Secure File Transfer Protocol – you want to use this!) server programs on top of the psych.stanford.edu server. That means that you can connect to these computers from any other computer to upload or download files. These five computers are

__Name	&	Room__

dmthal.stanford.edu 465 (the main Linux station)\
mpfc.stanford.edu	470\
nacc.stanford.edu	478\
mpfc2.stanford.edu	467\
insula.stanford.edu 465 (mac; knutson@)\
vta.stanford.edu 469 (mac; knutson@)\
dlpfc.stanford.edu	469 (mac)\
amygdala.stanford.edu 480 (Linux)

Note that none of these are windows PCs. You cannot connect to the lab Windows PCs (the laptops) from any other computer.

If you want to move a file onto a Windows PC you’ll need to put it on one of the SFTP servers, then get on that PC and download it with SecureFX since you will not be able to directly connect to the PC from any of the other computers.

Alternatively, you can email the file to yourself if it’s less than 12MB, or you can place it on the lab memory stick (see the lab coordinator) to transfer. You’ll need to do the same if you move files between the two Sony laptops. If you move files between the laptops you’re probably moving E-Prime files, in which case you should upload the file to the “tasks” folder on the psych server.

__From a Mac:__

  - Use MacSFTP or Fugu (icon located on the launch bar at the bottom of the screen)
  - Enter the address of the computer you’re connecting to under “host name” (e.g., “vta.stanford.edu” or “dmthal.stanford.edu”
  - Enter “span” for the login (or “knutson” if you are connecting to vta or insula)
  - Enter the password
  - Leave the Path field empty and click “connect”
  - You’ll get a window open to the default directory of whatever computer you connect to
  - Double-click the “..” folder to move up the directory tree
  
__From a Windows PC:__ Use SecureFX. Go to “quick connect” and connect to a server using SFTP, or use one of the saved connection profiles. This program is pretty user friendly (ignore license warnings); if you have any questions, ask the lab coordinator.

__From a Linux station:__

You can use sftp or scp on the command line.

Using sftp (to place or grab a file or folder onto/from another SFTP server … that is to say, another computer):
```
$ cd /directory1/directory2  		Move to the directory where the file is.
```
```
$ sftp username@servername 	e.g. span@vta.stanford.edu
```
(enter the password)
```
$ cd /directoryA/directoryB  	Move to the directory where you want to put the file or get the file from.
```
```
$ put filename  	Name of the file you’re uploading – use ‘mput’ for uploading a directory instead of ‘put.’
```
or
```
$ get filename  	Where ‘filename’ is the name of the file you’re downloading – use ‘mget’ for uploading a directory.
```
```
$ quit					The file/folder is now transferred.
```
Using the ‘scp’ command to upload a file to an SFTP server:
```
$ cd /data1/blah  			cd to where you want to keep the file.
```
```
$ scp span@vta.stanford.edu:/dir/folder/filename . 
```
/dir/folder/ is the directory on the server where the file is, “.” refers to the local destination (current dir).

or
```
$ scp –r span@vta.stanford.edu:/dir/folder/ . 
```
To download an entire directory.

__Where to save your files.__
```
AFNI files go on one of the Linux stations (dmthal, mpfc or nac, but not mpfc2)
Other data goes on insula in the /Users/Knutson/data folder
E-Prime files go to the “~span/public_html/Private/tasks” folder on the span@psych.stanford.edu server account, 
but individual subject’s behavioral data should go with their functional data to a linux server.
Other files (papers, graphics, et cetera) belong on insula or on the psych server; check with the lab coordinator if you’re unsure.
```

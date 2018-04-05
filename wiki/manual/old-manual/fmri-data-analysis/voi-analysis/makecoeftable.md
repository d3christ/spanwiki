# Makecoeftable.py

```
#!/usr/bin/env python
# Andrew Trujillo 11-12-09
# modified 03-19-10


#This script is for creating an organized table for the dumped coefficient data. For each voi specified in a coeff dump, you will have an output file in each participant's directory.    
This script will copy those files into a new directory (coeffdir=) in your scripts folder and create a table with all the coefficient data.  
# In order for this script to work:
#1. Your dump script must have created a file in each participants primary directory for each voi. Each file should have a row for each coefficient. 
#2. Make sure the dump files use the convention: subj_studystem_voi.coef (ex. cw_nfbk_nacc.coef) 
#3. Run this script from your scripts directory.


import sys
import os
import string
import glob
import shutil

#---User Control Variables
participants= ['nc'] #['ap', 'as', 'ba', 'bg', 'bh', 'bp', 'cd', 'ch', 'cn', 'dj', 'er', 'fl', 'jb', 'jj', 'js', 'kc', 'kk', 'ko', 
'lb', 'lg', 'ls', 'lu', 'mw', 'na', 'nh', 'nm', 'pn', 'rc', 'rf', 'rh', 'rm', 'rs', 'sc', 'sl', 'sw', 'sy', 'te', 'th']
coefficientNums=[4, 7, 13, 16] #must be in the order listed in the input files
vois=['linsu','rinsu', 'rmpfc', 'lmpfc', 'lrcau', 'rrcau', 'lmcau', 'rmcau', 'lnacc', 'rnacc', 'linsu2', 'rinsu2', 'ramyg', 'lamyg']
studystem='mil'
sub_coefdir='mil_coeff' # folder in subjects' main directory where coef files are
coeffdir='grpcoeff'#the directory you want your coeff table and copied coeffs to end up

#--Copy and Make Directories
def organize(participants, vois, studystem):
    if os.path.isdir('./'+coeffdir)==False:
	os.mkdir(coeffdir)
    elif os.path.isdir('./'+coeffdir)==True:
	shutil.rmtree('./'+coeffdir)
	os.mkdir(coeffdir)
    os.chdir(coeffdir)
    for x in vois:
	os.mkdir(x)	
    for y in participants:
	subDir=glob.glob('../../'+y+'*'/sub_coefdir)
	for z in vois: 
		shutil.copy(subDir[0]+'/'+y+'_'+studystem+'_'+z+'.coef', z+'/') 
	
    return 	
#---Create Header
def makeHeader(vois, coefficientNums):
    headerlst=['participant']
     for a in vois:
         for b in range(len(coefficientNums)):
             header=str(coefficientNums[b])
             header=a+header
             headerlst.append(header)
     return headerlst
#---Collect Data
def collectData(participants, vois, studystem, coefficientNums):
   coeffsout=[]
   coeffs=[]
   for y in range(len(participants)):
       coeffs=[]
       for x in vois:
           os.chdir(x)
           fid=open(participants[y]+"_"+studystem+"_"+x+".coef")
           file=fid.read()
           fid.close()
           coefflist=file.split('\n')
           if coefflist[(len(coefflist)-1)]==:
               coefflist=coefflist[:(len(coefflist)-1)]
    # Error flag if data file empty
	    if len(coefflist)==0 :
		print ("FILE: "+participants[y]+"_"+studystem+"_"+x+".coef, contains no data. Script Terminated!") 
		sys.exit(0)
        
            for z in range(len(coefficientNums)):
                coeffs.append(coefflist[z])
            os.chdir('../')    
        coeffs.insert(0, participants[y])
        coeffsout.insert(y, coeffs)
    return coeffsout
#---prepCSV
def prepCSV(headerlst, coeffsout):
    out=[]
    coeffsout.insert(0,headerlst)
    for i in range(len(coeffsout)):
        #print 'i'+ str(i)
        curString=
        for x in range(len(headerlst)):
            #print 'x'+str(x)
            curString =curString+coeffsout[i][x]+','
            #print curString
        out.insert(i, curString)
    outString='\n'.join(out)
    return outString

organize(participants, vois, studystem)        
head=makeHeader(vois, coefficientNums)
data=collectData(participants, vois, studystem, coefficientNums)
output=prepCSV(head, data)
outfid = open("coeftable_"+studystem+".csv", "w")
outfid.write(output)
outfid.close()  
os.chdir('../')    
print 'Table Created'
```

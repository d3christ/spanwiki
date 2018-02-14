# MSSD

MSSD is a calculation of noise similar to variance, however instead of variance from the mean, the variance is calculated from the previous time point.
I wrote a script to calculate MSSD in AFNI.

For those who missed Greg's discussion about MSSD: MSSD is calculated like variance, but instead of the average square of difference from the mean, it is the average square of differences from the previous time point.

The script can take some time to run, especially if you have 54 subjects, a 34 minute scan, and have resampled to teeny voxels.

This script is useful if you want to do a whole brain analysis of MSSD across subjects. DJY 20081022
```
<source lang="bash">
 #! /bin/csh

# created by djy 20081021 for FINRA

# this script will calcualte the MSSD for a block of data

#DEFINE VARIABLES HERE

set data = normfBIAS+tlrc # this is your data file
set output = bias_MSSD # this is the prefix of the output data

@ startTR = 0 # set this to the starting TR, usually 0 if you want to calculate MSSD from the very beginning
@ TRcount = 1010 # set this to the total TRs, if you want to model the entire block
@ endTR = $TRcount - 1 #set this to the TR you want to end on.


# The real meat starts here.

@ TRsum = $endTR - $startTR

@ currentTR = $startTR + 1 # set the current TR for the loop to the start TR + 1.
@ previousTR = $currentTR - 1

echo $currentTR # print which TR we are starting with

3dcalc -float -a $data'['$previousTR']' -b $data'['$currentTR']' -expr "(b-a) * (b-a)" -prefix 'tempsum_'$output 

3dcalc -float -a 'tempsum_'$output+tlrc -expr 'a' -prefix 'prevtotalsum_'$output

rm -f tempsum_${output}*

@ currentTR = $currentTR + 1

while ( $currentTR < $endTR + 1)
	echo $currentTR
	@ previousTR = $currentTR - 1
	3dcalc -float -a $data'['${previousTR}']' -b $data'['$currentTR']' -expr '(b-a) * (b-a)' -prefix 'tempsum_'${output}
	3dcalc -float -a 'tempsum_'$output'+tlrc' -b 'prevtotalsum_'$output'+tlrc' -expr 'a+b' -prefix 'newtotalsum_'${output}
	rm -f tempsum_${output}*
	rm -f prevtotalsum_${output}*
	3dcalc -float -a 'newtotalsum_'$output'+tlrc' -expr 'a' -prefix 'prevtotalsum_'$output
	rm -f newtotalsum_${output}*
	@ currentTR = $currentTR + 1
end

echo "total TR's for average = "$TRsum

3dcalc -float -a 'prevtotalsum_'$output'+tlrc' -expr 'a/('$TRsum')' -prefix $output

rm -f prevtotalsum_${output}*
</source>
```

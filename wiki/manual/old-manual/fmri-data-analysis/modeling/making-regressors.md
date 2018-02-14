# Making Regressors

## Contents
  1. [Quick reference](#quick-reference)
  2. [Overview](#overview)
  3. [Master Matrix](#master-matrix)
      1. [Replacing text](#replacing-text)
  4. [Making regressors](#making)
  5. [Adding ratings](#adding-ratings)
  6. [Vector description syntax](#vector-description-syntax)
  7. [txt2master and lookup tables](#txt2master)
  
<a name='quick-reference'></a>
## Quick Reference
Here are some useful downloads:
  - Download [a set of examples with e-prime files]() (Best way to get started)
  - Download [txt2matrix.py]()
    - You will have to edit the section at the top of this script
  - Download [example text replace script]()
    - this is useful for changing word codes to number codes
  - Download [makeVec.py]()
    - Do not edit this script
  - Download [example vector description file]()
    - You will need to rewrite this to fit your study
    
It is convenient to run these scripts from your regression script. For an example of this see the [GLM](glm.md) section.

<a name='overview'></a>
## Overview
  - Start with an eprime .txt file
  - Make a large matrix that records all of the relevant task events and behavior by TR
  - Add any extra data to the master matrix such as preference ratings taken after the scan.
  - Filter out the events that you want to make regressors out of using the makeVec.py script and a customized vector description file.

If you'd like to play with an example dataset to test out the scripts go to the /comonscripts/txt2matrix_example/ directory on dmthal or download it from [here](). Run the cleanAll script to get rid of leftover data to start fresh and open up the exampleInstructions file.

<a name='master-matrix'></a>
## Master Matrix
To set up the fields you want copy "txt2matrix.py" to your study directory and make the appropriate changes to the "USER EDIT" section.

here is an example of the "USER EDIT" section: 
```
<source lang="python">
  1. **********************USERS EDIT THIS****************************
  2. these names must match fields in the e-prime txt file.
FieldsOfIntrest = ["prodname", "price", "choose.RT", "ChooseProd", "condition"]
  1. For non-imaging data set TRsPerTrial to 1 and DynamicTRNumber to False.
TRsPerTrial = 6 #set to 1 for behavior only
  1. set DynamicTRNumber = True if there are different numbers of TRs on different
  2. trials, False otherwise.
DynamicTRNumber = True #set to false for behavior only
  1. VARIABLES BELOW ONLY FOR DYNAMIC TR NUMBER
  2. Use only if DynamicTRNumber = True
VariableDelayLengthField = "ITI" #name of the field that codes the iti length RemainderOfTrialTRs = 6 #this is the number of TRs not including the ITI TRLength = 2000
  1. set filterTrials = True if you only want to include certain kinds of trials
  2. (This is useful if you are running a few different kinds of blocks and you only
want output from only one.)
filterTrials = False
  1. VARIABLES BELOW ONLY FOR FILTERING TRIALS
trialAncor = 'Procedure' #this is the field you want to look for trialAncorVal = 'None' #this is the value you want the field to be
  1. you can set this to output a particular name. Or just use "Default" to create
  2. the name automatically from the input.
outputName = 'Default'
  1. **********************END USER SECTION****************************
</source>
```
To run the example txt2matrix script use:
```
./txt2matrix.py EndowRun
```
You will end up with a file called: EndowRunMATRIX.csv This is a csv file with a column for every field you listed in "FieldsOfIntrest" (plus a column for the trialnumber and TR number) and a row for every TR in the scan.

In this example both of the blocks were appended together. If you want to run it for one block use:
```
./txt2matrix.py EndowRun1
```

<a name='replacing-text'></a>
### Replacing text
Sometimes it is useful to replace certain words in your matrix files with number codes (for example, for easy import into matlab) or other words (for example, to keep all subject files the same). It is easy to make a python script that will do this for you. endowReplace.py is an example of a script that will convert the products and trial types into number codes.

Here is an example of running it:
```
./endowReplace.py EndowRunMATRIX.csv EndowRunMATRIX_numbers.csv
```
This will use the EndowRunMATRIX.csv file as input and create a new file called EndowRunMATRIX_numbers.csv with all your specified replacements.

<a name='making'></a>
## Making regressors
After you have the master vector you can make regressors using "makeVec.py" and a vector description file. You do not need to make any changes to the "makeVec.py" script or even copy it, but you will need to make your own vector description files. There are a few files in /commonscripts/txt2matrix/ on dmthal that you can use as examples:
  - perVecs.txt - this describes 3 period regressors
  - behVecs.txt - this description file results in regressors that are individualized by subject responses.
  - comboVecs.txt - this gives examples of combining info from different fields and using '>' and '<' signs.
  
here is an example of running make vec to get vector files:
```
makeVec.py perVecs.txt
```
(Note: if you are on dmthal you do not need to use "./makeVec.py" because the script is already on the path)

<a name='adding-ratings'></a>
## Adding ratings
You may have extra information that is not part of the e-prime task that you would like to include in the master matrix in order to build regressors. A common example of this is subjective ratings (say of valence and arousal for example). You can create a special vector description file to do this. buildVecs.txt is an example of this. It uses the file "ratings.csv" as well.

This is what buildVecs.txt looks like:
```
BEGIN_VEC
INPUT: "EndowRunMATRIX.csv"
BUILD_FROM: "ratings.csv"

MARK prodname WITH preference

END_VEC
```
This will look up the "prodname" and "preference" columns in ratings.csv and make a new column in EndowRunMATRIX.csv called "preference" that matches the preference rating to each product. Note that the "prodname" column must exist (spelled the same way) in both the EndowRunMATRIX.csv file and the ratings.csv file.

To run this just use makeVec.py in the same way as before:
```
makeVec.py buildVec.txt
```
Now there will be an extra column in your matrix file called preference with the ratings from this subject. You can build other regressors based on this using the method described above.

<a name='vector-description-syntax'></a>
## Vector description syntax
Vector description files are not written in any particular computer language. You can not use any python or shell script commands or syntax in these files. Instead they are just written in a simple "vector description language" that stephanie greer made up. The makeVec.py script, written in python, knows how to read this made up language. This language has a few simple key words and syntax rules described below.

Example of making a Buy vs. Sell regressor in the product period (i.e. 1st and 2nd TRs):
```
BEGIN_VEC
INPUT: "EndowRunMATRIX.csv"
OUTPUT: "SellvsBuy_prodPer.1D"

MARK condition = SELL_? AND
     TR = 1,2 WITH 1
MARK condition = BUY_? AND
     TR = 1,2 WITH -1

END_VEC
```
Example of making a reaction time regressor in the choice period (i.e. 5th and 6th TRs):
```
BEGIN_VEC
INPUT: "EndowRunMATRIX.csv"
OUTPUT: "RT_choicePer.1D"

MARK TR = 5, 6 WITH $Decision.RT

END_VEC
```
General form of a vector description:
```
BEGIN_VEC
INPUT: "{inputfile}"
OUTPUT: "{outputfile}"

MARK {expression} AND
     {expression} AND 
      ... WITH {code}

MARK {expression} WITH {code}

END_VEC
```
BEGIN_VEC and END_VEC mark the beginning and end of a *single* vector description. Anything outside of these tags will not be read by makVec.py. You may include as many vector descriptions in a file as you like.

INPUT: - after this keyword, enter the input matrix file in quotation marks.

OUTPUT: - after this keyword, enter the output vector name in quotation marks.

  - Note: Instead of OUTPUT you can also build your matrix file using these key words (download the example set for more info):
    - APPEND_TO: - takes a matrix file (usually the same as input) to append a column onto.
    - BUILD_FROM: - takes a different file to build input from
    
MARK - this keyword starts the description. for every MARK keyword there must be one WITH keyword.

AND - specifies the start of a second expression. you can have as many of these as you like between the MARK and WITH tags.

WITH - indicates what code to mark the conditions with.

{expression}: must be of the form:
[column title] = [column value]\
or\
[column title] > [column value]\
or\
[column title] < [column value]

{code}: must be of the form:\
[static value]\
or\
$[column title]

<a name='txt2master'></a>
## txt2master and lookup tables
You can find information on a second way of making regressors using the txt2master script and lookup tables in [Data analysis appendix D](data-analysis-appendices.md/#appendix-d). This method has been replaced with the method described above.

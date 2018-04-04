# Stata

## Contents
  1. [Appending blocks](#appending-blocks)
  2. [Cleanup](#cleanup)
  3. [Logistic regression](#logistic-regression)
  4. [Summary Stats](#summary-stats)
  5. [Other](#other)
  
We use a program called Stata to do our regressions. Stata is run from terminal and lives on Elaine, a Stanford server (elaine.stanford.edu). You should be able to access Elaine using your Stanford ID (everyone who is a full time employee is given a Stanford email account – your account will be your SUNet ID @ stanford.edu and your login to the server will be your SUNet ID logged on to elaine.stanford.edu.

Open X11, log on to Elaine, and move over your super-clean data if you haven’t already:
```
$ ssh –Y jpegg@elaine
```
Open Stata:
```
$ xstata –se
```
Commands in Stata will henceforth be designated by “S]” – type your commands in the large white box on the lower RH side.

Before you can complete the Stata stage of analysis you’ll need to make sure you have all the files you need in the correct spot. Two Stata files ending in “.do” should be placed in the same folder as your data and the scoreEPrimeScanner java script. They’re called “

<a name='appending-blocks'></a>
## Appending blocks
The first thing you’ll do in Stata is append your blocks if your subject was run on more than one block of BIAS. Stata has a special command for this called ‘append using.’

When you use the command 'append using filename.dta' there are a couple of things to note.
  1. It appends the filename.dta file to the bottom of the data you currently have open
  2. The file needs to be a STATA .dta file, otherwise it will give you an error. The file onto which you’re appending need not be .dta, however – it can be .txt, too.
  
So if appending two BIAS runs (here for hypothetical subject 404), you would complete the following process.

Load up your first block:
```
S] insheet using 404tab_B2.txt
```
The insheet loads the file into Stat’s conscious. You’re doing this so you can save it as a .dta file; .dta is the Stata format. Otherwise it won’t let you append it later.
```
S] save "whateveryourfilepathis/404tab_B2.dta"
```
Now you have to clear the buffer before reading in another file, so do this with the clear command.
```
S] clear
```
Load up block 1:
```
S] insheet using 404tab_B1.txt
```
Now append block 2 (don’t worry about saving your block 1 .txt file as a .dta file):
```
S] append using 404tab_B2.dta
```
But do save the appended file (block 1 + block 2) as a .dta file:
```
S] save "whateveryourfilepathis/404tab_B12.dta"
```
That will give you a set of data with the first run first, and the second run following, cut and pasted together.

If at any point you want to look at your data in Stata, you can type:
```
S] browse
```
Keep in mind, however, that you’ll need to close the browser window to execute any commands or to save.

<a name='cleanup'></a>
## Cleanup
Now you’re almost ready to run your logistic regression. First, however, you’ll need to ‘clean up’ your data a third and final time. To do this you’ll need to use what’s called a .do file. These files are Stata programs that execute specific tasks related to statistical analysis. We have a file called cleanup.do to get things in shape for the statistical analysis. In particular, it creates new “m” variables (new “vars”). Your “m” variables correspond to the types of errors subjects can make.
```
M0 = rational
M1 = risk-seeking
M2 = risk averse
M3 = confusion (wrong stock)
```
In Stata, go to
```
File menu → Open → filetype: do → cleanup
```
Then press the “do” button. It’s in the horizontal button bar up top and isn’t intuitive or unique looking. Just mouse-over till you find it.

<a name='logistic-regression'></a>
## Logistic regression
Now your data is ready for the regression.

In Stata, go to
```
File menu → Open → filetype: do → logits 
```
Then press the “do” button.

These regressions help pick out the areas of the brain that predict risk-taking and loss aversion. [Is the brain analysis really here or later? Where do we load in the brain data?]()

If there are vars you don’t want, use drop command, then proceed with the logits:
```
S] Example of drop command?
```
```
File menu → Open → filetype: do → logits
```
Then press the “do” button.

<a name='summary-stats'></a>
## Summary stats
For summary stats type:
```
S] collapse m0 m1 m2 m3
```
What does this do?
```
S] collapse (sum)m0 (sum)m1 (sum)m2 (sum)m3 (sum) madeMistake (sum)iswrongevalstock, by (subject session)
```
To export your results to Excel go to
```
File menu → export (Excel) → click “ok”
```
And name your exported file something descriptive, like "404sess_collapse.txt"

Now you’re ready to look at your behavioral results!

[Note on using this to analyze brain data.]()

One of the most interesting things to look at with BIAS data is, of course, the relationship between brain activation and the types of choices people make. Because different people make different decisions in each market (that is, each set of 10 trials), each individual will need to have personalized regressors that label not only anticipation and outcome, but also types of choices (risk-seeking mistakes, risk-averse mistakes, errors, etc).

With these master vectors in hand, we can then proceed as usual with 3dttests and logistic regressions on the brain data.

<a name='other'></a>
## Other
To log commands: log using temp.txt

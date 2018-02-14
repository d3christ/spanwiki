# Background

## Contents
  1. [Regression (Reg) Scripts](#reg-scripts)
  2. [Waver](#waver)
  3. [3dDeconvolve: Regression](#3ddeconvolve-reg)
  4. [3dDeconvolve](#3ddeconvolve)
  5. [Onward](#onward)
  6. [Another Math Moment](#another-math-moment)
  7. [The non-math explanation](#non-math-explanation)
  8. [How do I know if my regressors are “collinear enough” for bad things to happen?](#how-do-i-know)
  9. [3dMerge](#3dmerge)
  
<a name='reg-scripts'></a>
## Regression (Reg) Scripts
There are many variants of the reg script – usually one final version for each experiment, and many others along they way – but they all do essentially the same thing. The script compares your actual data to an ideal data set (where the ideal data set is a fake set that mimics what you would expect your data to look like if your hypothesis is correct for any given brain area), providing you with a statistical output that tells you how closely the two sets (real and imagined) match. It then gives you coefficients and z-scores for each voxel for every given contrast. 

Reg also convolves an idealized HRF (hemodynamic response function) to the step function .1D regressors, producing a convolved xxc.1D file.

__Convolution__ is a mathematical operator which takes two functions f and g and produces a third function that in a sense represents the amount of overlap between f and a reversed and translated version of g. In English, this means that we’re taking the step function (b) and combining it with what a typical HRF looks like. The math behind this step involves calculus and let’s be honest, even Newton probably didn’t really understand all of that. But the gist is that we’re essentially smoothing out the step function and making it look more like an HRF.

<a name='waver'></a>
## Waver
The waver command follows each call to an m2v. AFNI’s waver command convolves your HRF with each lookup table to create that combined function that looks more like what we’d expect to see in a real HRF in the brain. Waver is found in any model script where you use 3dDeconvolve. In addition to convolving your HRF with each lookup table, waver pushes forward your regressors so that they match up with the HRF lagged data. This convolution (the “c” in xxc.1D) lags the expected response by the hemodynamic lag of 4-6 seconds. That means that your output will be aligned to real-time stimulus onset.

We do a 6 second lag because we use the gamma HRF function (this is the -GAM flag that's inside waver). This HRF peaks earlier than some other default HRFs used in fMRI research; choice of an HRF function is a matter of personal preference. Your convolved file is output in the format xxc.1D for use later in the script.

__Note: Dealing with HRF Lag.__ When you view timecourse data, most will begin at the beginning of the trial. The timecourse should therefore start at a baseline, then deviate; remember that due to the HRF lag you'll want to look 2 or 3 TRs (4-6 sec) ahead of each event onset of interest to see what's happening in response to the event. While most researchers publish lagged timecourse data, some may chop things up, pulling back the BOLD response to correspond graphically to the stimulus onset. Read carefully.

<a name='3ddeconvolve-reg'></a>
## 3dDeconvolve: Regression
When we want to know how much of the variance in one variable is accounted for by another variable, we perform a regression. If, for example, I wanted to know how much I could know about someone’s height based on their weight, I would regress height on weight. Theoretically, there would be a strong relationship between weight and height and we could say that weight is a good (or strong) predictor of someone’s height. In mathematical terms, the regression equation is just like the equation for a line:

![Reg1.jpg]()

Where y is the variable that we’re trying to predict (in this case, ‘height’), alpha is the intercept, beta is the mathematical weight of the x (predictor variable, which in this case is ‘weight’), and some error term that represent unique variability for each case. So, if I wanted to try to predict Brian’s height, I would take some intercept that is the average height for all men (5’7”), add to that his weight (170lbs) multiplied by the beta of weight (let’s say ‘.8), and then add to that some unique term for Brian, and in the end I’d come up with his height: 5’10”.

We’re going to run a regression on our data using the command 3dDeconvolve. Our predictor variable (the variable we’re trying to predict) will be our activation in each voxel in the brain (the model, with all its regressors, is run on each voxel). Our predictor variables will be our regressors, or contrasts of interest. (See the txt2master and lookup table sections for examples of contrast regressors.)

<a name='3ddeconvolve'></a>
## 3dDeconcolve
Now that you have the ideal response to each stimulus coded in xxc.1D files, the reg script calls the AFNI program 3dDeconvolve. 3dDeconvolve does regressions; that is, it compares your actual data to what you would expect in a perfect world. [The perfect world is here represented by the idealized HRF convolved with the step function that was created by waver as a xxc.1D file.] In more concrete terms: 3dDeconvolve compares the fake hypothesis timecourse you’ve created with the waver command to your actual timecourse in each voxel of the brain. So for each voxel you’re comparing its real time series (or timecourse – a graph that plots its up and down movements over the course of the entire task) with the hypothesized time series to see how closely they match.

There are a lot of voxels in the brain, and it has to do this process individually for each, so the script tends to take a while on this step. Here, instead of dealing with height and weight, our predictor variables are the idealized sets of fake data that have been created with waver and the dependent variable is the actual data (scalar values) that we have in each voxel at each TR.

The better each regressor predicts your actual data for a given voxel, the stronger the beta-weighting coefficient will be for a given regressor. The script will give you a coefficient and t-statistic across the whole brain (that is, for each voxel) for each regressor (also known as a contrast) of interest.

This is what your regression looks like mathematically for multiple regressors. Note that it’s very similar to your equation for only one regressor.

![Reg2.jpg]()

Because Y and each X are actually column vectors, the equation can be rewritten as:

![Reg3.jpg]()

Where we’ve set epsilon, a term that contains our Y-intercept value plus an error value or some other kind of experimental fudge factor, equal to zero.

Here, each value of Y corresponds to the activation (in normalized units from your scanner output) of a single voxel during a single TR. So Y-sub-1 is the activation of that one single voxel during the first TR, Y-sub-2 is the activation of that same single voxel during the second TR, and so on. The “k” variable denotes your total number of TRs, so your last Y-sub-k is simply the value of that one single voxel during the last TR.

Each X column vector corresponds to an individual regressor. The first subscript entry denotes which regressor it is (here we’ve just labeled them 1 through N, but each of those numbers in reality would correspond with a different regressor). The second subscript entry, k, once again denotes the TR, with the first TR at the top and the last TR at the bottom.

Here, to simplify matters, all our values are 1, -1, or 0 (this is the step function given to you by your lookup tables … in your reg script you go a step further and convolve that step function with an HRF, so you’re real-life regressor column vectors will have values that are usually not integers like 1 – these column vectors are the xxc.1D files created by the reg script). For example, if the system of equations above represented the MID task our first column vector might be our reward anticipation contrast. The numeral 1 is our first entry because our very first TR in the task is a reward cue – that is, a reward anticipation period.

Just like the Y-column vector, each new row in all of the X-regressor vectors is a sequential TR (the first entry is TR 1, the second entry is TR 2, et cetera). An X column vector’s value at each TR is the value of your ‘hypothesis’ curve for that contrast at that TR – usually a non-integer value because you’ve convolved your step function with an HRF, but remember here we’ve left out that step for simplicity’s sake and we’re just looking at our regressor before it’s been convolved with the HRF.

Associated with each regressor column vector is a coefficient known as the beta coefficient. We can solve this system of equations for the beta coefficient, which will in effect tell us how much each column vector regressor is contributing the values of your Y column vector. If the beta coefficient is small, that regressor does not contribute much. If it is large, it indicates that that regressor is responsible for a non-significant amount of the variance you’re seeing in Y, your voxel activation at each TR timepoint k.

Note that there are certain circumstances in which you will have multiple solutions for the beta values. In fact, there are circumstances in which you could have an infinite number of beta value solutions. This circumstance is called collinearity. In the strictest mathematical sense it means that at least one of your vertical vectors (regressors) is a linear combination of at least two other regressors. That is, if you add those other two columns together you get a third column ... a collinear regressor could be a linear (additive) combination of any number of other regressors, however – it needn’t be just two.

Here's an example of collinear regressors. Our example consists of three regressors (regressor 1, regressor 2, and regressor 3) for an experiment only 3 TRs long (the top entry is TR 1, the middle entry is TR 2, and the last entry is TR 3).

![Reg4.jpg]()

We can re-write this in matrix format and, just for kicks, give our Y entries real values for different time points, just like we'd find if this were real data in a single voxel in the brain:

![Reg5.jpg]()

Here it's clear that column three, our third regressor, is a linear combination of column regressors one and two because by adding one and two together we get column three.

It's also clear that this is not a good state of affairs for our attempts to solve this system for the values of our beta coefficients. We'll find when we try to solve that there are in fact an infinite number of values the beta coefficients could take on and still satisfy the system of equations.

![Reg6.jpg]()

Note above how you can assign beta3 any value you want and still solve the system of equations for beta1 and beta2. The beta values are not in any way constrained; there is no unique answer for this system of equations. Were we running these regressors in our analysis, the computer would choose just one solution where many are possible.

With real data in this sort of collinear situation, we wouldn't know for sure how much each regressor is contributing to the variance in the dependent variable (brain activation).

Now, let's review what we know about the regression equation, then take a look at the above situation in regressors that are generalized to our much longer experiments (as you'll recall, the example regressor above would only be a 3 TR experiment!). We'll also look at reasons why we might get something resembling collinearity in our data when our regressors are not actually collinear in the strictest algebraic sense. In statistics the term 'collinearity' is usually used in a much looser manner than its well-defined pure math counterpart. Collinearity of any flavor can mean bad things for your data.

First, a quick review. The regression equation can be written as follows

![Reg7.jpg]()

Also recall that regression equations will often have a scalar correction term (epsilon) tacked onto the end; for the purposes of our example we'll set ours equal to zero.

![Reg8.jpg]()

<a name='onward'></a>
## Onward
For the sake of the example let's say that this is a 4-regressor experiment with k total TRs and 4 TRs per trial with only one trial type. Now, if we make a column vector out of each of our regressors (as we do with our .1D contrast files), we can write the above equation as:

![Reg8.jpg]()

Note that each entry in the column vectors represents a different time point, starting with TR 1 at the top, then TR 2, TR 3, and so on. The Y values represent the activation of a single voxel at each of those TR time points (Y1 is the activation of that one voxel at TR 1, Y2 is the activation of that same voxel at TR 2, et cetera). The beta coefficient is a scalar quantity -- that is to say, it is a single number, not a vector.

The equation above can be re-written in matrix format (factor your betas out into a column vector and combine your regressor vectors into a matrix):

![Reg9.jpg]()

Here we see that the fourth regressor is collinear with the 1st and 3rd regressors, since with only 4 TRs per trial and one type of trial, each vector will repeat its first four entries for the entirety of the experiment.

However, in practice your regressors need not be strictly collinear to manifest what appears to be collinear behavior in your analysis. There are at least three reasons for this.
  1. Computers do not work with infinite precision. For example, (1/3) * 3 equals one, but when a computer program calculates that amount it will terminate decimal places and arrive at the answer 0.999999999. This means that the computer could potentially make regressors that are not strictly collinear appear approximately collinear, with repercussions in your maps.
  2. Study design can force collinearity on your data. For example, if you were studying the effects of a number of independent variables on perceived pleasantness of music, but used sound files from real concerts whose volumes tended to scale with music type (eg, quiet ballads, medium country and ear-splitting death metal), you would have introduced collinearity in your design between your volume and music type regressors.
  3. Correlation serious enough to matter between predictor (independent) variables or contrasts of interest can cause collinear results. Some psychologists and neuroscientists will call any such correlation collinearity, others will not. It may not be obvious that it exists when you design your study and run your regressors, but it will hopefully be obvious in your output, which may have large standard errors, unexpected signs, and which may not show effects that have been robustly proven in past experiments.
  
In our own data it would be easy to create collinear regressors. For example, imagine that we have a regressor 'rout' (we want to see areas of the brain with more activation for a rewarding outcome than for anything else). However, we also want to look at hvlout (areas of the brain coding for high versus low reward outcomes). These two regressors are highly correlated since receiving *any* reward is correlated with receiving a high reward.

The take home message is that the word “collinearity” as used in fMRI and general statistics for psychology is NOT the same as the stricter definition used in mathematics. It IS, however, something you need to be very careful of when designing an experiment and when making your trial types and lookup tables.

__NOTE:__ Truly orthogonal regressor vectors will have a dot product equal to 0. Fully orthogonalizing your regressors means that the dot product should be zero between all of them; you’ll note that sometimes this requires using step function values in your lookup tables other than 1, -1, and 0.

<a name='another-math-moment'></a>
## Another Math Moment
For those of you unfamiliar with dot products, a dot product is simply a rule for taking the product of two or more vectors. If a and b are our vectors, their dot product is a scalar value that’s computed by taking the sum of the products of each row in a with the corresponding row in b:
```
a · b = a1b1 + a2b2 + a3b3 + … + aNbN
```
You can think of correlations as existing in vector space, with each vector representing a regressor. It’s difficult for us to think of our regressors in this space, however, because with N timepoints we have an N-dimensional vector space that we can’t represent graphically.

If you do some derivations using the Pythagorean Theorem and the definition of terms like cosine, you discover that the absolute value of A times the cosine of theta is the projection (a scalar value) of A on vector B. It’s obvious from the diagram above that if A and B are at a 90 degree angle to one another that there is no element of A that’s shared with B (eg, no relationship between the two), because if we were to arbitrarily say that B lies along the X-axis A would not have an X component, only a Y component, and B would have only the X component and no Y component. That is to say, you cannot predict anything about B based on what you know about A. The two vectors are orthogonal, and therefore their dot product is zero since cos 90º is 0. If the dot product is not zero, however, it means we have some degree of collinearity, which could be bad for our data.

<a name='non-math-explanation'></a>
## The non-math explanation
Practically speaking, collinearity means that some of your independent variables are not only related to one another, but that they are partially or fully predicted by one another (for example, in our previous example receiving a high reward overlaps in large part with receiving any reward). Sometimes such overlap is obvious and sometimes it is not. This state of affairs is problematic because the regression equation can no longer determine how much one independent variable uniquely affects the dependent variable. The scary consequence of collinearity is that the estimates of how much each independent variable affects the dependent variables will be unreliable. If there are two regressors you really want to run that are collinear, you will need to split them up and run them in separate models. (Meaning you’ll start back toward the beginning with a separate reg script for each.)

<a name='how-do-i-know'></a>
## How do I know if my regressors are “collinear enough” for bad things to happen?
Collinearity will most often manifest itself as wildly significant scores in either direction. The output of 3dDeconvolve will also tell you you’ve got it, but this doesn’t always mean anything bad for your data (it certainly means you should check, however).

The best way to check is to take a look at your contrasts. Here’s an example discovered during supplemental analysis for Knutson and Wimmer 2007.

Figure A. Illustration of two collinear regressors in the MID task modeling outcomes. Here, gain outcome is generated by the AD model, which calculates outcomes as the outcome value minus the expected value (cue*.66 win probability). The TD model includes a reward regressor that is positive and the same for all rewards.

Figure B. The effects of collinear regressors on regression output. Putamen and SMA activity are obviously flipped between the two regressors, a strong indication of model specification problems.

Collinearity that is not at all complete (and not flagged as a problem by AFNI when running the model), as shown in the first figure, results in very obvious problems in the regression output. These maps are largely inverses of one another (excluding the MPFC). Such a model should not be relied upon. Indeed, in the published report of this work (Knutson and Wimmer, 2007), including both regressors in the same model (a strong test of best fit) was rejected because of collinearity. Instead, two separate models were specified, one with the AD gain outcome regressor modeling the outcome period, and the other with the TD reward outcome regressor modeling the outcome period. A paired t-test comparison (3dttest –paired) showed that the AD gain outcome regressor indeed was a better fit for MPFC activity than the TD r(t) regressor.

<a name='3dmerge'></a>
## 3dMerge
After 3dDeconvolve, the reg script calls 3dmerge, which converts all t-scores to z-scores. A z-score is a way of standardizing some sort of measure or statistic; it tells us how many standard deviations away from the mean our data is (a z-score of 3 means 3 standard deviations). Importantly, it allows us to compare a given score on some variable to the mean score on that variable. In terms of brain imaging, a z-score lets us know how much greater (or lesser) than baseline a given region of the brain is showing activation. The normal distribution is important here:

So let’s say we’re testing the dorkiness of graduate students and we’re wondering if Hal is as dorky as the rest of the Stanford graduate population. Let’s also say that the average graduate student at Stanford has a dorkiness score of 100 and there is a standard deviation of 20. And, for the sake of this example, let’s pretend that Hal’s score is 50 (less dorky than average).

The question, then, is the following:

Is Hal’s score sufficiently lower than the mean to assume that he comes from a population of non-dorky graduate students (where would we find such people?).

The null hypothesis is that Hal’s score does come from the dorky population. So then, let’s calculate the probability that a score as low as Hal’s does actually come from this population. If the probability is very low, we can reject the null hypothesis and conclude that he comes from a cooler, non-helmet wearing graduate student population. On the other hand, if the probability is not very low, then we would have no reason to doubt that Hal actually comes from the dorky population.

To do this, all we have to do is calculate a z score and then refer to a z-score table.
```
Z = X - μ / σ =
50 – 100 / 20 = -50 / 20 = -2.5
```
From a table of z scores, we see that the probability of a z score of –2.5 is .0062. This is our p value = .0062. Because this number is so low (much lower than our standard cutoff of p = .05), we can say that Hal comes from a different population of graduate students (perhaps he’s actually an undergrad).

The preceding logic is very important and is at play in t-tests, which are very similar to z-scores, except that t-tests take into account the size of a sample, whereas z-scores do not. When a sample gets to be ‘large’ (i.e., 120 people or measurements), t-values become equivalent to z-values.

After completing 3dmerge, the reg script writes out your new z-file to the subject’s directory and the script ends. You will use this z-file later to grab coefficient tags for your 3dttest script.

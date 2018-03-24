# Logistic Regression 

## Stata notes
Stata is a statistical analysis program that runs on Stanford servers (e.g. Elaine). The lab primarily uses Stata to run logistic regressions, using neural activity to predict binary behavior like purchasing decisions or gamble choices. The interface is graphical, but has a convenient command-line option and easy to specify fixed-effects that make it better than SPSS. Stata is not free, however, and has not been installed on SPANlab servers. Thus lab users transfer data files to their space on a server and run Stata by ssh connection \[*note: Stata should probably be pronounced like the beginning of sta-tistics]

File types: easiest to use are comma-separated (.csv) files, because Excel readily saves in this mode (ignore it’s annoying warnings) and more importantly because it is also easy to turn timecourse data into .csv files used in logits (via $ paste –d , column1.tc column2.tc.. > output.csv). Note that once a .csv file has been imported for use, Stata requires a restart to accept another import. Stata natively uses .dta files, and imported files can saved in this format for easier loading.

File structure: for logistic regressions, typical spreadsheets used in the lab have a row for each decision and columns for the various behavioral and neural variables taken from that trial (e.g. nacc activity during product presentation, product preference, price, reaction time, and choice, during trials 32 / for trial 32. Files also will have a column for subject number (best to use scanner number) and choice number.

__logit__ Computes a logistic regression, using predictor variables(s) to estimate a binary outcome variable, e.g. logit purchase nacc will predict purchase decisions using nacc activity.

Fairly decent help can be found for this command (and others) [here](https://www.stata.com/help.cgi?logit).

__logistic__ Computes a logistic regression similar to the logit command, but only gives odds-ratios instead of t/z-scores.

__regress__	Computes a traditional regression, predicting a scalar quantity. An example would be using preference ratings to predict product-evoked nacc activity.

__pwcorr__	Computes a simple pair-wise correlation between two input variables. Using “pwcorr x y, sig” will additionally give you the significance of the correlation.

__xi: .. i.subj__	Controls for fixed effects. Using “xi:” and “i.subj” (where “subj” is a column that numerically codes subjects) as bookends to any command (like regress and logit) will control for subject fixed-effects. This has been used in all SPANlab logit analyses to date.

__e.g. $ xi:__ logit purchase nacc mpfc insula i.subj

__estat ic__	Computes Akaike and Bayesian information criterion (AIC, BIC) for the model just run. These are measures of model strength that are better than using simple pseudo-r2 because they look at fit and additionally take a hit from extra predictor variables.

__predict res, r__	Computes the residuals of the model just run. Initially used in the lab for covarying out motion and using residualized brain activation in logits. Puts the residuals in a new column in the input dataset. Note “res” is just the output column heading; it could be named anything.

Stata can do a lot more, but we have had no need to learn how to do ANOVAs there, for example, because SPSS works fine for small tasks on small datasets.

#### Brief note on mediation:

“In psychology, a mediation model is one that seeks to identify and explicate the mechanism that underlies an observed relationship between an independent variable and a dependent variable via the inclusion of a third explanatory variable, known as a mediator variable. Rather than hypothesizing a direct causal relationship between the independent variable and the dependent variable, a mediational model hypothesizes that the independent variable causes the mediator variable, which in turn causes the dependent variable.” (Wikipedia)

Mediation effects are simple to determine when you are already running logistic regressions. If the outcome variable is binary, logistic regressions can be used for the paths between the predictor and mediator and the outcome, while the “regress” regression command can be used from predictor to mediator, if the mediator is scalar. Then simply plug in the resulting coefficients and standard errors for paths a and b above into a mediation calculation.

These pages can be used to compute mediation significance: [1](http://psych.ku.edu/preacher/sobel/sobel.htm) or [2](https://www.danielsoper.com/statcalc/calc31.aspx)

Another useful resource, with SPSS macros is [here](http://www.comm.ohio-state.edu/ahayes/SPSS%20programs/indirect.htm).

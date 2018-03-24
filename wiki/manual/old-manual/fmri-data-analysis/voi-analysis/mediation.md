# Mediation 

__Brief note on mediation:__

![Mediator.jpg]()

“In psychology, a mediation model is one that seeks to identify and explicate the mechanism that underlies an observed relationship between an independent variable and a dependent variable via the inclusion of a third explanatory variable, known as a mediator variable. Rather than hypothesizing a direct causal relationship between the independent variable and the dependent variable, a mediational model hypothesizes that the independent variable causes the mediator variable, which in turn causes the dependent variable.” (Wikipedia)

Mediation effects are simple to determine when you are already running logistic regressions. If the outcome variable is binary, logistic regressions can be used for the paths between the predictor and mediator and the outcome, while the “regress” regression command can be used from predictor to mediator, if the mediator is scalar. Then simply plug in the resulting coefficients and standard errors for paths a and b above into a mediation calculation.

This page can be used to compute mediation significance:
```
http://www.psych.ku.edu/preacher/sobel/sobel.htm
```
or here:
```
http://www.danielsoper.com/statcalc/calc31.aspx
```
Another useful resource, with SPSS macros:
```
http://www.comm.ohio-state.edu/ahayes/SPSS%20programs/indirect.htm
```

If you've have followed the steps in [the roi analysis section](./roi_analysis.md), you should have a wide formated csv file containing your behavioral and brain data. 

If you would like to use this data to plot timecoures, you can use the time coures scripts in `spantoolbox/timecourses/plot_timecourses.R` and `timecourse_utilities.R` in order to make these quickly. 

The first script calls functions in the second, and loops over the ROIs and sides you want to make timecourses for. each timecourese is specified like so:
```
######## Stock Goes Up vs Down, Split by Previous Trial
tt <- standard_title(side, mask,"Stock Outcome, Split by Previous Trial", exp)
plt <- standard_df(subdf, roi, c('variable', 'Result', 'Previous_Trial'), max_tr=max_tr)
write.csv(x=plt, paste0(mask, side,'_', exp, '_stock_outcome_split_trial.csv'))
print(nrow(df))
print(nrow(plt))
color_by = 'Result'
facet_eqn <-  as.formula(". ~ Previous_Trial")
legend_title <- "Next Day:"
legend <- c("Down", "Up")
fg <-  bw_fg(plt, color_by, facet_eqn, max_tr = max_tr, legend=legend , legend_title=legend_title)
labels <- c("Went Down" = "Previous day: Price went DOWN", "Went Up" = "Previous day: Price went UP")
fg <- fg + facet_grid(facet_eqn, labeller=labeller(Previous_Trial = labels))
standard_save(plt, fg, tt, color_by)
```

To understand what's going on:
* standard title will render the title name. 
* standard df will be passed all the variables which appear below it (don't remove 'variable')
* we write a csv containing the data that goes into each plot
* the facet equation will create a panel for each variable combination in the formula (see facet_grid ggplot2 documentation)
* the fg command makes the figure
* the labels command lets you control the legend. 

If you're familiar with ggplot2, then the timecourse_utilities function  should be fairly transparent--feel free to change it however you'd like to get the results you want. 


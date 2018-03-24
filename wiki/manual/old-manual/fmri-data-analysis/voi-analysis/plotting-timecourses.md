# Plotting Timecourses

## Contents
  1. [In Matlab](#matlab)
  2. [In Excel](#excel)
      1. [SPSS](#spss)
      2. [Deltagraph](#deltagraph)
      
<a name='matlab'></a>
## In Matlab
There is a demo with practice data from the endowment study on dmthal in /data2/endowfmri/toolsDemo. It needs to be run from here because tcPaste grabs information from the subject folders for this study.

Here is the demo:

\<source lang="MATLAB"> %% Using tcPaste

help tcPaste

which tcPaste

subjects = {'mh'; 'tj'; 'ss'; 'rs'; 'da'; 'kp'; 'jh'; 'bg'; 'lb'; 'mr'; 'sj'; 're'; 'ng'; 'sb'; 'cs'; 'ad'; 'js'; 'sk'}; regions = {'lnacc_pbsc'; 'rnacc_pbsc'; 'nacc_pbsc'; 'lmpfc_pbsc'; 'rmpfc_pbsc'; 'mpfc_pbsc'; 'linsula_pbsc'; 'rinsula_pbsc'; 'insula_pbsc'; 'lacing_pbsc'; 'racing_pbsc'; 'acing_pbsc'; 'lpcing_pbsc'; 'rpcing_pbsc'; 'pcing_pbsc'};
<br/>
<br/>
<br/>
tcPaste(subjects, regions); % or tcPaste(\['mh'; 'tj'; 'ss'; 'rs'; 'da'; 'kp'; 'jh'; 'bg'; 'lb'; 'mr'; 'sj'; 're'; 'ng'; 'sb'; 'cs'; 'ad'; 'js'; 'sk'], {'lnacc_pbsc'; 'rnacc_pbsc'; 'nacc_pbsc'; 'lmpfc_pbsc'; 'rmpfc_pbsc'; 'mpfc_pbsc'; 'linsula_pbsc'; 'rinsula_pbsc'; 'insula_pbsc'; 'lacing_pbsc'; 'racing_pbsc'; 'acing_pbsc'; 'lpcing_pbsc'; 'rpcing_pbsc'; 'pcing_pbsc'})

%Note: tcPaste will look for files that incluse the subjects initials %foollowed by the exact region name followed by ".tc" for example the first %file it would look for in this example would be "mhlnacc_pbsc.tc"

%% Using plotTCs

help plotTCs

%%%% Ploting One Region %%%%

%first load in the matrix by double clicking on the file name in the %"Current Directory" window or using the following command:

nacc_pbsc = csvread('nacc_pbsc.csv');

%now a matrix called "nacc_pbsc" will appear in the "Workspace" window %(click on the tab next to "Current Directory" to see this window)

%things specified correctly: plotTCs(nacc_pbsc, {'nacc'}, {'buy'; 'sell'; 'choose'}, {'low'; 'mid'; 'high'})

%alternatively: plotTCs(nacc_pbsc, {'nacc'}, {'buy low'; 'buy mid'; 'buy high'; 'sell low'; 'sell mid'; 'sell high'; 'choose low'; 'choose mid'; 'choose high'})

%or plotTCs(nacc_pbsc, {'nacc'}, {'buy'; 'sell'; 'choose'}, {'all'}, 33) % The last argument here specifies the number of TRs that coorespond to each %trial type. That is why this command will work buy the one below will %not.
<br/>
<br/>
<br/>
%specifying things incorrectly:

%this will work but things will be labeled wrong: plotTCs(nacc_pbsc, {'nacc'}, {'low'; 'mid'; 'high'}, {'buy'; 'sell'; 'choose'})

%this will not work because the information is incomplete: plotTCs(nacc_pbsc, {'nacc'}, {'low'; 'mid'; 'high'})
<br/>
<br/>
<br/>
%%%Adding a highlight period %if you want a certain period of your trial to be highlighted you can do so %like this: plotTCs(nacc_pbsc, {'nacc'}, {'buy'; 'sell'; 'choose'}, {'low'; 'mid'; 'high'}, 11, 5:6) %this will highlight the price period (TRs 5 through 6)
<br/>
<br/>
<br/>
%%%% Plotting all Regions %%%%

%first load in the matrix by double clicking on the file name in the %"Current Directory" window or using the following command:

all_regions_tc = csvread('all_regions_tc.csv');

%The list of regions does not have to be identical to the list in tcPaste, %However, it must be int the same order. If it is not in the correct order %things will be labled incorrectly. % %You must include all of the regions that are in the input matrix. If you %try to use a subset of them plotTCs will not work. regions = {'lnacc'; 'rnacc'; 'nacc'; 'lmpfc'; 'rmpfc'; 'mpfc'; 'linsula'; 'rinsula'; 'insula'; 'lacing'; 'racing'; 'acing'; 'lpcing'; 'rpcing'; 'pcing'};

%plotTCs works exactly the same way but this time you will get a different %figure for each brain region. plotTCs(all_regions_tc, regions, {'buy'; 'sell'; 'choose'}, {'low'; 'mid'; 'high'})

\</source>

<a name='excel'></a>
## In Excel
On insula or the windows laptop, open a template file for the given study or steal a template TC excel file from a similar study - in data/studyname/timecourses - so that graphs appear automatically. Or make a new one. Then open a .csv file for an ROI, copy all the subject's TCs and averages/stddev to the template or new Excel file, check that the graphs are plotting from the right data (select a graph, then go to graph→choose dataset) and see what you've got. It’s useful to plot the averages and stddev on one graph or sheet in the excel file, and plot each individual on the same page as the data is copied onto, so that an individual can be matched to a timecourse.

note2: Because we now use a different script to generate the baseline files in the initial processing, we no longer need to set a baseline file in reordertcdump or divide the TC data by a baseline in arrangetcs, where we now use the python script 'tcourse2excelNoBaseDiv.py' instead of the older one, 'tcourse2excel.py.'

note3: see Jamil's sweet pictures from the 'processing.presentation' in Spanlab docs for more on reordering events.

Exploratory Excel timecourse plotting is an essential part of any study. Using this, we determine what direction brain activation is going in different trials and timepoints in a priori regions of interest or those that appear in the group maps. This grants much better understanding of the data than maps alone provide. For example, if depressed subjects show greater activation than controls in the anterior cingulate when anticipating high vs. low gain rewards, what is actually going on in the ACC? Timecourses show that instead of higher activation for high gain trials in depressed subjects, the difference is due to much lower low-gain ACC activity for the depressed subjects. Thus the difference between hi and low gain trials between groups could be due to at least two underlying causes (a third would be lo>hi gain activity in the control group ACC) that are only established by plotting actual activation.

<a name='SPSS'></a>
### SPSS
After you’ve dumped your timecourses to Excel .csv files, you can open them in SPSS and perform additional statistical tests (e.g., interactions of valence, differences in time courses of high versus low gain or any other contrast) that may or may not integrate behavioral information, as well.

<a name='deltagraph'></a>
### Deltagraph
Delta Graph is a program that does graphs. It has a variety of spreadsheet and graph files. Must click on a graph to use anything in the graph file menu.

When you have a delta graph graph, and you want to copy it to somewhere else, in one of the menus you can group the graph into one item before copying.

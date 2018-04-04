# Matlab

Here is a short matlab tutorial on plotting. You can find the .m file for this and some practice data in ~/smg_usefulMatlab on dmthal.

\<source lang="MATLAB"> %% Matlab Tutorial % Stephanie Greer 1/08/07

%% importing data from a csv file

%if there are no labels just use csvread: mpfcBuyHigh = csvread('mpfcBuyHighRaw.csv');

%if there are labels csvread will not work. %instead use: mpfcBuy1 = importdata('mpfcBuyLabeled.csv'); %this will make mpfcBuy1 into a struct with 2 fields: %one which is the titles of the columns mpfcBuy1.colheaders %and another that contains the data mpfcBuy1.data

%or %csvbyname is not a built in matlab function. It is a function that lives %on dmthal in /home/span/smg_usefulmatlab

mpfcBuy = csvbyname('mpfcBuyLabeled.csv'); %this will make mpfcBuy into a struct with fields for each column in the %csv file plus an extra field that contains all the data. for example:

%to see all the fields use: mpfcBuy

%for all the data use: mpfcBuy.all

%for one particular column use: mpfcBuy.time %or mpfcBuy.lowPrice %for example.

<br>
<br>

%% Ploting Data % The general way to approach ploting in matlab is to first open a new % figure if necessary, then use a ploting command to add data, and then add % things like titles or change the range of the axies. If you don't do % things in this order things might not work.

%Using "plot" to make line and scater plots:

%set up some practice data. x = mpfcBuy.time; y = mpfcBuy.highPrice; y2 = mpfcBuy.lowPrice;

<br>
<br>

%the simplist plot: plot(y) %if you only give plot 1 argument it will simply use the numbes 1 2 3 4 etc. ast the x axis %slightly more sophisticated: figure plot(x, y)

%there is a very handy third argument to plot that can be used to specfy %many different things %shapes for a scatter plot: plot(x, y, '\*') %other shapes include: o + ^ . %simple colors: plot(x, y, 'g') %other simple colors include: r, y, b, m, c, k for (red yellow blue magenta cyan and black) %simple line styles: plot(x, y, '--') %other line styles include: - -. and :

%or you can put all three together: plot(x, y, '\*g--') %another example: plot(y, '-ob')

%There are many many other ways to specfy how exactly you want your data to %look when you plot it. If you want to go beyond the short cuts listed above, you'll need to use argumenst of this general form: %plot(x, y, 'Property Name', Property Value, 'Property Name', Property Value, ....) %examples: plot(x, y, 'LineWidth', 3) plot(x, y, 'Color', [.5 .5 .5], 'LineWidth', 6) plot(x, y, '.', 'MarkerSize', 50, 'Color', [.5 0 .5])

<br>
<br>

%note when you use the plot command twice or more in a row like this, you only end up %with one plot which contains data from the last time you used plot.

%to make two figures use the figure comand:

plot(x, y2) %uses an automatic figure window. figure %opens a new blank window. plot(x, y) %plots in the new figure window. close all %this automatically closes all figure windows

%to plot two lines on the same graph plot(x, y2, 'b') hold on %tells matlab to keep the data on the figure plot(x, y, 'g')

%one other way to plot lines on the same graph is simply to use a matrix %instead of a vector. plot will automatically show different columns of the %same matrix as difffernt lines on the graph plot(mpfcBuy.all(:, 2:4))

<br>
<br>

%to plot two seperate charts in the same window subplot(2, 1, 1) %"subplot" sets up a figure for you that is divided into 2 rows (the first %argument) and 1 column (the second argument) the third argument specifies %which plot you're talking about. In this case we want to use the first %plot. plot(x, y2) subplot(2, 1, 2) %now we want the second plot. plot(x, y)

%% error bars

%example data: x = mpfcBuy.time; means = mpfcBuy.highPrice; sem = mpfcBuy.high_sem;

%you can plot a graph with error bars by useing the errorbar function %instead of plot. for example:

errorbar(x, means, sem)

%all the same formating properties apply: errorbar(x, means, sem,'\*g--') errorbar(x, means, sem,'.', 'MarkerSize', 50, 'Color', [.5 0 .5])

%you can allso plot a bunch of lines at the same time, just like using plot: errorbar([x, x, x], mpfcBuy.all(:, 2:4), mpfcBuy.all(:, 5:end)) %Note: x is repeated to match the size of the other inputs.

%an easy way to calulate means and standarderrors if you have raw data is %to use grpstats:

%the variable mpfcBuyHigh has the raw subject data from a timecourse dump. mpfcBuyHigh

[means2, sem2] = grpstats(mpfcBuyHigh);

%then you can use the output from grpstat as input to errorbar errorbar(x, means2, sem2)

%% bar graphs

% use the bar function to make bar plots:

bar(means) %or bar(x, means)

%if there are multibple rows in your input matrix you will get different %colorbars: bar(mpfcBuy.all(:, 2:4))

%% titels and labels

errorbar([x, x, x], mpfcBuy.all(:, 2:4), mpfcBuy.all(:, 5:end))

title('mpfc Buy trials') xlabel('Time') ylabel('% Signal change')

legend({'High'; 'Mid'; 'Low'})

line([0, 20], [0, 0])

%% axis:

errorbar([x, x, x], mpfcBuy.all(:, 2:4), mpfcBuy.all(:, 5:end))

%all at once: axis([0, 9, -.15, .15])

errorbar([x, x, x], mpfcBuy.all(:, 2:4), mpfcBuy.all(:, 5:end))

%for in parts xlim([0, 9])

ylim([-.15, .15])

%% image plots

%for data: imagesc(mpfcBuy.all(:, 2:4)'); caxis([0 1])

%for pictures: testPic = imread('anat.ppm', 'ppm'); imagesc(testPic) </source>

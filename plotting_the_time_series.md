# Plotting the time series

The *hctsa* package provides a simple means of plotting time series, via the `TSQ_plot_timeseries` function.

For example, to plot a set of time series that have not been assigned groups, we can run the following:

    whatData = 'norm'; % Get data from HCTSA_N.mat
    plotWhatTimeSeries = 'all'; % plot examples from all time series
    plotHowMany = 10; % how many to plot
    maxLength = 400; % maximum number of samples to plot for each time series
    TSQ_plot_timeseries(whatData,plotWhatTimeSeries,plotHowMany);
    
For our assorted set of time series, this produces the following:

![](timeSeriesPlot.png)

Showing 10 examples of time series, equally-spaced through the **ts_id**s in **HCTSA_N.mat**.
Many more custom plotting options are available by passing an options structure to `TSQ_plot_timeseries`, including the 'freeform' option which allows very many time series to be shown in a single plot (without the usual axis borders):
    
    plotOptions = struct('freeform',1);
    TSQ_plot_timeseries('norm','all',10,500,);

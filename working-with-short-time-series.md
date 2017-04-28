## Working with short time series

Although many sections of the time-series analysis literature has worked to develop methods for quantifying complex temporal structure in long time-series recordings, many time series that are analyzed in practice are relatively short.
_hctsa_ has been successfully applied to time-series classification problems in the data mining literature, which includes datasets of time series as short as 60 samples, [link](http://ieeexplore.ieee.org/lpdocs/epic03/wrapper.htm?arnumber=6786425).
However, time-series data are sometimes even shorter, including yearly economic data across perhaps six years, or biological data measured at say 10 points across a lifespan. Although many features in _hctsa_ will not give a meaningful output when applied to a short time series, _hctsa_ includes methods for filtering such features (cf. [`TS_normalize`](filtering_and_normalizing.md)), after which the remaining features can be used for analysis.

The number of features with a meaningful output, from time series as short as 5 samples, up to those with 500 samples, is shown below:
![](/img/LengthDependence.png)
In each case, over 3000 features can be computed.
However, note that one must be careful when representing a 5-dimensional object, with thousands of features, the vast majority of which will be highly redundant.

### Sample application to developmental gene expression data
To demonstrate the feasibility of running _hctsa_ analysis on datasets of short time series, we applied _hctsa_ to gene expression data in the cerebellar brain region, r1A, across seven developmental time points (from the Allen Institute's [Developing Mouse Brain Atlas]((http://developingmouse.brain-map.org)), for a subset of 50 genes.
After filtering and normalizing (`TS_normalize`) and clustering (`TS_cluster`), we plotted the clustered time-series data matrix (`TS_plot_DataMatrix('cl')`):

![](/assets/GeneExpressionExample.png)

Visually, using the time series plots to the left of the colored matrix, we can see that genes with similar temporal expression profiles are clustered together based on their 2829-long feature vector representations.
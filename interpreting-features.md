## Interpreting _hctsa_ features

Often, an _hctsa_ analysis will yield a list of features that may be particularly relevant to a given time-series analysis task, e.g., features that robustly distinguish time series recorded from individuals with some disease diagnosis compared to that of healthy controls. In cases like this, the next step for the analyst is to bridge the automated feature selection enabled by _hctsa_ to domain understanding.
Consider a problem in which `TS_TopFeatures` is run to find features that accurately distinguish groups of time series, yielding a list of features like the following:

```
[3016] FC_LocalSimple_mean3_taures (forecasting) -- 59.97%
[3067] FC_LocalSimple_median3_taures (forecasting) -- 58.14%
[2748] EN_mse_1-10_2_015_sampen_s3 (entropy,sampen,mse) -- 54.10%
[7338] MF_armax_2_2_05_1_AR_1 (model) -- 53.71%
[7339] MF_armax_2_2_05_1_AR_2 (model) -- 53.31%
[3185] DN_CompareKSFit_uni_psx (distribution,ksdensity,raw,locdep) -- 52.14%
[6912] MF_steps_ahead_ar_best_6_ac1_3 (model,prediction,arfit) -- 52.11%
[6564] WL_coeffs_db3_4_med_coeff (wavelet) -- 52.01%
[4552] SP_Summaries_fft_logdev_fpoly2csS_p1 (spectral) -- 51.57%
[6634] WL_dwtcoeff_sym2_5_noisestd_l5 (wavelet,dwt) -- 51.48%
[930] DN_FitKernelSmoothraw_entropy (distribution,ksdensity,entropy,raw,spreaddep) -- 51.37%
[6574] WL_coeffs_db3_5_med_coeff (wavelet) -- 51.26%
[6630] WL_dwtcoeff_sym2_5_noisestd_l4 (wavelet,dwt) -- 51.04%
[1891] CO_StickAngles_y_ac2_all (correlation) -- 50.85%
[16] rms (distribution,location,raw,locdep,spreaddep) -- 50.83%
[6965] MF_steps_ahead_arma_3_1_6_rmserr_6 (model,prediction) -- 50.83%
[2747] EN_mse_1-10_2_015_sampen_s2 (entropy,sampen,mse) -- 50.35%
[4201] SC_FluctAnal_mag_2_dfa_50_2_logi_ssr (scaling) -- 50.34%
[6946] MF_steps_ahead_ss_best_6_meandiffrms (model,prediction) -- 50.33%
```

Functions like `TS_TopFeatures` are helpful in showing us how these different types of features might cluster into groups that measure similar properties (as shown in the [previous section](ts_topfeatures.md)). This helps us to be able to inspect sets of similar, inter-correlated features together as a group, but even when we have isolated such a group, how can we start to interpret and understand what these features are actually measuring?
Some features in the list may be easy to interpret directly (e.g., `rms` in the list above is simply the root-mean-square of the distribution of time-series values), and others have clues in the name (e.g., features starting with `WL_coeffs` are to do with measuring wavelet coefficients, features starting with `EN_mse` correspond to measuring the multiscale entropy, mse, and features starting with `FC_LocalSimple_mean` are related to time-series forecasting using local means of the time series).
Below we outline a procedure for how a user can go from a time-series feature selected by _hctsa_ towards a deeper understanding of the type of algorithm that feature is derived from, how that algorithm performs across the dataset, and thus how it can provide interpretable information about your specific time-series dataset.

### Inspecting keywords

The simplest way of interpreting what sort of property a feature might be measuring is from its keywords, that often label individual features by the class of time-series analysis method from which they were derived. In the list above, we see keywords listed in parentheses, as _'forecasting'_ \(methods related to predicting future values of a time series\), _'entropy'_ \(methods related to predictability and information content in a time series\), and _'wavelet'_ \(features derived from wavelet transforms of the time series\). There are also keywords like _'locdep'_ \(location dependent: features that change under mean shifts of a time series\), _'spreaddep'_ \(spread dependent: features that change under rescaling about their mean\), and _'lengthdep'_ \(length dependent: features that are highly sensitive to the length of a time series\).

### Inspecting code

To find more specific detailed information about a feature, beyond just a broad categorical label of the literature from which it was derived, the next step is find and inspect the code file that generates the feature of interest. For example, say we were interested in the top performing feature in the list above:
```
    [3016] FC_LocalSimple_mean3_taures (forecasting) -- 59.97%
```
We know from the keyword that this feature has something to do with forecasting, and the name provides clues about the details (e.g., `FC_` stands for forecasting, the function `FC_LocalSimple` is the one that produces this feature, which, as the name suggests, performs simple local time-series prediction).
We can use the feature ID (3016) provided in square brackets to get information from the `Operations` structure array:

```matlab
>> disp(Operations([Operations.ID]==3016));
            ID: 3016
          Name: 'FC_LocalSimple_mean3_taures'
      Keywords: 'forecasting'
    CodeString: 'FC_LocalSimple_mean3.taures'
      MasterID: 836
```

Inspecting the text before the dot, '.', in the `CodeString` field (`FC_LocalSimple_mean3`) tells us the name that _hctsa_ uses to describe the Matlab function and its unique set of inputs that produces this feature. Whereas the text following the dot, '.', in the `CodeString` field (`taures`), tells us the field of the output structure produced by the Matlab function that was run.
We can use the `MasterID` to get more information about the code that was run using the `MasterOperations` structure array:

```matlab
>> disp(MasterOperations([MasterOperations.ID]==836));
       ID: 836
    Label: 'FC_LocalSimple_mean3'
     Code: 'FC_LocalSimple(y,'mean',3)'
```

This tells us that the code used to produce our feature was `FC_LocalSimple(y,'mean',3)`.
We can get information about this function in the commandline by running a `help` command:

```matlab
>> help FC_LocalSimple
  FC_LocalSimple    Simple local time-series forecasting.

  Simple predictors using the past trainLength values of the time series to
  predict its next value.

 ---INPUTS:
  y, the input time series

  forecastMeth, the forecasting method:
           (i) 'mean': local mean prediction using the past trainLength time-series
                        values,
           (ii) 'median': local median prediction using the past trainLength
                          time-series values
           (iii) 'lfit': local linear prediction using the past trainLength
                          time-series values.

  trainLength, the number of time-series values to use to forecast the next value

 ---OUTPUTS: the mean error, stationarity of residuals, Gaussianity of
  residuals, and their autocorrelation structure.
```

We can also inspect this code `FC_LocalSimple` directly for more information. Like all code files for computing time-series features, `FC_LocalSimple.m` is located in the Operations directory of the _hctsa_ repository. Inspecting the code file, we see that running `FC_LocalSimple(y,'mean',3)` does forecasting using local estimates of the time-series mean (since the second input to `FC_LocalSimple`, `forecastMeth` is set to `'mean'`), using the previous three time-series values to make the prediction (since the third input to `FC_LocalSimple`, `trainLength` is set to `3`).

To understand what the specific output quantity from this code is that came up as being highly informative in our `TS_TopFeatures` analysis, we need to look for the output labeled `taures` of the output structure produced by `FC_LocalSimple`. We discover the following relevant lines of code in `FC_LocalSimple.m`:
```matlab
% Autocorrelation structure of the residuals:
out.ac1 = CO_AutoCorr(res,1,'Fourier');
out.ac2 = CO_AutoCorr(res,2,'Fourier');
out.taures = CO_FirstZero(res,'ac');
```
This shows us that, after doing the local mean prediction, `FC_LocalSimple` then outputs some features on whether there is any residual autocorrelation structure in the residuals of the rolling predictions (the outputs labeled `ac1`, `ac2`, and our output of interest: `taures`). The code shows that this `taures` output computes the `CO_FirstZero` of the residuals, which measures the first zero of the autocorrelation function (e.g., cf `help CO_FirstZero`).
When the local mean prediction still leaves alot of autocorrelation structure in the residuals, our feature, `FC_LocalSimple_mean3_taures`, will thus take a high value.

### Visualizing outputs
Once we've seen the code that was used to produce a feature, and started to think about how such an algorithm might be measuring useful structure in our time series, we can then check our intuition by inspecting its performance on our dataset (as described in [Investigating specific operations](feature_summary.md)).

For example, we can run the following:

```matlab
TS_FeatureSummary(3016,'raw',true);
```
which produces a plot like that shown below. We have run this on a dataset containing noisy sine waves, labeled 'noisy' (red) and periodic signals without noise, labeled 'periodic' (blue):
![](img/FeatureSummaryForInterpretation.png) On the plot on the right, we see how this feature orders time series (with the distribution of values shown on the left, and split between the two groups: 'noisy', and 'periodic').
Our intuition from the code, that time series with longer correlation timescales will have highly autocorrelated residuals after a local mean prediction, appears to hold visually on this dataset. In general, the mechanism provided by `TS_FeatureSummary` to visualize how a given feature orders time series, including across labeled groups, can be a very useful one for feature interpretation.

### Summary
_hctsa_ contains a large number of features, many of which can be expected to be highly inter-correlated on a given time-series dataset. It is thus crucial to explore how a given feature relates to other features in the library, e.g., using the correlation matrix produced by `TS_TopFeatures` (cf. [Finding informative features](ts_topfeatures.md)), or by searching for features with similar behavior on the dataset to a given feature of interest (cf. [Finding nearest neighbors](sim_search.md)).
In a specific domain context, the analyst typically needs to decide on the trade-off between more complicated features that may have slightly higher in-sample performance on a given task, and simpler, more interpretable features that may help guide domain understanding. The procedures outlined above are typically the first step to understanding a time-series analysis algorithm, and its relationship to alternatives that have been developed across science.
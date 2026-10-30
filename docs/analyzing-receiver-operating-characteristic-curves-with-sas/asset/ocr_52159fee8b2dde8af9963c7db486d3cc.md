As was the case in the magnetic resonance example, sometimes more than one observation may be obtained from an individual unit. In this case, the research question centers on identifying abnormal vertebral marrow processes and classifying them as normal or benign. The radiologist performing the magnetic resonance imaging measures the signal at as many vertebrae as indicated based on the image. Some patients may contribute a single measurement while others will contribute several. When a patient contributes more than one observation to the analysis, the resulting data set is said to be *clustered*. The term is motivated by the observation that data coming from the same units behave similarly and tend to cluster together.

An intuitive way to think about clustered data is that the amount of information in two data points that are independent is generally more than the amount of information in two data points that are clustered. Clustering makes individual data points behave more like each other than they would otherwise. The net result is often an increase in standard errors (when compared with independence across data points), while point estimates are usually not substantially affected.

One can think of clustered data examples in other fields. A weather forecast for a given day is usually multi-dimensional: Temperature, precipitation, and pressure may all be simultaneously forecast. The forecasts within a given day will tend to be clustered because they are all related to various aspects of the same underlying meteorological events.

Clustered data do not always arise from multiple measurements on each observational unit. There could be other reasons to suspect that a subset of the observations behave like one another. For example, debtors in the same ZIP code may exhibit similar behaviors of default. Parts manufactured by the same factory may exhibit similar patterns of failure. All of these will have a bearing on the analysis of ROC curves.

Analysis of clustered data is a statistical topic in itself. An overview from a SAS user’s perspective can be found in *SAS for Mixed Models, Second Edition* by Littell, et al. (2006) for continuous outcomes and *Categorical Data Analysis Using the SAS System, Second Edition* by Stokes, Davis, and Koch (2001).

One method of handling clustered data is based on generalized estimating equations. This is a way of estimating the model parameters using a method that is robust to departures from the assumption of the independence of data points. This method has been very popular in analyzing correlated binary data, and it has been extended to the case of censored data. It can be fit using the COVSANDWICH option in PROC PHREG. The option is named after the sandwich estimator of the covariance, which is one of the features of the estimators obtained by estimating equations.

If we modify the call to PROC PHREG as follows, the model is fit using estimating equations instead of the usual partial likelihood. The ID statement defines the variable that identifies the clustering variable. Records sharing the same ID value are clustered.

```
proc phreg data=tpm1.ph covsandwich(aggregate);
    model signal=group/r1;
    if group='Normal' then grp=0;
    else if group='Benign' then grp=1;
    id name;
run;
```
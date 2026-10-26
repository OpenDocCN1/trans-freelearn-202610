# Spatially interpolating point data

Spatial interpolation is the procedure by which the behavior of a certain phenomenon of interest is predicted in locations where it has not been measured. For this purpose, we need a spatial prediction model — a set of procedures to obtain the predicted values given the calibration data. The two types of calibration data usually encountered are:

+   - **Field measurements**: Available for a limited set of locations (usually points), for example, meteorological data from stations in Spain
+   - **Covariates**: Available for each location within the area of interest, for example, elevation data from Spain's DEM

The spatial prediction model of our choice is calibrated using the calibration data. This model can then be used to calculate the predicted level of the phenomenon of interest in any location (usually points). The two main types of spatial interpolation methods recognized are:

+   - **Deterministic model**: In this model, model parameter values are arbitrarily determined
+   - **Statistical model**: In this model, model parameter values are objectively estimated from specific calibration data

Simply put, while both types of models use the calibration data to make predictions, deterministic models are predefined using fixed formulas to calculate predicted values given the calibration data; on the other hand, statistical models use the calibration data twice, first to fine-tune the model itself and then (as in the deterministic models) to calculate the predicted values. Examples of deterministic models are **inverse distance weighted (IDW)** interpolation and splines. Examples of statistical models are kriging and ordinary regression.

> Our purpose in this chapter is to present spatial interpolation from a practical point of view. Comprehensively covering the statistical theory and equations behind spatial interpolation techniques is well beyond the scope of this book. Readers familiar with the subject of spatial interpolation will, hopefully, get a perspective from this chapter on the way statistical tools such as these are applied in R. Readers unfamiliar with the subject of spatial interpolation are referred to the not-too-technical overview in the first two chapters of Tomislav Hengl's book *A Practical Guide to Geostatistical Mapping*, published in 2009 (available online), for supplementary reference. Those uninterested in this particular subject may skip to the last image in this chapter, and the few paragraphs that follow, where a dataset that we are going to use in the next chapter is described.
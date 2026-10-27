# ARIMA

With ETS, ARIMA is another very well-known family of flexible statistical forecasting models. ARIMA is a generalization of the ARMA family of models, where ARMA describes a time series with two polynomials: one for autoregression and the other for the MA.

Although all these family of models involves inputting many parameters and computing coefficients, Amazon Forecast leverages the auto.arima method from the forecast R package available on the Comprehensive R Archive Network (CRAN). This method conducts a search over the possible values of the different ARIMA parameters to identify the best model of this family.

# Theoretical background

Let's call yₜ the raw values of our initial time series and let's decompose the different steps needed to estimate an ARIMA model, as follows:

+   1. We start by making the time series stationary by using differencing. In statistics, this transformation is applied to non-stationary time series to remove non-constant trends. In the ARIMA implementation of Amazon Forecast, differencing is applied once (first-order differencing yₜ') or twice (second-order differencing yₜ'') and is the reason why the model is called integrated (the I in ARIMA stands for integrated). The formula is provided here:
+   
+       yₜ' = yₜ − yₜ₋₁
+       yₜ'' = yₜ' − yₜ₋₁' = yₜ − 2·yₜ₋₁ + yₜ₋₂

+   2. We then use this differenced data to build an autoregressive (AR) model that specifies that our prediction depends linearly on the previous timesteps and on a random term. Let's call Yₜ (with a capital Y) the differenced data obtained after the integrated step of the algorithm (Yₜ will either be yₜ' or yₜ'', depending on the differencing order retained in the previous step). An AR model of order p can be written as a function of previous terms of the series and of a series of coefficients φ₁ to φₚ, as shown in the following formula:
+   
+       Yₜ = c + εₜ + φ₁Yₜ₋₁ + φ₂Yₜ₋₂ + ⋯ + φₚYₜ₋ₚ
+   3. Once we have an AR model, we use an MA model to capture the dependency of our prediction on the current and previous values of the random term. An MA model of order q can be written as a function of the mean μ of the series and of a list of parameters θ₁ to θ_q applied to random error terms ε_t to ε_{t-q}, as shown in the following formula:

$$Y_t = \mu + \varepsilon_t + \theta_1\varepsilon_{t-1} + \cdots + \theta_q\varepsilon_{t-q}$$

+   4. When putting together an AR model and an MA model after differencing, we end up with the following estimation for the ARIMA model:

$$Y_t = c + \varepsilon_t + \varphi_1Y_{t-1} + \varphi_2Y_{t-2} + \cdots + \varphi_pY_{t-p} + \theta_1\varepsilon_{t-1} + \cdots + \theta_q\varepsilon_{t-q}$$

# Implementation in Amazon Forecast

When Amazon Forecast trains a model with ETS, it uses the default parameters of the `arima` function available in the R `forecast` package. Amazon Forecast also uses the `auto.arima` method, which conducts a search of the parameter space to automatically find the best ARIMA parameters for your dataset. This includes exploring a range of values for the following:

+   - The differencing degree
+   - The AR order
+   - The MA order

This means that all these `arima` parameters are automatically estimated and that the model type is also automatically selected: all of these choices are abstracted away from you as multiple models are run behind the scenes, and the best one is selected.

Amazon Forecast will create a local model for each time series available in your dataset. In the energy consumption example we have been running, each household electricity consumption would be modeled as a single ARIMA model.

If you want to get more in-depth details about the automatic parameter choice from ARIMA, I recommend that you deep dive into this paper by Hyndman and Khandakar: *Automatic Time Series Forecasting: The forecast Package for R*. Here is a persistent link to this paper: `https://doi.org/10.18637/jss.v027.i03`.
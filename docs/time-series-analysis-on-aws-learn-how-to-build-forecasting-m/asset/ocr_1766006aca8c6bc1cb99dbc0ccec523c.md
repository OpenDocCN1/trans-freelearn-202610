Once you click the Train predictor button, the following screen will open up to let you customize your predictor settings:

![](bbox=[0.13, 0.15, 0.87, 0.43])

Figure 5.5 – Predictor settings

For your Predictor settings options, we are going to fill in the following parameters:

+   - **Predictor name:** `london_energy_cnnqr_hpo`.
+   - **Forecast frequency:** We will keep the same frequency as before, which is also the original frequency of the ingested data. Let’s set this parameter to 1 day.
+   - **Forecast horizon:** 30. As the selected forecast horizon is 1 day, this means that we want to train a model that will be able to generate the equivalent of 30 days' worth of data points in the future. We want to compare the performance of our initial predictor with this new one, so let’s keep the same horizon to make this comparison more relevant.
+   - **Forecast quantiles:** Leave the settings at their default values, with three quantiles defined at 0.10, 0.50, and 0.90. We will detail this setting later in this chapter.

In the next section, disable the AutoPredictor toggle and select CNN-QR in the Algorithm selection dropdown.

Click on Perform hyperparameter optimization (HPO). A new section called HPO Config appears, where you can decide the range of values you want Amazon Forecast to explore for the different tunable hyperparameters of CNN-QR. This configuration must be written in JavaScript Object Notation (JSON) format.
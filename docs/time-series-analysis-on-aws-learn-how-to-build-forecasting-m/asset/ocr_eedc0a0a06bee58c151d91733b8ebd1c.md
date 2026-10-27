+   5. For the **Geolocation format** option, you have a choice between **Lat/Long Decimal Degrees** or **Postal Code** (if your location data is in the US).

You can then continue with the ingestion process, as explained in *Chapter 3, Creating a Project and Ingesting your Data*. If you have other datasets, you can also ingest them in the related time series data and item metadata dataset. You can then train a new predictor based on this dataset group.

# Creating a predictor with the Weather index

Once you have a dataset group with location data, you can enable the **Weather** index feature while creating a new predictor based on this dataset, as follows:

+   1. On the Amazon Forecast console, click on **View dataset groups** to bring up a list of existing dataset groups and choose a dataset group where your location-aware dataset has been ingested.
+   2. In the navigation pane, choose **Predictors** and click on **Train new predictor**.
+   3. On the predictor creation page, scroll down to the optional **Supplementary features** section and toggle on the **Enable weather index** button, as illustrated in the following screenshot:

![](bbox=[0.14, 0.5, 0.82, 0.75])

Figure 5.14 – Enabling the Weather supplementary feature

When you're ready to train a predictor, click on the **Create** button at the bottom of the prediction creation screen to start training a predictor with weather data.

Configuring your backtesting strategy happens in the **Predictor settings** section when you wish to create a new predictor without using the AutoPredictor settings, as illustrated in the following screenshot:

![](img/e4ee48104c730845e796d8c469dfeecc_168_0.png)

Figure 5.10 – Backtesting configuration for a new predictor

After you make your algorithm selection, you can choose from the following options:

- **Number of backtest windows:** This number can range from 1 to 5.
- **Backtest window offset:** This is the width of the window. By default, it is equal to the forecast horizon and has the same unit.

Let's create a new predictor with the following parameters:

- **Predictor name:** `london_energy_cnnqr_backtest`
- **Forecast horizon:** 30 (days)
- **Algorithm selection:** Manual
- **Algorithm:** CNN–QR
- **Number of backtest windows:** 5
- **Backtest window offset:** 30

Leave all the other parameters at their default values and click **Start** to create a new predictor with these settings. After an hour or so of training, you can explore the accuracy metric of this new predictor, as illustrated in the following screenshot:

![](img/e4ee48104c730845e796d8c469dfeecc_169_0.png)

Figure 5.11 – Backtesting accuracy metrics

You will notice that Amazon Forecast computes the accuracy metrics for each backtest window and also gives you the average of these metrics over five windows. You may note some fluctuations of the metrics, depending on the backtest window: this can be a good indicator of the variability of your time series across different time ranges, and you may run some more thorough investigation of your data over these different periods to try to understand the causes of these fluctuations.

You now know how to customize your backtesting strategy to make your training more robust. We are now going to look at how you can customize the features of your dataset by adding features provided by Amazon Forecast (namely, holidays and weather data).

## Including holiday and weather data

At the time of writing this book, Amazon Forecast includes two built-in datasets that are made available as engineered features that you can leverage as **supplementary features**: **Holidays** and the **Weather** index.

### Enabling the Holidays feature

This supplementary feature includes a dataset of national holidays for 66 countries. You can enable this feature when you create a new predictor: on the predictor creation page, scroll down to the optional Supplementary features section and toggle on the Enable holidays button, as illustrated in the following screenshot:

![](img/e4ee48104c730845e796d8c469dfeecc_170_0.png)

Figure 5.12 – Enabling the Holidays supplementary feature

Once enabled, a drop-down list appears, to let you select the country for which you want to enable holidays. Note that you can only select one country: your whole dataset must pertain to this country. If you have several countries in your dataset and wish to take different holidays into account, you will have to split your dataset by country and train a predictor on each of them.

When this parameter is selected, Amazon Forecast will train a model with and without this parameter: the best configuration will be kept based on the performance metric of your model.

### Enabling the Weather index

This supplementary feature includes 2 years of historical weather data and 2 weeks of future weather information for 7 regions covering the whole world, as follows:

- US (excluding Hawaii and Alaska), with latitude ranging from 24.6 to 50.0 and longitude between -126.0 and -66.4
- Canada, with latitude ranging from 41.0 to 75.0 and longitude from -142.0 to 52.0
- **Europe**, with latitude ranging from 34.8 to 71.8 and longitude from -12.6 to 44.8
- **South America**, with latitude ranging from -56.6 to 14.0 and longitude from -82.2 to -33.0
- **Central America**, with latitude ranging from 6.8 to 33.20 and longitude from -118.80 to -58.20
- **Africa & Middle East**, with latitude ranging from -35.60 to 43.40 and longitude from -18.8 to 58.20
- **Asia Pacific**, with latitude ranging from -47.8 to 55.0 and longitude from -67.0 to 180.60

To leverage weather data in Amazon Forecast, you need to perform the following actions:

- Ensure your dataset has data pertaining exclusively to one single region (for instance, all your data must be linked to locations that are all in Europe). If you have several regions in your data, you will have to create a dataset per region and train one predictor per region.
- Add geolocation information to your dataset.
- Include a geolocation attribute when creating and ingesting your target dataset.
- Select AutoPredictor or AutoML when creating your predictor or manually select an algorithm.

### Adding geolocation information to your dataset

Once you have a dataset with data from a single region (**US, Canada, Europe, Central America, Africa & Middle East, or South America**), you must add a location feature to your dataset. This feature can be one of the following:

- A ZIP code (if your locations are in the **United States (US)**). The column must follow this format: US, followed by an underscore character (_), followed by the 5-digit ZIP code. For instance, US_98121 is a valid postal code to be used.
- Latitude and longitude coordinates in decimal format, both coordinates being separated by an underscore character (_)—for instance, 47.61_-122.33 is a valid string. Amazon Forecast takes care of rounding to the nearest 0.2 degrees if you input raw values with more precision. This format is available for every region.

Our household electricity consumption dataset is from London and the coordinates of London are respectively 51.5073509 (latitude) and -0.1277583 (longitude). If we wanted to add weather data to our dataset, we could add a column called `location` to our dataset and set its value to 51.5073509_-0.1277583 for every row (as all our households are located in this city).

### Important Tip

As mentioned previously, the Weather index is only available for the past 2 years. If you want to get some practice with this feature, make sure the availability of weather data is compatible with the recording time of your time series data. If your time series data includes data points before July 1, 2018, the `weather_index` parameter will be disabled in the user interface (UI).

### Including a geolocation attribute

When ingesting a target time series data into a dataset group, you will have the opportunity to select a location type attribute. Follow these next steps:

1. After signing in to the AWS Management Console, open the Amazon Forecast console and click on the View dataset group button.
2. On the dataset group list screen presented to you, select an empty dataset group (or create a brand new one: you can refer back to Chapter 3, Creating a Project and Ingesting your Data, in case you need some guidance to create and configure a new dataset group).
3. When you're on your dataset group dashboard, click on the Import button that is next to the Target time series data label. You will arrive at the data ingestion wizard.
4. In the Schema Builder section, click on Add attribute, name your attribute `location`, and select geolocation for the Attribute Type value, as follows:

![](img/e4ee48104c730845e796d8c469dfeecc_172_0.png)

Figure 5.13 – Ingesting data with location information

5. For the **Geolocation format** option, you have a choice between **Lat/Long Decimal Degrees** or **Postal Code** (if your location data is in the US).

You can then continue with the ingestion process, as explained in *Chapter 3, Creating a Project and Ingesting your Data*. If you have other datasets, you can also ingest them in the related time series data and item metadata dataset. You can then train a new predictor based on this dataset group.

### Creating a predictor with the Weather index

Once you have a dataset group with location data, you can enable the **Weather** index feature while creating a new predictor based on this dataset, as follows:

1. On the Amazon Forecast console, click on **View dataset groups** to bring up a list of existing dataset groups and choose a dataset group where your location-aware dataset has been ingested.
2. In the navigation pane, choose **Predictors** and click on **Train new predictor**.
3. On the predictor creation page, scroll down to the optional **Supplementary features** section and toggle on the **Enable weather index** button, as illustrated in the following screenshot:

![](img/e4ee48104c730845e796d8c469dfeecc_173_0.png)

Figure 5.14 – Enabling the Weather supplementary feature

When you're ready to train a predictor, click on the **Create** button at the bottom of the predictor creation screen to start training a predictor with weather data.

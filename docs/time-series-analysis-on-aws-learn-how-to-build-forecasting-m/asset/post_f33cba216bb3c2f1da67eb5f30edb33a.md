
3. On this page, you can either click on the View dataset groups button on the right side of the screen or click the hamburger icon (the one with three horizontal lines) on the top left to open the left panel, where you can click on Dataset groups. You will land on a page listing all the dataset groups visible from your AWS account. If this is your first time using the service, your list will be empty.

![](img/e4ee48104c730845e796d8c469dfeecc_105_0.png)

Figure 3.19 – Amazon Forecast dataset groups list

4. Click on Create dataset group:

![](img/e4ee48104c730845e796d8c469dfeecc_105_1.png)

Figure 3.20 – Amazon Forecast dataset group creation screen

- 5. On this screen, you are just asked to give a name to your dataset group (let's call it `london_household_energy_consumption`) and a forecasting domain. As mentioned earlier, we are going to use the **CUSTOM** domain. Note that the domain is defined at the dataset group level. Every dataset associated with this dataset group later will have the same domain: you cannot mix a CUSTOM domain for a target time series and a **RETAIL** domain for the related time series.

- 6. Then, click **Next** to create your target time series dataset (the only one mandatory for the **CUSTOM** domain).

The dataset group will be created, and you will be brought to the target time series ingestion screen.

## Ingesting data in Amazon Forecast

We are going to ingest our three datasets one by one to populate a complete dataset group: a target dataset that we are going to forecast for, a related time series dataset, and an item metadata dataset.

## Ingesting your target time series dataset

At the end of the dataset group creation, you are automatically brought to the target time series dataset creation. If you stop the process after the dataset group creation and want to access this ingestion screen later from the Amazon Forecast home page, you just need to click on your dataset group name to view this dashboard.

## Dashboard Info

## Overview

## Import your data

Datasets are required to train predictors, which are then used to generate forecasts.

## Train a predictor

Train a predictor, a custom model with underlying infrastructure that Amazon Forecast trains on your datasets.

## Generate forecasts

Generate forecasts using your trained predictors.

## Explore insights

Explore and identify the attributes impacting your forecasts. Insights provide explainability to your forecasts.

Target time series data

Import

Item metadata data

Import

Related time series data

Import

Predictor training

Start

Forecast generation

Start

Insight generation

Explore insights

![](img/e4ee48104c730845e796d8c469dfeecc_107_0.png)

# Figure 3.21 – Initial dataset group dashboard

From there, you can click on the Import button next to the Target time series data label. To create your target time series dataset, you will need to feed in dataset details and the import details.

## Configuring dataset details

We are going to name our target dataset energy_consumption_data as it only contains the energy consumption for each household. The remaining parameters will be as follows:

![](img/e4ee48104c730845e796d8c469dfeecc_108_0.png)

Figure 3.22 – Target dataset details

When configuring your dataset, you must define the following:

- The *frequency* with which your data is recorded in your file. Frequency can be set with the two drop-down lists, one defining the unit of the interval (which can either be minute, hour, day, week, month, or year) and the number of units (only applicable when you select the minute interval). For instance, your time series can have data recorded every 15 minutes or every month. In our case, we are selecting 1 day.
- The *data schema*: This schema will initially be populated with the fields that are mandatory for the domain you selected when you created the dataset group. You can only configure the timestamp format, which can either be `yyyy-MM-dd HH:mm:ss` (example: `2021-05-19 07:00:00`) or `yyyy-MM-dd` (`2021-05-19`). If you chose hour or minute for the interval frequency, you must select the long format (`yyyy-MM-dd HH:mm:ss`). For our daily dataset, we are selecting `yyyy-MM-dd`.

On the drag-and-drop schema builder, you can reorder the fields by using the handles next to each attribute name.

> **Important Note**
> 
> The order of the fields *must match* the order of your CSV file.

You can add other attributes by clicking on the Add attribute button at the bottom of the schema builder. Each new attribute needs a name and a type.

![](img/e4ee48104c730845e796d8c469dfeecc_110_0.png)

Figure 3.23 – Adding an attribute with the schema builder

An attribute can take the following type: string, float, integer, or geolocation.
Geolocation is used by Amazon Forecast to match your location with the internal weather data feature. At the time of writing, the geolocation attributes can only accept locations in Europe, the US (excluding Hawaii and Alaska), Canada, South America, and Asia Pacific. To get an up-to-date list of geolocations supported by Amazon Forecast, you can refer yourself to the weather index documentation:

```
https://docs.aws.amazon.com/forecast/latest/dg/weather.html#geolocation-bounds
```

The geolocation format can either be a ZIP code (for US locations only) or a latitude/longitude decimal degree (for all locations):

- ZIP codes must be entered by specifying US_#####, followed by the five-digit ZIP code (US_98121). Only US ZIP codes are supported at the time of writing.

- The latitude/longitude must be a string following this format: `##.####_##.####` (for instance, `47.4922_-2.3639` will be used to store a location with a 47.4922 latitude and a -2.3639 longitude).

As an alternative to the schema builder, you can also select the JSON schema and specify all the attributes (mandatory and optional ones) in a JSON format. Note that the timestamp format must be configured in a separate control below the JSON specification.

![](img/e4ee48104c730845e796d8c469dfeecc_111_0.png)

Figure 3.24 – Specifying a schema in JSON format

For this tutorial, you can keep the default values and field order in the schema builder

## Configuring dataset import details

In the **Dataset import details** screen, you can fill in the following fields:

![](img/e4ee48104c730845e796d8c469dfeecc_112_0.png)

Figure 3.25 – Specifying the import details

You will need to do the following:

- Give a *name* to your import. This will help you distinguish between different import jobs. We will call it `energy_consumption_20210519` (I added a timestamp to know when I imported it).

> **Important Note**
> Note that *successive import jobs are not aggregated*: only the most recent import will be considered when you use your datasets to train a predictor.

- Enter the S3 URI for the *target time series* that you copied earlier when you uploaded the CSV file to Amazon S3.

- Select **Enter a custom IAM role ARN** and paste the ARN of the `AmazonForecastEnergyConsumption` role that you created earlier in the IAM service. The format of the ARN should be `arn:aws:iam::<ACCOUNT-NUMBER>:role/AmazonForecastEnergyConsumption`, where `<ACCOUNT-NUMBER>` needs to be replaced by your AWS account number.

Once you're done, you can click on **Start** to start the ingestion process. Your ingestion process begins.

![](img/e4ee48104c730845e796d8c469dfeecc_113_0.png)

Figure 3.26 – Target time series ingestion in progress

If you click on your dataset name, you can get the status with an estimated time remaining for ingestion. You also have the ability to stop or delete the ingestion if you made a mistake and don't want to wait for the process to finish before you can actually restart it.

## Ingesting related time series

Let's go back to our dataset group dashboard: we can see that our target time series data is active.

![](img/e4ee48104c730845e796d8c469dfeecc_114_0.png)

Figure 3.27 – Target time series ingested

> **Important Note**
> 
> In your case, the dataset ingestion might still be in progress, but you don't have to wait for an ingestion job to finish before starting another one. Several ingestion jobs can run in parallel.

From the previous screen, you can click on the **Import** button next to the **Related time series data** label. To create your related time series dataset, you will need to feed the same details as for the target time series dataset. We will use the following information to configure it:

- *Dataset name:* `london_weather_data` (as the related time series are weather data).
- *Frequency of your data:* 1 day.
- *Dataset import name:* `london_weather_20210519`.

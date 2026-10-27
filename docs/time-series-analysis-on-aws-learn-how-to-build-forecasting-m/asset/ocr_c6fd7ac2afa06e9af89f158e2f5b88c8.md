# Enabling the Holidays feature

This supplementary feature includes a dataset of national holidays for 66 countries. You can enable this feature when you create a new predictor: on the predictor creation page, scroll down to the optional Supplementary features section and toggle on the Enable holidays button, as illustrated in the following screenshot:

![](bbox=[0.13, 0.21, 0.86, 0.49])

Figure 5.12 – Enabling the Holidays supplementary feature

Once enabled, a drop-down list appears, to let you select the country for which you want to enable holidays. Note that you can only select one country: your whole dataset must pertain to this country. If you have several countries in your dataset and wish to take different holidays into account, you will have to split your dataset by country and train a predictor on each of them.

When this parameter is selected, Amazon Forecast will train a model with and without this parameter: the best configuration will be kept based on the performance metric of your model.

# Enabling the Weather index

This supplementary feature includes 2 years of historical weather data and 2 weeks of future weather information for 7 regions covering the whole world, as follows:

+   - US (excluding Hawaii and Alaska), with latitude ranging from 24.6 to 50.0 and longitude between -126.0 and -66.4

+   - Canada, with latitude ranging from 41.0 to 75.0 and longitude from -142.0 and 52.0
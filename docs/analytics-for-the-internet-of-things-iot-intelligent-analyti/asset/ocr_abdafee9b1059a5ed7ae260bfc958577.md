You will need GIS tools to open and process the data. These will be discussed in a later chapter. Having a full dataset with details about roads (name and type of road for example) and points of interest (restaurant or gas station for example) can be especially useful for transportation-related IoT data. Incorporating a full-scale dataset into your Hadoop environment, for instance, allows you to operate geospatial analytics at scale.

# Google Maps API

Location data from IoT devices can also be decorated as needed versus integrating a full dataset into your environment. Third-party APIs are a good way to do this while leveraging the expertise of other companies that specialize in geospatial analytics. There are many good ones available, including APIs from Google, Bing (Microsoft), Esri, and OpenStreetMaps.

Although mainly intended for web and mobile application developers, the web service APIs for Google Maps can be used to quickly add location identifiers, turn addresses into latitude and longitude coordinates, or calculate driving times between locations. The APIs can add many useful attributes to a dataset. Using them makes a lot of sense when your data record flow is in the tens of thousands per day versus tens of millions per day.

The following screenshot shows the variety of currently available Google Map APIs. Each API has its own set of functionality and options, putting a lot of power at your fingertips:

![](bbox=[0.1, 0.57, 0.92, 0.72])

Google Maps web service APIs. Source: Google

Leveraging the web service APIs to add attributes to IoT data can be done easily in R or Python. Google provides a Python Client Library to simplify the use for the programming language. There are packages available on CRAN, such as a googleway, that simplify Google Maps API calls for R.
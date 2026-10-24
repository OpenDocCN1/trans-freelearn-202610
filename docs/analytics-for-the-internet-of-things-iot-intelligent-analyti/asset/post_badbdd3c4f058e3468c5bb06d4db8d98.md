
NED is useful for higher-precision elevation data needs. The downside is that it is limited in coverage to the United States. The author recommends using SRTM when possible as long as the resolution is acceptable for your use case. The elevation values will differ between the datasets, but in general, the author has found them to be fairly comparable.

NED data can be downloaded using the map viewer tool at `https://viewer.nationalmap.gov/basic/`. The following screenshot shows the coverage of the NED 10 data. Data files are in GIS format, which we will discuss further in a later chapter. Files are also available for a direct download from an FTP site (`ftp://rockyftp.cr.usgs.gov/vdelivery/Datasets/Staged/Elevation/`). The following image shows the map and download interface:

![](img/9f2a55db1236f995dcd9392acbc82910_206_0.png)

National map viewer data download tool. NED 10 coverage shown in orange.

## Weather

There are many sources for weather-related data, both paid and free. The **National Oceanic and Atmospheric Administration (NOAA)** has multiple free datasets available for download. From climatic data to daily averages, there is a lot of useful information to choose from.

The Integrated Surface Global Hourly dataset can be useful for many IoT analytics projects. This dataset includes weather attributes such as air quality, atmospheric pressure, temperature, dew point, wind speed and direction, clouds, and precipitation from over 20,000 global stations. The digital observations are forwarded from various operation centers or decoded directly at the Federal Climate Complex in Asheville, North Carolina, USA.

All the data is stored in a single ASCII format that conforms to **Federal Information Processing Standards (FIPS)**. The data files are sorted by the weather station, year, month, day, hour, and then minute. An example of the dataset is shown in the following screenshot:

![](img/9f2a55db1236f995dcd9392acbc82910_207_0.png)

The dataset can be downloaded in multiple ways, including with a map viewer. You can also get data definition and other useful documentation from https://data.noaa.gov/dataset/integrated-surface-global-hourly-data.

If you know the latitude and longitude of your IoT devices, you can download the location information for the weather stations from the web page. This information is compiled in a KMZ file. Use this to link to the nearest weather station in order to add the weather attributes at the place and time of IoT data records.

## Geographical features

Elevation and weather conditions are valuable datasets to add when you know the location of your devices, but incorporating geographical features in relation to your device can also be a way to unlock hidden value through analytics. This becomes especially important when your device moves, such as when attached to a shipping container.

You will need GIS tools to open and process the data. These will be discussed in a later chapter. Having a full dataset with details about roads (name and type of road for example) and points of interest (restaurant or gas station for example) can be especially useful for transportation-related IoT data. Incorporating a full-scale dataset into your Hadoop environment, for instance, allows you to operate geospatial analytics at scale.

## Google Maps API

Location data from IoT devices can also be decorated as needed versus integrating a full dataset into your environment. Third-party APIs are a good way to do this while leveraging the expertise of other companies that specialize in geospatial analytics. There are many good ones available, including APIs from Google, Bing (Microsoft), Esri, and OpenStreetMaps.

Although mainly intended for web and mobile application developers, the web service APIs for Google Maps can be used to quickly add location identifiers, turn addresses into latitude and longitude coordinates, or calculate driving times between locations. The APIs can add many useful attributes to a dataset. Using them makes a lot of sense when your data record flow is in the tens of thousands per day versus tens of millions per day.

The following screenshot shows the variety of currently available Google Map APIs. Each API has its own set of functionality and options, putting a lot of power at your fingertips:

![](img/9f2a55db1236f995dcd9392acbc82910_209_0.png)

Google Maps web service APIs. Source: Google

Leveraging the web service APIs to add attributes to IoT data can be done easily in R or Python. Google provides a Python Client Library to simplify the use for the programming language. There are packages available on CRAN, such as a googleway, that simplify Google Maps API calls for R.

To start using Google Maps API, you will need to obtain a developer key to use in your API calls that authenticates the code and sets up billing. There are links on the Maps API website to help you set this up.

Here is a sample R code using the *googleway* package. You will need to install it first using the RStudio GUI or the code, `install.packages("googleway")`. Insert your Google Maps API key and run the code to get the time zone offset for the latitude and longitude coordinates:

```
```r
#Load googleway package (assuming it is already installed)
library(googleway)

#Get the timezone offset information for a pair of latitude and longitude coordinates
tz <- google_timezone(location = c(41.882702,-87.619392),timestamp = as.POSIXct("2017-03-05"),key = "< Replace everything between these quotes with your API Key >")

#View results
View (tz)
```
```

This is a very simple example, but shows how easily it can be used to do something that would be quite complex to develop by yourself.

## USGS national transportation datasets

The **United States Geological Survey (USGS)** national transportation dataset is a collection of datasets that includes railroads, trails, airports, roads, waterways, and other transportation-related geographical features. The data was provided through U.S. Census Bureau sources and supplemented with road data from a specialist mapping company, HERE.

Data not only includes the shapes of roads and railways but also tables of information about them. The coverage is limited to the United States but has standardized classification codes and descriptors. The dataset was compiled to support geospatial analysis related to traffic safety, congestion, disaster planning, and emergency responses.

## Decorating Your Data - Adding External Datasets to Innovate

The dataset is available to download at no cost in either Esri File Geodatabase or Shapefile format. The following screenshot is a subset of the available data tables in the dataset:

![](img/9f2a55db1236f995dcd9392acbc82910_211_0.png)

Partial view of USGS Best Practices data table definitions. Source: USGS

## External datasets - demographic

Another way to enhance the value of IoT data is to add demographic information based on time and location. Attributes such as healthcare coverage rate, average income in the area, and population ages could have predictive values when combined with your IoT data.

## The U.S. Census Bureau

For the U.S., the Census Bureau maintains demographic datasets down to the state and county level. Many other countries have similar governmental bodies with the same mission. There are multiple datasets available, but a great place to start is the QuickFacts dataset.

QuickFacts maintains statistics for all U.S. states and counties, along with cities and towns that have a population of 5,000 or more. It includes statistics on population size and density, age and sex, race, housing, education, economy, income, business, and health. The dataset uses the **Federal Information Processing Standards (FIPS)** code as the identifier. The FIPS code is normally what you would use to link to your IoT data.

The FIPS code is a five-digit identifier that identifies counties (or county equivalents) in the United States. The first two digits signify the state and the last three identify the county. A list of FIPS codes can be downloaded from `https://www.census.gov/geo/reference/codes/cou.html`.

The U.S. Census Bureau also maintains some useful geospatial datasets that have demographic information already integrated. Information about the **Topologically Integrated Geographic Encoding and Referencing (TIGER)** datasets is available at `https://www.census.gov/geo/maps-data/data/tiger.html`.

| Product | Best For... | File Format | Type of Data | Level of Detail | Descriptive Attributes | Vintages Available |
|---|---|---|---|---|---|---|
| TIGER/Line Shapefiles | Most mapping projects—this is our most comprehensive dataset. Designed for use with GIS (geographic information systems). | Shapefiles (.shp) and database files (.dbf) | Boundaries, roads, address information, water features, and more | Full detail (not generalized) | Extensive | 2006–2016, 2013 |
| TIGER Geodatabases | Useful for users needing national datasets or all major boundary files. Designed for use in ArcGIS. Files are extremely large. | Geodatabase (.gdb) | Boundaries, roads, address information, water features, and more | Full detail (not generalized) | Limited | 2013–2016 |
| TIGER/Line with Demographic and Economic Data | Data from selected attributes from the 2010 Census, 2006–2010 through 2010–2014 ACS 5-year estimates and County Business Patterns (CBP) for selected geographies. Designed for use with GIS. | Shapefiles (.shp) and Geodatabases | Boundaries, Population Counts, Housing Unit Counts, 2010 Census Demographic Profile 1 attributes, 2006–2010 through 2010–2014 ACS 5-year estimates data profiles, CBP data | Full detail (not generalized) | Limited | 2012 CBP, 2010, 2006–2010 to 2010–2014 ACS 5-Year Estimates |
| Cartographic Boundary Shapefiles | Small scale (limited detail) mapping projects clipped to shoreline. Designed for thematic mapping using GIS. | Shapefiles (.shp) | Selected boundaries | Less detail (generalized) | Limited | 2013–2015, 2010, 2000, 1990 |
| KML – Cartographic Boundary Files | KML — creating maps using Google Earth, Google Maps, or other platforms that use KML. | KML (.kml) | Selected boundaries | Less detail (generalized) | Limited | 2013–2015 |
| TIGERweb | Viewing spatial data online or streaming to your mapping application. | Interactive viewer, HTML data files, plus REST and WMS map services | Boundaries, roads, address information, water features, and more | Detailed | Extensive | Current, 2015 ACS, 2014 ACS, 2010 Census |

![](img/9f2a55db1236f995dcd9392acbc82910_212_0.png)

## CIA World Factbook

The Central Intelligence Agency (CIA) of the United States maintains a free, useful, no-strings-attached (that we know about) database of information about the countries of the world. Despite the reputation of daring spywork, CIA also has the comparatively boring role of compiling useful information about places all over the world.

The agency makes some of this information available to anyone that can use a web browser. If anyone from the agency is reading this book (hopefully through a purchase and not directly from the author's laptop - but we kid, we kid), the author would like to thank you for making this great source of global information available for free. It is available as public domain, which means no worries about copyright or reserved publication rights issues.

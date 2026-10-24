
This chapter is about dramatically enhancing value by adding additional datasets to the stored IoT data. Valuable additions come from both internal sources, such as manufacturing or **Customer Relationship Management (CRM)** data, and external data sources, such as economic or demographic datasets. You will learn how to look for valuable datasets and combine them to enhance future analytics in the search for untapped business value.

This chapter covers the following topics:

- General strategies on extracting value from combining datasets
- Internal datasets:
  - Which ones and why?
- External datasets - geography:
  - Elevation
  - Weather
  - Map APIs
  - Transportation
- External datasets - demographic:
  - Census statistics
  - World factbook
- External datasets - economic:
  - Global economic data
  - Federal Reserve US time series
  - Using code and APIs to add data

## Adding internal datasets

IoT data by itself is only part of the story. There is a multitude of useful data already available to you that could be a store of hidden value. Internal datasets can be overlooked as a way to quickly enhance IoT data. They are an excellent place to start. IoT data should not be viewed in isolation; think of it as a continuation of data already stored about your company's products, customers, and processes.

Data should be combined into a 360-degree view of your business to maximize the opportunity of finding new value in it. The fastest and easiest place to start is usually (but not always) the internal datasets that you already have available. The reason it may not always be the fastest and easiest comes from internal data security and legacy system hurdles; which can sometimes make it difficult to extract internal data to combine with IoT data. This may be a stumbling block for internal datasets, which, perhaps counter-intuitively, is not the case with external datasets. External datasets tend to be easier to obtain and have less security concerns, since they are publicly available. Either way, the potential value is still worth the effort of overcoming these hurdles.

### Which ones and why?

When deciding on which internal datasets to integrate with IoT data, think about which ones would help explain unusual changes in sensor values or would correlate with patterns in the data. These datasets are more likely to help uncover business value. There are many internal company datasets that will help you unlock value; we will discuss four areas to consider next.

#### Customer information

As obvious as it may be, it bears pointing out how important incorporating information about the customer is to uncovering value from your IoT data. Information such as customer address can be geocoded to get the latitude and longitude coordinates. This can be used to establish a device location, which will allow you to build another layer of analytics.

Understanding things such as sex, age, or income level can help identify patterns not visible otherwise. For industrial customers, knowing industry and application of the equipment where your device is located can be the key that unlocks hidden patterns in the data. For these and many other reasons, customer information should be at the top of the list of internal datasets to combine with IoT data.

#### Production data

Production information about the equipment that is the source of your IoT data is also valuable to throw into the mix. Failures are often linked to the manufacturing date, plant, and line where the equipment was assembled. Subcomponents and third-party parts are tied to the equipment identifier.

Production data can also be used to estimate time in the field if that is not directly available. This can be done by using manufacturing date as a starting place and estimating time to sale and in field operation. Different equipment configurations may also end up being key attributes that you can use to make assumptions for other data that you may not have. For example, if a company builds vehicles and red flashing lights are only included on vehicles built as fire trucks, you can probably classify the customer of these vehicles as fire departments even if you do not have this information directly in the customer data.

#### Field services

Bringing in datasets from technician repair visits, maintenance records, and call center logs is another great place to start tapping value from your data. There will be key events such as repair work or complaint calls that can be linked to unusual patterns in IoT data.

Maintenance records will be useful in developing predictive maintenance models. It may also tie into equipment failure predictive models. Call center logs could be mined to develop attributes of IoT data events beyond just reported numerical or categorical values. **Natural Language Processing (NLP)** can be used to extract attributes from text data.

For a utility company that provides natural gas, for example, the phrase *rotten egg* in call log texts, corresponding to a pattern in the IoT data for low gas pressure, could be a tip off that a gas leak is occurring. Analytics could then be developed to recognize the pattern in future IoT data records for proactive investigation.

#### Financial

The difference between revenue and cost is profit. The anticipation of profit is the driver for most investments in IoT systems. Linking customer payments (revenue) and company expenses (costs) to IoT datasets can shed light on potential value opportunities.

You may find that customers spend money on your products after certain events identifiable in IoT data. This could be extended warranty purchases or service parts. You may also find that they do not spend money when you would expect they should. An example is IoT data indicating part failures but with no corresponding part purchased by the customer. This implies the service parts were bought from someone else. You will want to identify both situations.

## Adding external datasets

There is a wide variety of useful external datasets available to download and integrate into your IoT analytics, often for free. There are far too many of these to do an exhaustive tour, but we will touch on some useful examples. Using these datasets is a great low-cost way to enhance the value of your IoT data.

### External datasets - geography

You have IoT devices distributed across many locations. Location is now an attribute that can allow you to find patterns in the data, which you can then monetize. The environment, the demographics in the area, the local economic conditions, and the weather can all be tied to the geographic location of the device. Use geographic datasets to enhance the potential value of your IoT data.

#### Elevation

Elevation can be an important attribute in many situations as it is tied to the operating conditions of the device and the surrounding environment. Even if your device does not move, it can still be valuable to know its elevation. The maker of a smart oven, for example, would surely want to know elevation as it affects baking time and characteristics.

##### SRTM elevation

In February 2000, the U.S. Space Shuttle Endeavour was launched, specially equipped with two radar antennas on a mission to map the topography of most of the globe. As depicted in the following image, one of the radar antennas was inside the shuttle bay and the other was at the end of a 200 foot (60 meter) mast. The mast was extended once the shuttle was in orbit. The combined radar swept the earth as the shuttle orbited in a pattern over the globe.

![](img/9f2a55db1236f995dcd9392acbc82910_204_0.png)

The artist rendering of SRTM in operation. Courtesy NASA/JPL-Caltech

The mission was called the **Shuttle Radar Topography Mission (SRTM)** and in its 10 days of operation, it collected the most complete near-global dataset of high-resolution elevation mapping to date. This was an international mission led by the National Geospatial-Intelligence Agency (NGA) and the National Aeronautics and Space Administration (NASA).

The data was captured using two sets of radar equipment: one referred to as C-band and the other as X-band. The C-band data was processed over two years by the Jet Propulsion Laboratory in California. The slightly higher resolution X-band data was processed by German Aerospace Center (DLR). Global data was fully released in 2015.

The resolution of the data is at 90 meters globally and 30 meters within the United States. The dataset can be downloaded from the **U.S. Geological Survey (USGS)** website (`https://lta.cr.usgs.gov/SRTM`). There are also R and Python packages (*SRTM.py* on GitHub is an example) built to handle downloading and manipulation of the SRTM dataset. The author recommends using one of these instead of the full dataset, unless you have a very large set of locations that you want to add elevation data for all at once.

##### National Elevation Dataset (NED)

The NED is a high-resolution elevation dataset covering the United States, including Alaska, Hawaii, and territorial islands. It is managed by the USGS, which updates it about every 2 months to include new and improved data. The data is assembled from a diverse set of sources and transformed to a common coordinate system and elevation scale (meters).

The resolution ranges from 90 meters down to 3 meters (1/9th arc-second) in some areas. Processing and interpolation techniques are used to produce a seamless dataset for full coverage. The size of the total set of data files is quite large; the USGS has a map viewer to help identify regions to download the subsets of the data.

NED includes the following sets of products:

- **NED 3**: This data has the highest resolution (1/9 arc-second, which is approximately 3 meters) but has limited coverage.
- **NED 10**: This data has high resolution (1/3 arc-second, which is approximately 10 meters) and is created from multiple **Digital Elevation Models (DEM)**. Sources can include LIDAR and aerial photography.
- **NED 30**: This dataset has wide coverage and moderate resolution (1 arc-second, which is approximately 30 meters). It is assembled from multiple DEMs resampled to 1 arc-second.
- **NED 60**: This is the lowest resolution data (2 arc-seconds, which is approximately 60 meters). It adds coverage for some areas of Alaska.

For London household energy consumption, each time series is associated with a household that could be further associated with its house size, the number of people, its type (house or flat), the construction time, the address, and so on. The following screenshot lists some of the metadata associated with a few households from this dataset: we can see, for instance, that 27 households fall into the ACORN-A category that has a house with 2 beds:

|  | MAIN CATEGORIES | CATEGORIES | REFERENCE | ACORN-A | ACORN-B | ACORN-E |
|---|---|---|---|---|---|---|
| 668 | SHOPPING | Furniture & Fittings Stores | Premium | 214.000000 | 209.000000 | 139.000000 |
| 778 | LEISURE TIME | Interests & Hobbies | Charity / Voluntary Work | 122.000000 | 122.000000 | 46.000000 |
| 47 | HOUSING | House Size | Number of Beds : 2 | 27.000000 | 28.000000 | 124.000000 |
| 182 | MARKETING CHANNELS | Future Responses | Messages on mobile phone | 105.000000 | 105.000000 | 126.000000 |
| 159 | MARKETING CHANNELS | Channels Received | Leaflets - Door or Newspaper | 102.000000 | 105.000000 | 98.000000 |
| 51 | HOUSING | House Value | House Value up to 100k | 13.000000 | 17.000000 | 48.000000 |
| 175 | MARKETING CHANNELS | Future Responses | Leaflets/Samples handed out in the street or p... | 101.000000 | 101.000000 | 113.000000 |
| 148 | TRANSPORT | Public Transport Accessibility Level | 6a | 101.394793 | 8.309678 | 176.905736 |
| 285 | DIGITAL | Types of internet usage : Laptop or PC | Vote on television shows (e.g. X Factor, St... | 72.000000 | 88.000000 | 84.000000 |
| 38 | HOUSING | House Type | Semi-detached house | 56.000000 | 70.000000 | 66.000000 |

Figure 1.6 – London household metadata excerpt

Now you have understood how time series can be further described with additional context such as labels, related time series, and metadata, let's now dive into common challenges you can encounter when analyzing time series data.

# Learning about common time series challenges

Time series data is a very compact way to encode multi-scale behaviors of the measured phenomenon: this is the key reason why fundamentally unique approaches are necessary compared to tabular datasets, acoustic data, images, or videos. Multivariate datasets add another layer of complexity due to the underlying implicit relationships that can exist between multiple signals.

This section will highlight key challenges that ML practitioners must learn to tackle to successfully uncover insights hidden in time series data.

These challenges can include the following:

+   - Technical challenges
+   - Visualization challenges
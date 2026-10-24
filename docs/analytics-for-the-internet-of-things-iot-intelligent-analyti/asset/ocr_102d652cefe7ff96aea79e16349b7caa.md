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

![](bbox=[0.104, 0.398, 0.894, 0.551])

# CIA World Factbook

The Central Intelligence Agency (CIA) of the United States maintains a free, useful, no-strings-attached (that we know about) database of information about the countries of the world. Despite the reputation of daring spywork, CIA also has the comparatively boring role of compiling useful information about places all over the world.

The agency makes some of this information available to anyone that can use a web browser. If anyone from the agency is reading this book (hopefully through a purchase and not directly from the author's laptop - but we kid, we kid), the author would like to thank you for making this great source of global information available for free. It is available as public domain, which means no worries about copyright or reserved publication rights issues.
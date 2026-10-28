Let’s look at how fields look in the Splunk search results in the following figure:

![](bbox=[0.125, 0.105, 0.86, 0.335])

Figure 11.1: Fields in Splunk search results

The preceding figure shows how the fields appear in Splunk, which is showing default fields, a custom extraction field (method = GET), an auto-extracted field (namespace = search), and a data type symbol (a for alphanumeric and # for numeric).

# Lookups

Lookups are loaded into Splunk from external sources to enrich the data with additional information. The most used lookups are structured contents in the format of CSV files and the KV Store (processed by MongoDB). Popular Splunk Search Processing Language (SPL) commands to read the lookup files are input lookup and lookup. Splunk supports the following types of lookups. We are going to learn about CSV and KV Store-backed lookups in this chapter:

+   - Geospatial lookups
+   - External lookups
+   - CSV lookups
+   - KV Store lookups

Geospatial lookups contain geospatial information such as the longitude and latitude related to a place in the world. By default, Splunk ships with US information and worldwide countries’ information. You can issue a search request in the Search and Reporting app - | input lookup geo_countries - and click on the Visualization tab, and then choose Choropleth Map.
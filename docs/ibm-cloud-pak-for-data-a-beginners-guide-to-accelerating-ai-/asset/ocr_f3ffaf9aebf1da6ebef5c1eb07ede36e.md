With Cloud Pak for Data, we have a centralized governance catalog that acts as the index of all our data assets and helps us organize resources for many data science projects: data assets, analytical assets, and the users who need to use these assets. This catalog also has built-in profiling capabilities that allow the consumers to select a given dataset and initiate a quick data profiling activity. The focus is on the following two aspects of profiling:

+   - Data structure discovery; that is, what attributes are available in the dataset?
+   - Data content discovery; that is, what are the data values, their distribution, quality, and so on?

The first step is to define a connection to the data source and onboard the data asset to the catalog. Cloud Pak for Data supports a wide array of data source connectors, and this list of supported connectors is growing with every new version of the platform.

Once a data asset has been onboarded, we can use the catalog to do the following:

+   - Tag the asset with one or more tags.
+   - Get a quick preview of the data.
+   - Add reviews and comment on the asset.
+   - Define access policies for the asset.
+   - Track the event lineage of the asset.
+   - Create a data profile:

![](bbox=[0.119, 0.573, 0.851, 0.803])

Figure 4.4 – Initial profiling of catalog assets
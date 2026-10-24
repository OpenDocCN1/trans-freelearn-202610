Your results should look similar to the following image:

![](bbox=[0.176, 0.176, 0.824, 0.559])

# Summary

In this chapter, we produced a dynamic web application using a physical raster analysis example: hydrological analysis. To do this, we started by preparing the raster elevation data for the hydrological analysis and then performed the analysis. We took a look at how we could automate that workflow using the Modeler workflow automation tool. Next, we used NNJoin to create a spatial join between some hydrological outputs to produce a data source that would be suited to web interaction and querying. Finally, we published this data to an external cloud platform, CartoDB, and implemented their SQL API in a JavaScript function to find the toxic sites upstream from a location, given the Leaflet web client interaction. In the next chapter, we will produce a web application using network analysis and crowd sourced interaction.
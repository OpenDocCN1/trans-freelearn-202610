In the preceding code, the data now contains the exact same object that we saw in the preceding example. Now, we only have to extract the relevant information from the data. We obtained the desired properties using a map function:

```
d3.json("data/default.json", function(error, data) {
  if (error) throw error;
  var labels = data.features.map(function(d) {
    return d.properties.prop0;
  });
  var values = data.features.map(function(d) {
    return d.properties.prop1.count;
  });
});
```

In the preceding code, we use map to transform and extract the relevant properties, such as `values` and `labels`, from the feature collection's `properties` attribute. Note that the shape of the properties object is not standardized and depends on the data structure of your API. In our case, it contains the `prop0` and `prop1` attributes, but it could also contain other attributes. If one of the properties would contain a nested objected, we could also use the `reduce` function to aggregate these values.

It is important to know that we always have to extract the values out of the properties object in order to visualize them.

# TopoJSON – GeoJSON for web visualizations

TopoJSON is a format optimized for compressed GeoJSON topology data specified and developed by Mike Bostock, the main author of `d3.js`. It was developed to reduce the size of big GeoJSON files in order to speed up web visualizations.

As we saw in the previous section, the GeoJSON format stores a topology as a list of polygon points; however, any in-land border often belongs to multiple countries or states. Thus, encoding the states' bounding boxes as polygon points leads to (a lot of) redundant information. According to Mike Bostock, a TopoJSON file is 80% smaller than the original GeoJSON file:
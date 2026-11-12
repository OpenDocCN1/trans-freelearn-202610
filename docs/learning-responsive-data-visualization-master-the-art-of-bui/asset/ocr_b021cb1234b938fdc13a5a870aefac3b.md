If you are interested in creating beautiful color scales, you should take a look at the Color Brewer library at http://colorbrewer2.org/.

If you want to display the color scale next to the visualization, you can use the great d3 color legend plugin by John Goodall at https://github.com/jgoodall/d3-colorlegend.

# # The geographic shape generator

You have already learned about the different types that are supported in the GeoJSON format, each one defines a specific geometry. When dealing with these GeoJSON features, we often have the problem of drawing these shapes onto a map using a certain predefined projection. However, I should send flowers to Mike Bostock for developing the geographic shape generator which can be accessed via d3.geo.path().

The D3 geographic shape generator makes it trivial to plot GeoJSON features on a map. It can draw any of the supported GeoJSON types and output the geometry according to a predefined projection as a string for SVG path elements.

Let's take a look at how to use this generator:

```
var projection = d3.geo.albersUsa();
var path = d3.geo.path()
whitespace ".projection(projection)"
svg.append("path")
      .datum({type: "FeatureCollection", features: features})
      .attr("d", path);
```

In the preceding code, we generated a path for a FeatureCollection object with a fictive array of feature. Note that this is the exact GeoJSON format that makes it super simple to plot GeoJSON features.

# ## Graticule – grids on maps

An essential tool to understand maps and projections is a grid in cartographic visualizations. Grids are essential because other than in Cartesian coordinate systems, grids of parallels and meridians are no longer equally spaced or normal onto each other. Hence, the graticule carries a lot of useful visualization about the current projection and geographic coordinate system.
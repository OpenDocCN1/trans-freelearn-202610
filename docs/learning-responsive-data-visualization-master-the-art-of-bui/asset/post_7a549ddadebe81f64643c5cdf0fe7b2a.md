
```html
<script src="bower_components/topojson/topojson.js"></script>
<script src="bower_components/queue-async/queue.min.js"></script>
<style></style>
</head>
<body>

<script>

var padding = 10,
  body = d3.select("body"),
  width = body.node().clientWidth - padding,
  aspectRatio = 16.0 / 9.0,
  height = width / aspectRatio;

var svg = d3.select("body").append("svg")
  .attr("width", width)
  .attr("height", height);

// The visualization code goes here
</script>
</body>
</html>
```

In the preceding code, we get the size of the browser window using the `clientWidth` property and define an aspect ratio of 16/9 for the map. We will also use this setting for the other examples.

First, we need to create a projection and draw the map; we will translate and scale the projection to show the map on the full width of the container (note that the scale is interpreted differently in different projections):

```
var projection = d3.geo.albersUsa()
  .scale(width)
  .translate([width / 2, height / 2]);
```

Next, we can also create the path generator using the projection:

```
var path = d3.geo.path()
  .projection(projection);
```

Let's use a simple linear color scale from orange to red values:

```
var color = d3.scale.linear()
  .interpolate(d3.interpolateRgb)
  .domain([0.1, 5])
  .range(['#FFCC66', '#FF0000']);
```

Great, let's load the map and the data and then draw the map. To do so, we will use the TopoJSON format of the US map provided by Mike Bostock at https://gist.github.com/mbostock/4090848:

```
queue()
.defer(d3.json, "data/us.json")
.defer(d3.json, "data/earthquakes.json")
.await(ready);
```

Once the resources are available, the ready function gets called. Let's implement it:

```
function ready(error, us, earthquakes) {
  if (error) throw error;

  svg.append("g")
    .attr("class", "counties")
    .selectAll("path")
    .data(topojson.feature(us, us.objects.counties).features)
    .enter().append("path")
    .attr("d", path);

  svg.append("path")
    .datum(topojson.mesh(us, us.objects.states))
    .attr("class", "states")
    .attr("d", path);
}
```

In the preceding code, all the data of the deferred functions gets passed as arguments in the order of calling the functions. The US map is stored in TopoJSON format. Therefore, we need to transform it back to valid GeoJSON. We do this using either the topojson.feature() or topojson.mesh() function. First, we will draw a path for every county in the us.objects.counties collection. Next, we will draw one single mesh for all states in the us.objects.states collection.

Finally, we are only missing the dots; let's draw them:

```
svg.append("g")
  .attr("class", "data")
  .selectAll('circle')
  .data(earthquakes.features)
  .enter().append('circle')
  .attr('r', function(d) {
    return Math.abs(d.properties.mag);
  })
  .attr('fill', function(d) {
    return color(Math.abs(d.properties.mag));
  })
  .attr('transform', function(d) {
    var pos = projection([d.geometry.coordinates[0], d.geometry.coordinates[1]]);
    if (pos) {
        return "translate(" + pos + ")";
    }
  });
```

The earthquake data is formatted as GeoJSON and available in the earthquake.features object. We draw one circle for each element in the collection and adapt the radius size and color of the shape. Finally, we do some error checking and translate the circle shape into its proper position on the map. We use the projection function to achieve this. That's it, the final result is displayed in the previous figure.

## Choropleth maps

In the next map, we want to display unemployment rates per county in the US; the dataset shows unemployment rates per county ordered by county id, taken from Mike Bostock at https://gist.github.com/mbostock/4060606. The result of the map is shown in the following figure:

![](img/72559bd4713b2b89631dcca187d7e2db_195_0.png)

A simple Choropleth map

We can see in the previous figure that we want to color each county according to its unemployment rate. We use the same template (same dimensions and projection) as in the previous example.

Let's create a color scale and load the map and the data; this step is quite similar to the previous example:

```
var color = d3.scale.linear()
.domain([0, .15])
.interpolate(d3.interpolateRgb)
.range(["#FF7948","#B93200"]);
queue()
.defer(d3.json, "data/us.json")
.defer(d3.csv, "data/unemployment.csv")
.await(ready);
```

Now, we can draw the map:

```
function ready(error, us, unemployment) {
 if (error) throw error;
 var data = {};
 unemployment.forEach(function(d) {
 data[d.id] = +d.rate;
 });
 svg.append("g")
 .attr("class", "counties")
 .selectAll("path")
 .data(topojson.feature(us, us.objects.counties).features)
 .enter().append("path")
 .attr("fill", function(d) { return color(data[d.id]); })
 .attr("d", path);
 svg.append("path")
 .datum(topojson.mesh(us, us.objects.states))
 .attr("class", "states")
 .attr("d", path);
}
```

In the preceding code, we first transform the unemployment array into an object that stores the unemployment according to the county ID as properties. Then, we draw the counties and fill in the color according to the unemployment rate.

## Summary

In this chapter, you learned all the necessities to draw cartographic visualizations with D3. You learned about the geographic coordinate system, the graticules, and color scales.

GeoJSON is an important format to encode geometric features; it can store different types of geometries and values. However, it also stores a lot of redundant information; that's where TopoJSON comes handy.

Finally, we start drawing different cartographic visualizations, such as symbol maps and Choropleth maps.

In the next chapter, you will learn about testing the responsive visualizations.

## Testing Responsive Visualizations

In the previous chapter, you learned how to create amazing cartographic visualizations using D3.js, projections, and geographic maps. Now, it is time to look into the testing strategies of responsive visualizations. In this section, you will learn the following:

- Learning about common testing strategies
- Understanding the difference of unit and integration tests
- Learning about the importance of manual tests in visualizations
- Learning how to set up an environment for manual testing
- Setting up an environment for Unit and Integration testing
- Running unit tests using Karma on your local machine
- Running integration tests using TravisCI and Sauce Labs using a testing environment
- Understanding how the different strategies work together
- Building a testing strategy that fits your budget

In the first section, you will learn about testing, testing strategies, and different types of tests. You will understand the differences between manual and automated testing and why it is crucial to have great tools in your workflow for manual testing.

In the next section, we will take a closer look at these manual testing and debugging tools. We will find out how to use Browsersync, Browserstack, Device Modes, and much more to boost up your local development environment.

Then, we will discuss automated tests, an important part of writing maintainable code. We will not only look into some local unit testing with Karma but also integration testing using Protractor, Selenium, Travis CI, and Sauce Labs.

Finally, we will summarize how a great development environment should look when you care about testing and code quality.

## A guide on testing visualizations

At the end of this chapter, you will have a clear idea about testing strategies for responsive visualizations; you will be aware about the costs and benefits of testing; and you will be able to implement both manual and automated tests with the best tools that are available today.

If you are new to software development, you may have heard about testing and testing strategies and how many developers pretend that they are obsessed about testing. However, it is very important to understand the differences between, for example, test-driven development in software development and manual testing and debugging, in order to compare the costs and benefits for these strategies. If you are already writing tests on a daily basis and have a clear strategy about unit and integration tests, you might skip this introduction and jump directly to the next section. For everyone else, I promise that you will learn some great insights and information about testing, so go ahead!

When testing interactive visualizations, we are somehow in a very difficult domain. On the one hand, we write code, so we need to make sure that all functions of the code do exactly what they should. However, on the other hand, we produce visualizations that should look and feel the same across all different devices, operating systems, and browsers—something that can only be evaluated manually. The goal is to combine the best of manual testing and automated tools to maximize productivity and maintainability and minimize the number of errors in the code.

> In this chapter, I will mention tools and services that may require a paid subscription. My intention is not to sell you a subscription to one of these services but to present the best tools for manual and automated cross-browser testing from my experience.

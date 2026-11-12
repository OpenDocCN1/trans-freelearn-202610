# # Drawing arcs

The third complex shape that is often used in visualizations is an arc. D3 also provides a generator function for this to easily draw arcs with polar coordinates. Again, if you have ever dealt with arcs in SVG, you will probably be very glad to know this generator.

Let's create an arc generator:

```
var arc = d3.svg.arc()
.innerRadius(40)
.outerRadius(100)
.startAngle(0)
.endAngle(Math.PI);
```

In the preceding code, we see that we can define inner and outer radius as well as the start and ending angles of the arc in radiant. Similarly to the previous example, we can now generate the d attribute for a path element:

```
$svg.append("path")
.attr("d", arc)
.attr("transform", "translate(200, 200)");
```

In the previous example, we observe that we need to translate the arc a little bit in order to make it visible. The output of the example in the web browser is displayed in the following figure:

![](bbox=[0.153, 0.511, 0.883, 0.803])

Output of an arc generator

D3 provides more useful methods on the generator functions, for example, the arc.centroid() method. This function returns the center of the arc shape and suits perfectly to display labels with an arc.
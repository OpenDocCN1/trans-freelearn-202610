Perfect! This is already our line generator. The advantage is that we only have to define the structure of the line generator, that is, where in the dataset it can find the x and y coordinates, and we can then draw any dataset that follows the same structure. Let's draw the line; we need to use the selection.datum() method to bind one specific datum to the selection. In contrary to the previously discussed data joins, this function will not compute a join but only returns a selection. Then, we can apply the line generator to the d attribute of the path, which will implicitly look for the bound data and creates the proper d string:

```
$svg.datum(data)
.append("path")
.attr("d", line);
```

The following figure shows a result of this example in the web browser:

![](bbox=[0.07, 0.32, 0.9, 0.64])

Output of a simple line generator

In the previous example, you learned how to create a simple line generator that only connects the points of the dataset together. We can now easily define an interpolation method on the line generator in order to draw a smooth line:

```
var line = d3.svg.line()
.x(function(d, i) { return d[0]; })
.y(function(d, i) { return d[1]; })
.interpolate('cardinal');
```
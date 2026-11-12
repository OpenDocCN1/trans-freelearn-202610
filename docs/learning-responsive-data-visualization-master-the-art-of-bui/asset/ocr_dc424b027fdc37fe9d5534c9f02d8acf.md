The previous figure shows that everything looks good so far. Great, we are almost there. Finally, we can start drawing the bars in our chart. Before we start drawing bars, we need to define the width of our bars as the maximal width divided by the number of bars:

```
var barWidth = (width - 2*pad) / (data.length + 1);
```

Now, we can create a data join for the bars using the x value of the dataset as the identifier:

```
var $bars = $dataGroup.selectAll('rect')
.data(data, xKey);
```

We can go ahead and create the rect node for all elements in the enter set:

```
$bars.enter()
.append('rect');
```

Next, we move the rectangles to their proper position in the chart by simply using the scale function on the values of the dataset:

```
$bars
.attr('x', function(d) { return xScale(d.label); })
.attr('y', function(d) { return yScale(d.y); })
.attr('height', function(d) { return yScale(0) - yScale(d.y); })
.attr('width', barWidth);
```

Please note that, due to the SVG coordinate system, we need to define the y values and height of the bars by drawing them from the top to the bottom. Finally, for completeness, we also add the last step of our data join and remove all the elements from the exit set:

```
$bars.exit()
.remove();
```

In this example, we will never remove bars from the chart, but it is good practice to include the preceding lines to your code. This will become very useful once you put the code in a function and call it multiple times.

Let's make a slight adjustment to the chart to better stick to the responsive visualization topic. As a general rule, one can say that every time we define static pixel values, the result will look bad on some or the other device. So, what can we change? Well, never use explicit static pixel values but relative values extracted from the parent element.
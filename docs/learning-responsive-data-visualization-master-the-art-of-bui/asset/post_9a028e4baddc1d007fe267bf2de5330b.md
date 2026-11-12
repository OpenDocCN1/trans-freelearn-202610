
.ticks(data.length);

```
var yAxis = d3.svg.axis()
  .scale(yScale)
  .orient('left');
```

After creating the axis generators, we can already draw the axis. We can do this by simply passing the axis generator to the .call() method on a Selection of an empty container. We also need to shift the axis to the proper position in the chart. The x axis should be at the bottom and the y axis at the left:

```
$axesGroup.append('g')
  .attr('class', 'x axis')
  .attr('transform', 'translate(0, ' + (height - pad) + ')')
  .call(xAxis);
```

```
$axesGroup.append('g')
  .attr('class', 'y axis')
  .attr('transform', 'translate(' + pad + ')')
  .call(yAxis);
```

Let's look at the output of the preceding code in the browser; we will be able to see the axes:

![](img/72559bd4713b2b89631dcca187d7e2db_73_0.png)

Axes of the chart

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

If we follow this strategy, we can use the width of the body element for our chart width. All other values (the height and padding) can be defined relative to the width of the chart:

```
var $body = d3.select('body');

// Define the dimensions
var width = $body[0][0].clientWidth;
var height = width * 0.5;

// Define the padding
var pad = width * 0.1;
```

If we replace the old dimensions with these new computed dimensions, the chart will look good on every device that opens the web page. Great! I am very happy about how far you have come in this chapter; you can be very proud of yourself. You build your first (almost) responsive bar chart!

## Summary

In this chapter, you learned how to select DOM elements and do data-driven transformations using data joins and dynamic properties. We also saw how to use shape and axis generator functions to quickly create common shapes such as lines or areas.

In the end of this chapter, we built our first simple (almost) responsive bar chart. The bar chart already adapts to the width of the browser window on the page load, but it doesn't get recomputed when we change the browser dimensions.

In the next chapter, we will take a look at loading, filtering, and grouping data. This is very important because almost always we will have to load remote data in a certain format (such as CSV) and then process, filter, or group it afterwards in order to visualize it.

## Loading, Filtering, and Grouping Data

In the previous chapter, you learned to draw a simple bar chart. In this chapter, you will learn to load, parse, and display real data from a remote location with this chart. In this chapter, you will learn the following:

- Why we need to preprocess our data
- How to filter outliers
- How to map data to another representation
- How to aggregate information
- How to load data using D3
- How to parse strings using Regular Expressions
- How to parse date strings
- How to format numbers
- How to group flat data

First, we will start with some basic preprocessing steps that are always necessary. We will use the `filter` function to filter outliers and unexpected values from the dataset; we will also use the map function to access inner properties of array elements.

Then, you will learn about reduce functions and how they can help in aggregating and extracting information out of the data. We will implement our own simple reduce function and also take a look at the ones provided within D3.

Next, you will learn how to load data from remote locations. Spoiler alert, this will be very easy with D3. However, we need to make sure that the loaded data can be understood by JavaScript, and therefore, we sometimes need to parse the files. You will learn to use Regular Expressions to split strings into chunks, which can be accessed as arrays via JavaScript.

We will then take a look at parsing and formatting date, time and numeric values. This is very important because, when we deal with dates in text files, they are always represented as strings rather than JavaScript objects.

In the last section, you will learn about grouping flat data structures, such as CSV, that contain hierarchical information to nested JavaScript objects. Together, with the previous section, we will be able to extract any information that we want out of a dataset and plot it in a chart.

### Preprocessing data

In this chapter, you will learn about preprocessing and that it is an important step when you deal with real-world data. If we want to design a robust visualization that can handle different remote data sources, we need to make sure we process the data beforehand. Datasets can have outliers, can be noisy, can have different representations, can have nested objects, can be flat, and so on. You can see that this list is very long.

Thus, whenever we deal with real data, we need to process it before using it in a visualization; this step is called preprocessing.

### Filtering data to remove outliers

As a first preprocessing step, we will look at filters. Filters are array operators that return an array containing a subset of the original elements. In the following figure, we see an example of such an operation:

![](img/72559bd4713b2b89631dcca187d7e2db_77_0.png)

Let's try an example. We load the following array from a remote data source:

```
var data = [
 {x: 1, y: 6},
 {x: 2, y: undefined},
 {x: 3, y: 4},
 {x: 4, y: 2},
 {x: undefined, y: 3},
 {x: 5, y: 3},
 {x: 6, y: 7},
 ];
```

We see that the preceding dataset contains some values that are undefined. Whenever we work with real data, we should expect that some data values are not defined or properly formatted. If we want to remove these values from our array, we can use the array.filter(filterFn) operator. The filter function called filterFn will be evaluated for each element in the dataset and needs to return either true or false. Here, false means that the element will not appear in the returned array. Let's write a filter function that returns only valid objects:

```
var filtered = data.filter(function(d, i){
 if (d.x !== undefined && d.y !== undefined) {
 return true;
 }
 return false;
});
```

In the previous code, we see a typical filter function, which returns true if d.x and d.y are defined and false otherwise. However, we can write this more elegantly and simply return the if expression:

```
var filtered = data.filter(function(d, i){
 return d.x !== undefined && d.y !== undefined;
});
```

> Be careful with implicit type coercions when dealing with numerical values. If you write d.x && d.y instead, you will implicitly cast the numbers to Boolean values. And guess what kind of Boolean you'll get when you cast a numerical 0. Well, you get false.

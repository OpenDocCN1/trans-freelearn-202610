
## Parsing and formatting numeric values

We will not only have to deal with date and time strings but also with numeric values. In the case of integer and float values, we can use native functions to parse strings and D3 number formatters for formatting and styling them.

Integers and floats can be parsed easily by the `parseInt()` and `parseFloat()` functions. However, sometimes it is convenient to use the short `+` notation to cast strings to numeric values.

```
// Parsing strings to numbers
var i = parseInt("5.0", 10);
var f = parseFloat("3.8");
```

```
// Short notation
var s = "932.2";
var j = +s;
```

In the preceding code, we see that it's very convenient to use the native functions and the short `+` notation for converting strings into numbers. We also observe that, most of the time, we will use the base 10 in the `parseInt` function.

Formatting number values into strings in D3 is very similar to formatting date values into strings. However, D3 can only format numbers into strings; for string to number conversion, you can use the native functions that we just discussed. Again, we need to first create a formatter using the `d3.format(specifier)` function; then, we can use the formatter as a function to format numbers. Let's take a look at an example:

```
// Create a formatter
var formatter = d3.format('.2f');
```

```
// Format a number object to a string
formatter(3.12e-1);
// "0.31"
```

The specifier argument again defines the structure of the formatted string values that we want to generate. This can be composed in the following form:

```
[fill] [align] [sign] [symbol] [0] [width] [, ] [.precision] [type]
```

Each of these options allows you to customize different properties of the generated string. Here, I just want to provide a list of all possible type specifiers, which is also taken from my previous book Data Visualization with D3 and AngularJS by Packt Publishing:

- e: exponent
- g: general
- f: fixed
- d: Integer
- r: Rounded to precision if specified
- %: Percentage
- p: Percentage rounded to precision if specified
- b: Binary
- o: Octal
- x: Hexadecimal
- X: Hexadecimal with upper case letters
- c: Unicode character
- s: SI-suffixed and rounded

Let's take a look at some selected examples using different type specifiers:

```
console.log(d3.format('e')(0.3123)); // "3.123e-1"
console.log(d3.format('g')(31.223e3)); // "31223"
console.log(d3.format('f')(31.23)); // "31"
console.log(d3.format('d')(124)); // "124"
console.log(d3.format('%')(0.23)); // "23%"
console.log(d3.format('p')(0.2314)); // "23.14%"
console.log(d3.format('b')(23)); // "10111"
console.log(d3.format('s')(31.223e3)); // "31.223k"
```

> You can find more information about number specifiers on the D3 Github page at https://github.com/mbostock/d3/wiki/Formatting.

### Grouping data

Most of the time when we find data resources on the Internet, it has a flat data structure just as a comma separated list. But often, it contains hierarchical structured data. In D3, it is very easy to group data together by creating nested tree structures of the dataset. To achieve this, we can use the d3.nest() operator. This operator creates a nest object, which can further define a grouping key via the .key() method and a sorting function via the .sortKey() method. These functions can also be repeated to create multiple nested layers of the flat data structures. Let's take a look at the dataset:

```
var values = [
  {
    date: "04/23/12",
    key: "Group1",
    value: "37"
  },
  {
    date: "04/23/12",
    key: "Group2",
    value: "12"
  },
  {
    date: "04/23/12",
    key: "Group3",
    value: "46"
  }, ...
];
```

First, we create a nest function that will then group our data accordingly:

```
var nest = d3.nest()
  .key(function(d, i){
    return d.date;
  });
```

A nest now implements an .entries(data) method, which can take dataset as input and returns a grouped result of the flat data structure. If you are curious, you can take a look at the output of the preceding nest function applied to our dataset.

```
console.log(nest.entries(data));
```

The previous code shows a result such as the following figure in the developer tools of the browser:

![](img/72559bd4713b2b89631dcca187d7e2db_94_0.png)

Nest function applied to dataset

> You can find more information about nests on the D3 Github page at https://github.com/mbostock/d3/wiki/Arrays#d3_nest.

As a logical next step, we can now apply map and reduce functions to count objects, occurrences, and values and map them into a simpler structure. It would be a good exercise to map the preceding hierarchical object into an array containing {x, y} elements with y being the sum of all values for each object.

Let's use this nest function together with the reduce function to draw a chart similar to the previous example. However, we want to group the entries per day and sum over all groups now.

We can continue doing so by mapping the value property of the elements in the values array and summing the values up. We use the +string short notation to convert the value properties into numeric values:

```
draw(nest.entries(data).map(function(d) {
  return {
    date: d.key,
    value: d.values.map(function(d) {
      return +d.value;
    })
  }.reduce(sum, 0)
}));
```

In the preceding code, we first apply the nest function on the original dataset. Then, we use a map function to apply the next processing step to every group of the nested groups. In the inner processing step, we map the array of values to an array of numbers and sum it up via the reduce function. We can write this a bit more elegantly using the d3.sum(arr, accessor) method, which combines the map and reduce steps. The resulting code is here:

```
draw(nest.entries(data).map(function(d) {
  return {
    date: d.key,
    value: d3.sum(d.values, function(d) {
      return +d.value;
    })
  }
}));
```

Great, this looks much cleaner and is also super readable. Now, we can see the result of the browser in the following figure:

![](img/72559bd4713b2b89631dcca187d7e2db_95_0.png)

Grouped and aggregated values

## Summary

In this chapter, you learned about data preprocessing in order to make it displayable for the visualization. We saw how to filter unexpected data, how to map data to a different representation, and how to aggregate data.

We took a look at loading data from remote resources and custom-parsing via Regular Expressions. We saw how to split a string into an array of elements that can then be used for further processing. In the end, you learned how to use D3 for grouping flat data into nested objects. This helped us to ultimately extract all the desired information out of the remote dataset.

In the next chapter, we will take the previously generated chart and make it responsive. You will learn what it exactly means for a chart to be responsive and how we can use Bootstrap to achieve this.

## Making the Chart Responsive
Using Bootstrap and Media Queries

In the previous chapter, you learned all about loading, parsing, filtering, and grouping data from real sources. This gives us everything we need to design responsive charts using Bootstrap and Media Queries on some real data. In this chapter, you will learn the following:

- Learning absolute and relative units in the browsers
- Understanding what a responsive chart really is
- Drawing charts with percentage values
- Scaling charts using the `viewBox` and `preserveAspectRatio` attributes
- Adapting charts using JavaScript event listeners
- Learning to adapt the resolution of the data
- Using Bootstrap's Media Queries
- Understanding how to use media queries in CSS, LESS, and JavaScript
- Learning the usage of Bootstrap's grid system

First, we will discuss the most important absolute and relative units that are available in modern browsers. You will learn the difference between absolute pixels, relative percentages, em, rem, and many more.

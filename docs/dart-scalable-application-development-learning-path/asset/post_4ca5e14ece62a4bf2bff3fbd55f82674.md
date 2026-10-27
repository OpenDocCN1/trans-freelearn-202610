
The syntax is a bit easier than dart:js but because the names in the js package cannot be minified since it uses dart:mirrors and noSuchMethod, using this library can result in a noticeable increase in code size when compiled to JavaScript. If this is a big disadvantage for you, use dart:js instead. We use the js package in the next recipe to talk to the Google Visualizations API.

### See also

- See the Using JavaScript libraries recipes for more information on how to use JavaScript libraries
- A small library that makes it easy to call Dart from Javascript is available at https://github.com/jptrainor/js_bridge, it's a thin layer around dart:js.

## Using JavaScript libraries

In this recipe, we use the js package as an interface from our Dart script to the Google Chart JavaScript API. This gives us many rich and highly customizable ways to graphically represent data in our Dart web apps and, because it is built with HTML5/SVG, it works cross-browser (even for older IE versions) and cross-platform (also for iOS and Android).

### How to do it...

Take a look at the project googlechart:

1. In the `<body>` tag of the HTML file, place the following:

   ```
   <div id="chart" style="width: 900px; height: 500px;"></div>
   <script type="text/javascript"
     src="https://www.google.com/jsapi"></script>
   <script type="application/dart"
     src="googlechart.dart"></script>
   ```

   The `<div>` tag with the ID chart is where the chart will be drawn; the code to do this is contained in googlechart.dart.

2. The following is the data we want to represent in a chart:

   ```
   var listData = [
     ['Year', 'Sales', 'Expenses'],
     ['2004', 1000, 400],
     ['2005', 1170, 460],
     ['2006', 660, 1120],
     ['2007', 1030, 540]
   ];
   ```

3. After importing the js package, we load the corechart package from the Google API and tell our code to execute the method drawChart when this is done:

   ```
   import 'dart:html';
   import 'package:js/js.dart' as js;
   main() {
       js.context.google.load('visualization', '1', js.map(
           { 'packages': ['corechart'],
             'callback': drawChart,
           }));
   }
   ```

4. Calling the drawChart method then loads the data and draws the chart:

   ```
   void drawChart() {
       var gviz = js.context.google.visualization;
       var arrayData = js.array(listData);
       var tableData = gviz.arrayToDataTable(arrayData);
       var options = js.map({
           'title': 'Company Performance, ',
           'hAxis': {'title': 'Year', 'titleTextStyle': {'color': 'red'}}
       });
       var chart = new js.Proxy(gviz.ColumnChart, querySelector('#chart'));
       chart.draw(tableData, options);
   }
   ```

Performing the previous steps gives the following output in the browser:

![](img/52c05b53a56abd53584e4fed738ed509_699_0.png)

Using Google Charts with js

Also, notice that when hovering over the columns, a tooltip is shown with the exact data for that column.

### How it works...

In the first step, the highlighted script tag refers to the online Google visualization libraries that have to be loaded dynamically before you can use them to draw charts. Step 2 defines the data; this could have been loaded from a file or database. In step 4, we get a reference `gviz` to the JavaScript Google Visualizations object via `js.context`. Then, the data is transformed into a JavaScript array with `js.array`; the chart options such as title, x axis, color, and so on are passed via `js.map`. Then, we construct a proxy `chart` to the JavaScript object, on which the `draw` method is invoked.

So basically, there are four steps to create a chart:

1. Load the `jsapi` library.
2. List the data.
3. Configure the options.
4. Create the chart.

### See also

- Want to learn more about Google Charts? Then visit `https://google-developers.appspot.com/chart/interactive/docs/`, where you can find a wealth of examples. These are written in JavaScript, but with what you now know, you can easily translate them to Dart.

## 6 Working with Files and Streams

In this chapter, we will cover the following recipes:

- Reading and processing a file line by line
- Writing to a file
- Searching in a file
- Concatenating files
- Downloading a file
- Working with blobs
- Transforming streams

### Introduction

Working with files is the bread and butter of every programming language when reaching out for data in the environment. The classes and methods dealing with this functionality can be found in the dart:io package, together with support for networking (sockets and HTTP). This package can only be used in Dart command-line applications, not in browser apps, so our code runs in a Dart VM.

When working with files, and I/O in general, there are two modes of operation:

- Synchronous operations, where code execution waits for the I/O result
- Asynchronous operations, where the code execution is not blocked and continues while I/O is taking place

Because the Dart VM is single threaded, a synchronous call blocks the application. So, for scalability reasons, the asynchronous way is the best practice using the `Future` and `Stream` classes from the `dart:async` package. Most methods on files come in pairs, the asynchronous and the synchronous versions, such as copy and copySync. Unless you really have to wait for the result, use the asynchronous way so that screens and apps do not appear to be blocked and can still respond.

> If you need a recap, visit the tutorials at https://www.dartlang.org/docs/tutorials/futures/ and https://www.dartlang.org/docs/tutorials/streams/. In this chapter, we will focus on recipes to handle files.

### Reading and processing a file line by line

Files containing data in the **comma separated values (csv)** format are structured so that one line contains data about one object, so we need a way to read and process the file line by line. As an example, we use the data file `winequality-red.csv`, that contains 1,599 sample measurements, 12 data columns, such as **pH** and **alcohol**, per sample, separated by a semicolon (;), of which you can see the top 20 in the following screenshot:

| # | fixed acidity | volatile acidity | citric acid | residual sugar | chlorides | free sulfur dioxide | total sulfur dioxide | density | pH | sulphates | alcohol | quality |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 1 |  |  |  |  |  |  |  |  |  |  |  |  |
| 2 | 7.4 | 0.7 | 0 | 1.9 | 0.076 | 11 | 34 | 0.9978 | 3.51 | 0.56 | 9.4 | 5 |
| 3 | 7.8 | 0.88 | 0 | 2.6 | 0.098 | 25 | 67 | 0.9968 | 3.2 | 0.68 | 9.8 | 5 |
| 4 | 7.8 | 0.76 | 0.04 | 2.3 | 0.092 | 15 | 54 | 0.997 | 3.26 | 0.65 | 9.8 | 5 |
| 5 | 11.2 | 0.28 | 0.56 | 1.9 | 0.075 | 17 | 60 | 0.998 | 3.16 | 0.58 | 9.8 | 6 |
| 6 | 7.4 | 0.7 | 0 | 1.9 | 0.076 | 11 | 34 | 0.9978 | 3.51 | 0.56 | 9.4 | 5 |
| 7 | 7.4 | 0.66 | 0 | 1.8 | 0.075 | 13 | 40 | 0.9978 | 3.51 | 0.56 | 9.4 | 5 |
| 8 | 7.9 | 0.6 | 0.06 | 1.6 | 0.069 | 15 | 59 | 0.9964 | 3.3 | 0.46 | 9.4 | 5 |
| 9 | 7.3 | 0.65 | 0 | 1.2 | 0.065 | 15 | 21 | 0.9946 | 3.39 | 0.47 | 10 | 7 |
| 10 | 7.8 | 0.58 | 0.02 | 2 | 0.073 | 9 | 18 | 0.9968 | 3.36 | 0.57 | 9.5 | 7 |
| 11 | 7.5 | 0.5 | 0.36 | 6.1 | 0.071 | 17 | 102 | 0.9978 | 3.35 | 0.8 | 10.5 | 5 |
| 12 | 6.7 | 0.58 | 0.08 | 1.8 | 0.097 | 15 | 65 | 0.9959 | 3.28 | 0.54 | 9.2 | 5 |
| 13 | 7.5 | 0.5 | 0.36 | 6.1 | 0.071 | 17 | 102 | 0.9978 | 3.35 | 0.8 | 10.5 | 5 |
| 14 | 5.6 | 0.615 | 0 | 1.6 | 0.089 | 16 | 59 | 0.9943 | 3.58 | 0.52 | 9.9 | 5 |
| 15 | 7.8 | 0.61 | 0.29 | 1.6 | 0.114 | 9 | 29 | 0.9974 | 3.26 | 1.56 | 9.1 | 5 |
| 16 | 8.9 | 0.62 | 0.18 | 3.8 | 0.176 | 52 | 145 | 0.9986 | 3.16 | 0.88 | 9.2 | 5 |
| 17 | 8.9 | 0.62 | 0.19 | 3.9 | 0.17 | 51 | 148 | 0.9986 | 3.17 | 0.93 | 9.2 | 5 |
| 18 | 8.5 | 0.28 | 0.56 | 1.8 | 0.092 | 35 | 103 | 0.9969 | 3.3 | 0.75 | 10.5 | 7 |
| 19 | 8.1 | 0.56 | 0.28 | 1.7 | 0.368 | 16 | 56 | 0.9968 | 3.11 | 1.28 | 9.3 | 5 |
| 20 | 7.4 | 0.59 | 0.08 | 4.4 | 0.086 | 6 | 29 | 0.9974 | 3.38 | 0.5 | 9 | 4 |

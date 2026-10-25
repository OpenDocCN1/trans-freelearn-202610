
The next step would be to define views that would get displayed when you select any of the buttons in TabStrip:

```html
<div
id="contactsView"
data-role="view"
data-layout="defaultLayout">

<ul
id="contactsViewContainer"
data-role="listview">
</ul>
</div>

<div
id="favoritesView"
data-role="view"
data-layout="defaultLayout">

<ul
id="favoritesViewContainer"
data-role="listview">
</ul>
</div>

<div
id="settingsView"
data-role="view"
data-layout="defaultLayout">

<ul
data-role="listview">
<li>Sorted
<input type="check" data-role="switch" checked>
</li>
<li>Show Images
<input type="check" data-role="switch">
</li>
</ul>
</div>
```

Notice that the id attribute of the preceding view widget definitions are referred in the href attribute of the anchor elements in the TabStrip widget.

## How it works...

When you render the page, the **Contacts** view is shown, and the same is highlighted in the TabStrip widget:

![](img/2a66507445090790d34dc5588b56b24c_176_0.png)

Notice that the **Contacts** icon in TabStrip is highlighted, indicating that the current view shows you a list of **Contacts**. When you touch on the **Favorites** or **Settings** icon, the corresponding icon in TabStrip is highlighted and the view is also updated:

![](img/2a66507445090790d34dc5588b56b24c_176_1.png)

## 10 Kendo UI DataViz

In this chapter, we will cover the following recipes:

- Creating charts using `kendoChart`
- Binding a chart to a `DataSource` object
- Creating a multiaxis chart
- Displaying data over a period of time and using aggregate functions
- Making a chart interactive by adding events
- Changing the chart type dynamically

### Introduction

The Kendo UI library provides a set of powerful visualization components that can be used to bring data to life. It uses modern browser technologies such as SVG and Canvas to render these charts. It falls back to VML on older browsers. The browser support ranges from IE7+ to Chrome, Firefox, Opera (15+), and Safari. In this chapter, we will look at building some of the basic charts such as the area chart, pie chart, and column chart, and how they can be customized. In the next chapter, we will look at some of the advanced data visualization charts that are used to build dashboard-like applications.

## Creating charts using kendoChart

The Kendo data visualization library provides several charting widgets that can be built with ease. In this recipe, we will first build a column chart and then customize its look and feel. A column chart displays vertical bars, visually representing the provided data.

### Getting started

The kendo.all.min.js file contains all the components, that is, Web, Mobile, and DataViz widgets. However, if you're building only data visualization components, then include the kendo.dataviz.min.js file.

### How to do it...

To create a chart, let's add a container to the page, specifying the dimension of the chart area, as shown in the following line of code:

```html
<div id="chart" style="width:500px; height: 400px">
```

Notice that the chart has a width of 500 pixels and a height of 400 pixels. This area will be used to render the chart. The next step is to initialize the chart using the kendoChart function, as shown in the following line of code:

```javascript
$("#chart").kendoChart()
```

In the preceding code line, a chart is initialized by invoking the kendoChart function on the selected div element. When you render the page, a chart with only its axes will be shown. The next step is to provide some data and a set of options that will be used to display the chart. These includes the chart title and the type of chart that you want to display, as shown in the following code snippet:

```javascript
$("#chart").kendoChart({
    title: {
        text: 'Browser Adoption'
    },
    categoryAxis: {
        categories: [2010, 2011, 2012, 2013, 2014]
    },
    series: [{
        name: 'Internet Explorer',
        data: ['32', '23', '17', '12', '9'],
        type: 'column'
    }],
    valueAxis: {
        labels: {
            format: '{0}%',
        }
    }
});
```

In the preceding code snippet, several options have been provided to initialize the chart widget. Let's take a look at each one of them:

- title: This option is used to specify the title text that should be shown on top of the chart. The text option specified in the title will be displayed.
- categoryAxis: This option is used to specify the categories that should be displayed on the x axis of the chart.
- series: This option is used to specify the data that needs to be displayed in the chart. The name option is used to display the legend information. The data option includes all the data that needs to be plotted on the chart and type specifies the chart type that needs to be displayed.
- valueAxis: This option refers to the y axis and in the preceding example, it specifies the format in which the values in the y axis should be displayed. The {0}% value serves as a template where {0} is replaced with a value based on the data that is provided.

### How it works...

After providing all the options in the kendoChart function, you will see that a column chart is added to the page, which also contains the title and legend information, as shown in the following screenshot:

![](img/2a66507445090790d34dc5588b56b24c_180_0.png)

Notice that the preceding column chart shows you the title at the top and the legend information on the right-hand side. The color theme that has been applied is the default one.

### Showing multiple categories in the chart

The chart that we created had only one series added; when you want to compare multiple products or categories, the series array can include them:

```javascript
series: [{
 name: 'Internet Explorer',
 data: ['32', '23', '17', '12', '9']
}, {
 name: 'Firefox',
 data: ['45', '39', '34', '28', '26']
}, {
 name: 'Chrome',
 data: ['15', '26', '40', '52', '56']
}, {
 name: 'Safari',
 data: ['6', '5', '4', '4', '4']
}]
```

Notice that the type information is removed from the series. When building a chart, you can specify the common options for the series under seriesDefault:

```javascript
seriesDefaults: {
 type: 'column'
}
```

Now when you render the page, other series will be included.

![](img/2a66507445090790d34dc5588b56b24c_181_0.png)

## Changing the position of the title and legend information

It is possible to customize the chart to change the position of the title and legend information:

```javascript
title: {
    text: 'Browser adoption',
    position: 'bottom'
},
legend: {
    position: 'bottom'
}
```

When you render the page, the chart will occupy the entire width and the legend and title information will be shown below the chart.

![](img/2a66507445090790d34dc5588b56b24c_182_0.png)

The height of the chart is affected and is now shown in the given height of 400 pixels in the chart along with the legend and title information. You can specify the position as top, bottom, right, left, or custom. When the position is custom, you need to specify the `offsetX` and `offsetY` options as well:

```javascript
legend: {
    position: 'custom',
    offsetX: 50,
    offsetY: 20
}
```

## Changing the default theme

The theme applied in the preceding chart is the default one. You can change the theme from the default one by specifying the theme option, as shown in the following line of code:

```javascript
theme: 'black'
```

After changing the theme to black, the page would be rendered as shown in the following screenshot:

![](img/2a66507445090790d34dc5588b56b24c_183_0.png)

![](img/2a66507445090790d34dc5588b56b24c_183_1.png)

Now when you render the page, you will notice that the legend information is positioned at 50 pixels from the left and 20 pixels from the top of the chart area, as shown in the following screenshot:

The other values that can be assigned to the theme property include blueopal, bootstrap,
highcontrast, metro, metroblack, moonlight, silver, and uniform.

## Binding a chart to a DataSource object

A Kendo chart widget can be bound to a DataSource object. This DataSource object
can be local or remoteDataSource. When using a DataSource object, the series
and configurationAxis fields must be configured to refer to the fields in the
DataSource object.

### How to do it...

Let's first create a local DataSource object, which will then be used to construct the chart:

```javascript
var chromeBrowser = [{
'year': 2008,
'percentage': 0
}, {
'year': 2009,
'percentage': 4
}, {
'year': 2010,
'percentage': 15
}, {
'year': 2011,
'percentage': 26
}, {
'year': 2012,
'percentage': 40
}, {
'year': 2013,
'percentage': 52
}, {
'year': 2014,
'percentage': 56
}];
```

The next step is to specify the preceding code as a DataSource reference when constructing
the chart:

```javascript
$("#chart").kendoChart({
 dataSource: {
  data: chromeBrowser
 },
 title: {
```

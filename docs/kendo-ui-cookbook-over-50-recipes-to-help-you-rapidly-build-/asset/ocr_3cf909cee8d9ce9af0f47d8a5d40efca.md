```
}, {
'year'
: 2012,
'percentage' : 40,
'feature' : 'New Extensions API'
}, {
'year'
: 2013,
'percentage' : 52,
'feature' : 'Web Speech API'
}, {
'year'
: 2014,
'percentage' : 56,
'feature' : 'Different look for Win8 metro mode'
}];
```

The next step is to specify the tooltip option that contains fields that will be used to display
a tooltip when the user hovers over a data item in the series, using the following code snippet:

```
tooltip: {
visible: true,
template: '#= dataItem.feature #',
background: '#0F0'
}
```

Here, the template field contains the dataItem.feature value. The dataItem field refers
to the object that is used to plot the column in the chart. The other possible values include
category, which refers to the category in categoryAxis, which, in this case, would be the
year field; value, which refers to the percentage value used to draw the column; and series,
which refers to the entire series object.

To handle click events, specify the seriesClick option:

```
seriesClick: function (e) {
//iterate over series data and set the color for each series
for (var i = 0, j = e.series.data.length; i < j; i++) {
e.series.data[i].color = '#00F';
}
//change the color of the selected series
e.dataItem.color = '#F00';
//redraw the graph - required for the changes to get reflected
this.redraw();
}
```

The first statement in the preceding function loops through all the objects in the series and
resets the color; then, the selected item in the series is assigned a different color. The chart
needs to be refreshed or redrawn to bring these changes into effect; hence, the last line
invokes the redraw function on this. The this object refers to the context; in this case,
this is the series object.
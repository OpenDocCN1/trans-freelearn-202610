```
new OpenLayers.Geometry.Point(
    (Math.floor(Math.random() * 360) - 180),
    (Math.floor(Math.random() * 180) - 90)
),
{
    //Attributes go here
    size: 5 + (Math.floor(Math.random() * 20)),
    label: 'F' + i,
    strokeWidth: (Math.floor(Math.random() * 10))
}
]);
}
```

2. Now, we just need to modify the vector_style style object to include attribute replacements so we can use the size, label, and strokeWidth properties of the features we just created. The syntax to do so is '${variable_name}', where variable_name is the name of the feature property we want to use. Modify the vector_style object as follows, changing the label, pointRadius, strokeDashStyle, and strokeWidth:

```
var vector_style = new OpenLayers.Style({
    'cursor': 'pointer',
    'fillColor': '#787878',
    'fillOpacity': '.8',
    'fontColor': '#343434',
    'label': '${label}',
    'pointRadius': '${size}',
    'strokeColor': '#232323',
    'strokeDashstyle': 'solid',
    'strokeWidth': '${strokeWidth}'
});
```

3. Open the map. We used randomized values, so when you refresh your map you should see something different each time. You should see something similar to this, but with different point locations and sizes:
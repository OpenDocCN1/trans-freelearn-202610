```
new OpenLayers.Geometry.Point(
    (Math.floor(Math.random() * 360) - 180),
    (Math.floor(Math.random() * 180) - 90)
),
{
    'population': Math.floor(Math.random() * 2000)
}
]);
}
```

3. Now we need to create a style object—rules are applied directly to style objects, and filters are applied directly to rules. So, let's create the objects in order, starting with the style object:

```
var vector_style = new OpenLayers.Style();
```

4. Let's now create three rules. For each rule, we'll pass in an object with two keys: filter (which will be a Filter class object) and symbolizer (which is a familiar symbolizer object). The filter object will need a filter type, which we'll cover after this example, and a property which the rule will match on (in this case, population). We'll also need to specify the value (or range of values) we want to match for each rule. The value parameter depends on the filter type, and we'll be using either value or lowerBound and upperBound. Let's create the first rule which will handle population values less than 500:

```
var rule_pop_low = new OpenLayers.Rule({
    filter: new OpenLayers.Filter.Comparison({
        type: OpenLayers.Filter.Comparison.LESS_THAN,
        property: 'population',
        value: 500
    }),
    symbolizer: {
        fillColor: '#ababab', fillOpacity:.8,
        pointRadius:8, strokeColor: '#454545',
        strokeWidth:2
    }
});
```
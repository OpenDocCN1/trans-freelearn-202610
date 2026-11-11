7. Now we'll create one more filter. This one will use the OR logical operator to see if the population is above 1500 or if the settlement_type is 'city'. So, even if settlement_type is 'city' and the population is say, 500, it will get matched because at least one of the filters matches:

```
var filter_city = new OpenLayers.Filter.Logical({
    type: OpenLayers.Filter.Logical.OR,
    filters: [
        new OpenLayers.Filter.Comparison({
            type: OpenLayers.Filter.Comparison.GREATER_THAN,
            property: 'population',
            value: 1500
        }),
        new OpenLayers.Filter.Comparison({
            type: OpenLayers.Filter.Comparison.LIKE,
            property: 'settlement_type',
            value: 'city'
        })
    ]
});
```

8. Ok—now we'll just create some rules and apply the filters to them. Each rule will be applied to one of the filters we just created.

```
var rule_village_low_pop = new OpenLayers.Rule({
    filter: filter_village_low_pop,
    symbolizer: {
        fillColor: '#ababab', fillOpacity:.8,
        pointRadius:8, strokeColor: '#454545',
        strokeWidth:2
    }
});

var rule_village_high_pop = new OpenLayers.Rule({
    filter: filter_village_high_pop,
    symbolizer: {
        fillColor: '#FFFA93', fillOpacity:.8,
        pointRadius:8, strokeColor: '#AFAB57',
        strokeWidth:4,
    }
});

var rule_city = new OpenLayers.Rule({
    filter: filter_city,
    symbolizer: {
        fillColor: '#BD1922', fillOpacity:.8,
```
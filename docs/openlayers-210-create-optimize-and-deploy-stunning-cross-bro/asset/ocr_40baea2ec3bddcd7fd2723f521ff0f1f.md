8. Now create a styleMap object and apply the style object to it and set the vector layer to use it:

```
var vector_style_map = new OpenLayers.StyleMap({
  'default': vector_style
});
vector_layer.styleMap = vector_style_map;
```

9. Take a look at the map now. You should see something like this, but with different point locations and colors, as the values were randomized:

![](bbox=[0.2, 0.3, 0.8, 0.55])

# What Just Happened?

We just showed how rules and filters are used. We used three rule objects, each with a different filter object. The type property of the filter objects determine the type of comparison that will be applied. So, in the previous example, if a feature had a property of 'population' with a value less than 500, the rule_pop_low rule would be applied to it. Now that we have a basic understanding of how it's used, let's look more closely at the two classes.

# OpenLayers.Rule class

When you want to be very specific about the types of styles that get applied to feature objects, the best way to do it is by using the Rule class. When you create a rule object, you specify a Filter object, a symbolizer to apply to the feature if the rule matches, and an optional name property which specifies an arbitrary name for the rule.

```
format: new OpenLayers.Format.KML({
    extractAttributes: true
})
}),
strategies: [new OpenLayers.Strategy.Fixed()]
}
);
```

5. And add it to the map:

`map.addLayer(vector_layer);`

6. We should see some points now that show the location of the images that were uploaded. Open up the page via localhost, for example: `http://localhost/openlayers_book/chapter_11/code/example_1.html`.

![](img/1b8cd151c9f278555f9cf255b20d244e_334_0.png)

7. Now, let's take a look at what `extractAttributes` is used for. When we set it to `true`, attributes from each feature in the KML file is applied to the `attributes` property of each feature object. More bluntly—it takes attributes from the file and applies them to your points. Bring up Firebug and, in the console, access one of the feature objects:

`map.layers[1].features[0].attributes`

8. You should see an object that contains three properties: `Snippet`, `name`, and `styleUrl`. These are somewhat arbitrary properties, specified in the KML file for each photo, and with `extractAttributes: true` we can access them. The `styleUrl` is a reference to a style tag inside the KML file—we'll talk about it in the next example.

## What Just Happened?

We just loaded in the KML file and used `extractAttributes` to look at the properties that each photo contains. Flickr is providing these attributes—the attributes in the KML file gets applied to our feature objects. Another property we can use is `extractStyles` to apply the styles from the KML file to our features.

## Time for Action – extract style

The `extractStyles` can be used to style the vector layer based on the styles specified in the KML file. Because the Flickr KML file provides style information, we can use this property—let's see how.

- 1. If the KML file you are working with contains style information, we can directly access and use it. In this case, Flickr is providing us with a KML file embedded with style tags, so let's use it.
- 2. We'll specify `extractStyles: true` inside our vector layer creation call. In the format value setting, add it in, and be sure to include the comma:

```
...
format: new OpenLayers.Format.KML({
    extractAttributes: true,
    extractStyles: true
})
...
```

3. Now open the page and you should see thumbnails of the images:

![](img/1b8cd151c9f278555f9cf255b20d244e_336_0.png)

## What Just Happened?

Using extractStyles, our features went from the default orange dots to thumbnails of the actual uploaded photos. When this property is used, style definitions in the KML file are applied to the style property of each feature. KML files can include style information—that is why we can use the extractStyles property with the KML format. OpenLayers will parse the style information from the KML file and generate the necessary style definitions.

## Turning our example into an application

So far, we've accessed data from Flickr, saved it to a file, and added it to our map. This is pretty cool, but we really haven't done much else than just load in the data, from an OpenLayers point of view. It's useful, but we really haven't created a full featured 'web application' just yet. So, let's focus on how to build a more useful web-mapping application.

To do this, we'll basically need to do two general things:

- 1. Add some interactivity to our map.
- 2. Use 'live' data. We shouldn't have to manually download a KML file every time we want new data—our web application should do it automatically.

Let's focus on the first part, and then change the data source after we develop some interactivity.

## Adding interactivity

In the previous example, each feature received an `externalGraphic` property because we used `extractStyles` (causing thumbnails to be displayed for each feature)—but it's hard to see where they are on the map, especially when zoomed out. We'll need to address that. We want to show the photos, but we also want to show where they are.

To accomplish this, we could revert back to using point features and making use of the `selectFeature` control. But what if too many features are too close to each, making it harder to select them? That's what the **cluster** strategy is for, so we'll use it as well.

## Selecting features

What should happen when we select a feature? We'll need to show information about the feature we clicked on, and that information is the Flickr photo itself and any associated attributes. So, let's keep things a little simple for now and show the photo(s) information below the map when a feature is selected.

## Time for Action – adding some interactivity

In this example, we'll be using the **cluster** strategy, **selectFeature** control, and showing photo information on selection. As we go along, feel free to tweak the examples yourself, change the functionality, or anything else—that's the best way to learn!

- 1. Make a copy (or direct edit) of the code from the previous example. We'll be improving it.
- 2. We'll keep `extractAttributes` and `extractStyles`. Even though we specify `extractStyles`, we're going to be using a custom `styleMap` object. We're doing this so that we can still access the thumbnail image location (which is set as `externalGraphic` in the feature's `style` object when using `extractStyles`). Make sure the `format` object looks like the following:

```
...
format: new OpenLayers.Format.KML({
    extractAttributes: true, extractStyles: true
```

```
})
...
```

3. Let's add a `cluster` strategy to the vector layer now so our points cluster. Our `strategies` array should now look like the following:

```
strategies: [new OpenLayers.Strategy.Fixed(),
            new OpenLayers.Strategy.Cluster()]
```

4. Let's take a quick look at the map to make sure it works. Since we have used the cluster strategy, we won't see the thumbnails that we would have (like in the previous example). Remember to access it from http://localhost/...:

![](img/1b8cd151c9f278555f9cf255b20d244e_338_0.png)

5. So far, so good. Our points look kind of small though, and we can't visually distinguish how many points are in a cluster. Let's change that. We'll make the clusters larger depending on how many points the cluster contains. When using clustering, each feature belongs to a cluster, and we can access the number of features in a cluster through feature.attributes.count. Let's also add a label to each feature showing the number of points it contains. We'll also pass in a second context object when instantiating the style object, which allows us to define the values of variables to use with attribute replacement. The context object will contain a function that returns some value which will be used with the attribute replacement.

```
//Create a style object to be used by a StyleMap object
var vector_style = new OpenLayers.Style({
    'fillColor': '#669933',
    'fillOpacity': .8,
    'fontColor': '#f0f0f0',
    'fontFamily': 'arial, sans-serif',
    'fontSize': '.9em',
    'fontWeight': 'bold',
    'label': '${num_points}',
    'pointRadius': '${point_radius}',
    'strokeColor': '#aaee77',
    'strokeWidth': 3
},
    //Second parameter contains a context parameter
    {
        context: {
            num_points: function(feature){ return feature.attributes.count; },
            point_radius: function(feature){
                return 9 + (feature.attributes.count)
            }
        }
    }
));
```

```
//Create a style map object and set the 'default' intent to the
var vector_style_map = new OpenLayers.StyleMap({
    'default': vector_style
});
```

```
//Add the style map to the vector layer
vector_layer.styleMap = vector_style_map;
```

6. Let's take a look. The size and labels should change for each cluster:

![](img/1b8cd151c9f278555f9cf255b20d244e_340_0.png)

7. The first part of our plan is done—we've successfully added clusters. Now, we need to add feature selection ability. Let's create a selectFeature control. We'll add events soon, but for now let's just create the control and add it to the map and then activate it.

```
//Add a select feature control
var select_feature_control = new OpenLayers.Control.SelectFeature(
    vector_layer,
    {}
)
map.addControl(select_feature_control);
select_feature_control.activate();
```

8. Now, if we select a feature it won't look quite right. This is because we haven't defined a style for the select intent. Let's go back and define a style object and add it to our vector_style_map object:

```
var vector_style_select = new OpenLayers.Style({
    'fillColor': '#cdcdcd',
    'fillOpacity': .9,
    'fontColor': '#232323',
```

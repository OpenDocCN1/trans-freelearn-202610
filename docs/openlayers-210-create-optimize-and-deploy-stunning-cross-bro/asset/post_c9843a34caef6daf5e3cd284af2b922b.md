
A developer that has proficient design skills is not a common thing—fortunately, there are resources that provide high quality, professional user interface elements. **GeoExt** is a third party library that integrates **ExtJS**, a popular JavaScript framework, with OpenLayers, and includes numerous user interface and OpenLayers map components. It does, however, require a bit of familiarity with the ExtJS library. GeoExt can be found at http://www.geoext.org/index.html, and the ExtJS can be found at http://www.sencha.com/products/js/.

## Summary

In this chapter, we talked about what CSS is and what it's used for. We learned how to use CSS and HTML together, along with how to refer to elements in CSS. Then, we discussed how OpenLayers uses CSS, and how control elements get their names. Lastly, we created and applied our own custom styles to some controls.

The goal of this chapter was to provide a foundation for understanding how we style controls with CSS. If you haven't worked much with CSS before or if you're unsure how to style other types of controls then don't worry. We'll be styling controls throughout the book using the same principles we introduced in this chapter, so you'll get a lot more exposure to it.

## 8 Charting the Map Class

> *The Map class is, as you have probably figured out by now, the core piece behind your map. The map object(s) you create is the most important thing behind your map, as without a map object you can't do anything with layers or controls. In this chapter, we'll be talking about the Map class, which we've been taking for granted so far.*
>
> *Understanding the Map class will enable us to do even more cool things with our applications, and provides a way for us to programmatically tell our maps what to do.*

We've been using the Map class throughout the book so far without really knowing how or why. This chapter aims to not only explain how and why we've been doing things (such as using `map.zoomToMaxExtent()`, but also provide a thorough coverage of one of the core parts of OpenLayers—the Map class. We'll take a look at

- What the Map class is
- How the Map class relates to the other classes we've discussed
- Accessing the Map class' properties
- Using functions of the Map class
- Working with events to define and extend map behavior
- Creating a simple application that contains multiple maps

### The Map class

OpenLayers' Map class is what drives our maps. All the things we'd like to do with our maps—moving them, zooming, adding layers—all these things are made possible by this class. We've worked extensively with it already by creating a map object, adding controls and layers to it, then telling the map to zoom to the max extent. While we've covered Layers and Controls in pretty good detail, we have yet to really discuss the functionality behind the Map class, the core component of our applications.

In OpenLayers, control and layer objects belong to a map object. Control and layer objects must be hooked up to a map if we want them to do anything. So, we need a map object to actually make a useful map—and as you might imagine, we'll see later in the chapter that it is possible to make an application that uses multiple map objects.

### Creating a map object

Before we jump in, let's review the base code for instantiating a map object. We've done this many times before, but a little review won't hurt.

```
var map = new OpenLayers.Map('html_element', { options });
```

The first parameter is a string consisting of the ID of the HTML element you wish the map to be placed in (almost always a div element). The second parameter is optional, and consists of an object literal, or anonymous object, (key:value pairs) containing property and value settings.

So, there's not much new here yet. This is, essentially, all you need to set up your map object. However, as we've mentioned before, if you're using a projection other than EPSG:4326, such as spherical Mercator, you're going to have to set some other properties as well (such as maxExtent and units), when creating the map object.

This chapter will cover most of the properties and functions available to us through the Map class, so let's start with the properties.

### Map class properties

There is a vast array of possible properties that you can pass in, and we've already seen a few of them throughout the book. There are also quite a few properties which are pretty useful which we haven't yet covered. I'll start off by introducing a few properties, and then we'll see them in action with some examples.

Throughout the explanation of properties and methods, I'll typically refer to an arbitrary map object as `map`. If your map object is called something else, then you would simply replace `map` with the name of your map object. Another quick note; while we will be talking about all these properties in the context of passing them in while creating the map object, you can call any of them at any time to get the value (e.g., calling `map.tileSize` will return the map's tile size).

> While we'll cover all the properties and functions relevant to all the examples and discussions in the book, for an always up to date and complete list you can visit the docs at http://dev.openlayers.org/docs/files/OpenLayers/Map-js.html.

### Map properties

Let's go over the map properties that are typically passed in when creating a map object. There are other properties that we can access but shouldn't pass in at instantiation time—we'll cover those next. For now, let's focus on properties to use when instantiating map objects.

#### allOverlays

```
allOverlays: {Boolean} Default is false
```

The `allOverlays` property specifies whether or not the map can function without any base layers. So far, we've been using base layers in all our examples, either explicitly or implicitly (letting our layers be set as base layers automatically). By setting this property, *all* layers will act as overlay layers.

There may be times when you wish to use this—just keep in mind, when using this property, users have the ability to disable all layers and they could, in effect, see an empty map. By setting this property to `true`, third party API layers will also act as overlay layers. So, if you wish, you could use a Google maps layer as an overlay layer and make it semi transparent.

#### controls

```
controls: {Array {OpenLayers.Control}}
```

We covered controls in Chapter 6, so we don't need to go into too much depth here. We can define our map's controls when we create the map object, or we can add them via `addControl(s)` after the map object has been instantiated. After the map is instantiated, we can access its controls by calling `map.controls`. If you do not specify an array of controls for the map to use when you create it, your map will receive four default controls: **Navigation**, **PanZoom**, **ArgParser**, and **Attribution**.

#### displayProjection

```
displayProjection: {OpenLayers.Projection}
```

This is a property that is used mainly by controls which show coordinate information. By setting this property on the map object, any control that has a displayProjection property will be set to this value. Controls, such as the MousePosition control, can display coordinates in the displayProjection. So, your map could be in a different projection than what you wish to display the coordinates in. However, to use a displayProjection other than EPSG:4326 or EPSG:900913, Proj4js must be included on your page.

This property comes in very handy if, for instance, your map is in a spherical Mercator projection (i.e., EPSG:900913), but you might wish to display coordinates in another projection, like EPSG:4326 (to display lon/lat coordinates).

#### div

```
div: {HTML Element or String}
```

So far, we've been creating our maps like this:

```
var map = new OpenLayers.Map('map_element', {});
```

With the div property, we can instead pass in an HTML element (or the ID of the element, as above) in the options; i.e.,

```
var map = new OpenLayers.Map({'div': 'map_element'});
```

One of the advantages of doing it this way is that the div property is optional, so you can create a map object and choose not to place it in any div. If you do this, you can place your map in a div later by using the render() method.

This is actually almost the same thing OpenLayers is doing for us automatically when we pass in an HTML element during map object instantiation. When we do specify this div property, OpenLayers simply calls the render() method right then—we're just delaying this from happening and calling it manually if we choose not to specify a div. This can be useful when we only want to show the map after a user clicks on a button, for instance.

### Time for Action – using the allOverlays Map property

Let's take a look at using the allOverlays property and not specifying a div when creating a map.

- 1. In your init() function, let's create our map object with allOverlays set to true and with some controls passed in. We will also not specify an HTML element:
- 
-       var map = new OpenLayers.Map({

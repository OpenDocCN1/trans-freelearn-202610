4. You should see something like this:

![](bbox=[0.22, 0.18, 0.78, 0.31])

# What Just Happened?

We just used scales to determine the zoom levels of our map. Using scales or resolutions comes in handy when you want to specify the maximum or minimum zoom levels, as well as if you want to specify a hard-coded array of zoom level values (and especially when you are working with a cached tile server). Let's talk about some other, non zoom level related properties now.

# panMethod

```
panMethod: {Function} Default: OpenLayers.Easing.Expo.easeOut
```

This property specifies what type of tween animation will be used when panning the map. The default value is a function called OpenLayers.Easing.Expo.easeOut, which causes the animation to 'ease out' (or slow down) when the panning is finished. At the time of writing, the possible values are OpenLayers.Easing.Expo, OpenLayers.Easing.Quad, and OpenLayers.Easing.Linear. Each of those three base functions are divided into three further types: easeOut ('out' means when the animation is finished), easeIn (in means when the animation starts), and easeInOut (which means the animation will happen at both times). The docs at http://dev.openlayers.org/docs/files/OpenLayers/Tween-js.html will contain the most up to date possible values.

When specifying this property, use the full combination of the base class and subclass, such as OpenLayers.Quad.easeInOut. The animation effect will occur whenever the map is panned, such as when using the pan arrow buttons, panning the map with your keyboard, or calling panTo().
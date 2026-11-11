5. Now let's register the sketch events. They will get triggered when using any of the EditingToolbar controls.

```
map.layers[1].events.register('sketchstarted', this, sketch_start);
map.layers[1].events.register('sketchcomplete', this, sketch_complete);
```

6. Now, when adding a point or creating a polygon / line, you should see a total of three messages when you finish adding the feature:

```
sketch started Object { feature=Object,  more...}
sketch done Object { feature=Object,  more...}
feature added Object { feature=Object,  more...}
```

# What Just Happened?

We just demonstrated a few more event types. You can also figure out how the events propagate (which events get executed first) by viewing the console log. From the example above, we can see that the sketchcomplete event get executed before the featureadded event.

Even though we registered the featureadded event first, the order that we register the events in does not change the order that the events propagate in. We'll be using more of the vector layer events throughout the chapter, but it shouldn't be much more complicated than the previous two examples. So, let's get on with it!

# Geometry and Feature classes

We've been using feature objects throughout the chapter so far without really talking about the Feature class itself. We also have hinted at the Geometry class, but we haven't gone into any detail so far. This section will cover both classes in a bit more detail so that you can gain a bit more confidence working with them. Don't worry—it's easy, you've already been exposed to both classes.

Before we get into the Feature class though, we should go over the Geometry class, as it's used to create the actual geometry objects that make up a feature object.
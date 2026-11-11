```
function update_map_2(event){
    map_1.setCenter(map_2.getCenter(), null, true);
}
```

8. Finally, let's finish up by registering the events for our two map objects.

```
map_1.events.register('moveend', map_1, update_map_1);
map_2.events.register('moveend', map_2, update_map_2);
```

9. All done! Open the page and pan around. When you finish panning around with one map, the other map will be updated.

# What Just Happened?

You just created a fairly complicated mapping application with two map objects and a couple custom events. With all you've learned from this chapter and the previous ones, we can start to do some pretty interesting things. This example is just a small sample of what we can do with just a little bit of work in OpenLayers.

# Summary

This brings us to the end of the core coverage of the OpenLayers library. At this point, we've covered in good detail the three 'core' classes—Layer, Control, and Map.

This chapter specifically covered the Map class, along with its properties and methods. We then went in to greater detail about map events and how to work with them. Lastly, we learned how to use multiple map objects on the same page.

Now that we've gotten the fundamentals down, we're ready to start applying those concepts and to create some really interesting mapping applications. The next chapter covers the Vector class, a very powerful layer class that allows us to do a lot of really neat things.
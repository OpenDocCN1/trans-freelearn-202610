# BBOX parameter

One of the important variables to take a look at in the URL is the BBOX, or Bounding Box, parameter. Expand one of the URLs by clicking on the plus sign next to it and take a look in the Params tab. This BBOX number is the extent of that individual map image piece. You'll see a number in the following format:

```
minx, miny, maxx, maxy
```

These four numbers form a rectangle that contains the extent of the map image. A number example might be something like -73.5, 39.3, -67.5, 45. These numbers will depend on the projection your map is in—but don't worry too much about that for now.

If you look at the BBOX parameter of other URLs, you'll notice that part (or all) of the BBOX values change for each request. URLs that are grouped together in the request list share some values for their BBOX parameter. It is analogous to a grid; each request returns an individual cell of the grid. (This functionality is not the same for all Layer types in OpenLayers, but this basic concept holds for the majority of Layer types, including the WMS layer in this example.)

# Pop Quiz– panel

+   1. What panel would you use if you wanted to execute JavaScript code?
+      a. The Net panel
+      b. The Console panel
+      c. The DOM panel
+      d. The HTML panel

# Panel conclusion

Each panel serves a certain purpose and all of Firebug's panels are extremely useful, but throughout the book we will be mainly focusing on the following panels:

+   - Console panel (Command Line JavaScript)
+   - HTML panel
+   - Net panel

These three panels will be used the most throughout the book. We'll occasionally come back to the other panels, but we won't spend a whole lot of time with them. However, before we conclude this chapter, let's get a bit more familiar with the Console panel, since we'll be making heavy use of it in the coming chapters.
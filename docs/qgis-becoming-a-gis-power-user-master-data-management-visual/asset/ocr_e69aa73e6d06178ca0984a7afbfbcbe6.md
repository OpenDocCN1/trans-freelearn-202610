# How it works...

Converting to simpler types from more complex ones is fairly straightforward in simple cases. Lines are just multiple points connected together and polygons are lines that start and end with the same point. So, it's pretty easy to see how to deconstruct one geometry to simpler geometries.

It's building up from points, which is a little trickier. In a line with three or more points, you need to make sure that you have them in the correct order; otherwise, you'll end up with a squiggle. When going to polygons, this can create bigger issues by leaving you with invalid polygons that self-intersect. So, it's really important to order your points in your source table in the same order that they will be combined. Reordering your data can be somewhat tricky. The Points2One plugin now includes a sort order option; to use this, make sure that your attribute table has a numeric column with the order of the points specified per group (you can restart the numbering at 1 for each distinct grouping).

# There's more...

You can also split or combine multipolygons with the **Singleparts to Multiparts** and **Multiparts to Singleparts** commands.

When things get really tricky, you may need to switch to editing the shapes by hand or custom scripts. A good example of this is when you want a polygon with a hole in the middle. If you do go the route of editing by hand, make sure to turn on snapping so that your lines are automatically snapped to existing points. The official documentation on snapping can be found at http://docs.qgis.org/2.8/en/docs/user_manual/working_with_vector/editing_geometry_attributes.html#setting-the-snapping-tolerance-and-search-radius.

The **Editing** and **Advanced Editing** toolbars and additional editing related plugins offer the ability to manipulate particularly tricky geometries, one at a time, if you need to.

# Converting points to lines to polygons and back – SpatiaLite

The goal of this recipe is identical to the previous recipe, but it covers how to perform the process with data in a SpatiaLite database. You will to turn points into lines and lines into polygons.

Not all methods are available; for those that are not available, you can use the previous recipe. It will also work on a database layer; it just doesn't save the results to the database. So, the results will need to be imported to the database after completion.
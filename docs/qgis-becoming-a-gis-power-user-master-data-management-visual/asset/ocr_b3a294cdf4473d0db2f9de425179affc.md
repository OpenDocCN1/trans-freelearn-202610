# Extending QGIS

> If you disable **Projection-on-the-fly**, make sure that all layers are in the same projection; otherwise, they won't line up. Also, perform all analysis steps before converting to a projection that is intended for cartography, as the units of measurement may become messy.

Geometries that cross the outer edge of projections don't always cut off nicely. You will often see this as an unexpected polygon band across your map. The easiest thing to do in this case is to remove data that is outside your intended mapping region. You can use a clip function or simply select what you want to keep and **Save Selection As** a new layer.

There are other common projection description formats (`prj`, `WKT`, and `proj4`) out there. Luckily, several websites help you translate. There are a couple of good websites to look up the existing Proj4 style projection information available at `http://spatialreference.org` and `http://epsg.io`.

# See also

+   - Need more information on how to pick an appropriate projection for the type of map you are making? Refer to the USGS classic map projections poster available at `http://egsc.usgs.gov/isb/pubs/MapProjections/projections.html`. Much of this is also used in the Wikipedia article on the topic available at `http://en.wikipedia.org/wiki/Map_projection`. The `https://www.mapthematics.com/ProjectionsList.php` link also has a great list of projections, including unusual ones with pictures.

# Working near the dateline

If you read the previous recipe about custom projections, you might have noticed the note about data that crosses the edge of projections and how it doesn't usually render properly. When working on data near -180 or 180 degrees longitude, you are going to have this issue. Maps showing far Eastern Russia, Fiji, New Zealand, and the South Pacific, to name a few places, will often contend with this issue.

The required solution really depends on what you're trying to do. If you just need a map of such areas, pick a locally suitable projection. If you have existing data from other sources, it may be cut along the edge and you might need to stitch it back together. As for worldwide maps, sometimes you have to trim .01 degrees of the edge of your data so that it doesn't display oddly.

# Getting ready

To follow this recipe, you will need the `honolulu-flights.shp` layer and the SpatiaLite database `new-zealand.sqlite` from the sample data.
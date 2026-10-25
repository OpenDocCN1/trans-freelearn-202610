# # Choropleth maps

**Choropleth maps** are maps that show some particular area or region with an associated measurement or statistic. They are commonly used in cartography to depict population densities and similar metrics and are used regularly to visualize data with geographical significance. The following image, compiled using the https://commons.wikimedia.org/wiki/File:World_population_density_map. PNG and https://en.wikipedia.org/wiki/File:ElectoralCollege2012.svg Wikimedia Commons images, demonstrates applications in cartography (left) and in data visualization (right):

![](bbox=[0.1758, 0.3125, 0.8242, 0.5414])

# # Creating choropleth maps

Often when you need to associate data with some geographical region or area, the best approach is to utilize an existing map in SVG format that you can then tailor to your needs (manually by modifying the raw XML or using a graphics editor such as Inkscape). For the purposes of demonstration, we will work with an existing map of the United Kingdom taken from http://en.wikipedia.org/wiki/File:England_ Regions_within_UK.svg.

If you download this map and open it up in your XML editor you will see that the districts of England are defined as individual <path> elements with IDs on them (for example, <path id="London"> represents the London region while <path id="north-west"> depicts the entire North West England). Ready Set Raphael handles this really well in that it defines variables for each of the paths with IDs:

```
```
var london = rsr.path(...);
var northwest = rsr.path(...);
```
```
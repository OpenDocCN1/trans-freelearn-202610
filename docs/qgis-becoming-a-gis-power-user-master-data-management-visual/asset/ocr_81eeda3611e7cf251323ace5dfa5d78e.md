For example, consider the following URL: http://a.tiles.mapbox.com/v3/bmearns.c7/page.html#11/39.8715/-75.7514.

![](bbox=[0.205, 0.195, 0.8, 0.545])

You may also want to preview the TileJSON web service connected to your data. You can do so by adding .json after your web map ID (bmearns.c7); for example, this is done in http://a.tiles.mapbox.com/v3/bmearns.c7.json, which executes the following:

```
{"attribution":"","bounds":[-75.8537,39.7943,-75.6519,39.9376],"center":[-75.7514,39.8715,11],"description":"packt, qgis, c7","download":"http://a.tiles.mapbox.com/v3/bmearns.c7.mbtiles","embed":"http://a.tiles.mapbox.com/v3/bmearns.c7.html","filesize":15349760,"format":"png","grids":["http://a.tiles.mapbox.com/v3/bmearns.c7/{z}/{x}/{y}.grid.json","http://b.tiles.mapbox.com/v3/bmearns.c7/{z}/{x}/{y}.grid.json"],"id":"bmearns.c7","legend":"","maxzoom":16,"minzoom":11,"modified":1435689144009,"name":"c7","private":true,"scheme":"xyz","template":"{{#_location_}}{{/ _location_}}{{#_teaser_}}{{{id}}}{{/ _teaser_}}{{#_full_}}{{{id}}}{{/ _full_}}","tilejson":"2.0.0","tiles":["http://a.tiles.mapbox.com/v3/bmearns.c7/{z}/{x}/{y}.png","http://b.tiles.mapbox.com/v3/bmearns.c7/{z}/{x}/{y}.png"],"version":"1.0.0","webpage":"http://a.tiles.mapbox.com/v3/bmearns.c7/page.html"}
```
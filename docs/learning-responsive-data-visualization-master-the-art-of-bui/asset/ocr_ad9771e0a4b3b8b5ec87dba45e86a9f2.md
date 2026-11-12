# GeoJSON – a format for geographic features

JSON is a great data format for JavaScript, and it works nicely together with D3.
So, the only natural choice is to use a JSON format for geographic data, and this is
exactly what GeoJSON is.

GeoJSON is a very flexible format that can encode anything from the geometries,
features, and collections of features. Let's jump directly into it and take a look at a
typical GeoJSON file:

```
{
    "type": "FeatureCollection",
    "features": [
    {
    "type": "Feature",
    "geometry": {
    "type": "Point", "coordinates": [102.0, 0.5]
    },
    "properties": {
    "prop0": "value0",
    "prop1": {"count": 3}
    }
    }, {
    "type": "Feature",
    "geometry": {
    "type": "LineString",
    "coordinates": [
    [102.0, 0.0], [103.0, 1.0], [104.0, 0.0], [105.0, 1.0]
    ]
    },
    "properties": {
    "prop0": "value1",
    "prop1": {"count": 2}
    }
    }, {
    "type": "Feature",
    "geometry": {
    "type": "Polygon",
    "coordinates": [
    [ [100.0, 0.0], [101.0, 0.0], [101.0, 1.0],
    [100.0, 1.0], [100.0, 0.0] ]
    ]
    },
    "properties": {
```
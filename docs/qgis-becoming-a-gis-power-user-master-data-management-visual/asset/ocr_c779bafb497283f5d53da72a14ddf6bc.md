8. Run the following in the interpreter (refer to the following section to run it noninteractively):

```
import json
f = open('./output/twitter.json', 'r')
jsonStr = f.read()
f.close()
jpy = json.loads(jsonStr)
geojson = ''
for x in jpy['safe']:
    if x['geo'] :
        geojson += '{"type": "Feature","geometry": {"type": "Point", "coordinates": [' + str(x['geo']['coordinates'][1]) + ',' + str(x['geo']['coordinates'][0]) + ']}', "properties": {"id": "' + str(x['id']) + '", "text": "' + x['text'] + '"}}',
geojson = geojson[:-1]
geojson += '}'
geojson = '{"type": "FeatureCollection","features": [' + geojson
f = open('./data/output/twitter.geojson', 'w')
f.write(geojson)
f.close()
```

Or run the following command:

```
> python twitterJson2GeoJson.py
```

Here is an example of the GeoJSON-formatted output:

```
{"type": "FeatureCollection","features": [{"type": "Feature","geometry": {"type": "Point", "coordinates": [-75.75451,39.67434]}, "properties": {"id": "606454366212530177", "text": "Hello world"}},{"type": "Feature","geometry": {"type": "Point", "coordinates": [-75.73968,39.68139]}, "properties": {"id": "606454626456473600", "text": "Testing"}},{"type": "Feature", "geometry": {"type": "Point", "coordinates": [-75.76838,39.69243]}, "properties": {"id": "606479472271826944", "text": "Test"}}]}
```
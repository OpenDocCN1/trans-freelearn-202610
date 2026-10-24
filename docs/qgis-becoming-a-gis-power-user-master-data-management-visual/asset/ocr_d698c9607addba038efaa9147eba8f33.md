```
"http://YOURCARTODBACCOUNTNAMEHERE.cartodb.com/api/v2/sql?q=%20SELECT%20toxic_channels.*%20FROM%20toxic_channels%20INNER%20JOIN%20channels%20ON%20toxic_channels.join_BASIN%20=%20channels.basin%20WHERE%20toxic_channels.join_order%20%3C%20(SELECT%20channels._order%20FROM%20channels%20WHERE%20st_distance(the_geom,%20ST_GeomFromText(%27POINT(" + lon + "%20" + lat + ")%27,4326))%20IN%20(SELECT%20MIN(st_distance(the_geom,%20ST_GeomFromText(%27POINT(" + lon + "%20" + lat + ")%27,4326)))%20FROM%20channels%20x))%20AND%20toxic_channels.join_basin%20=%20(SELECT%20channels.basin%20FROM%20channels%20WHERE%20st_distance(the_geom,%20ST_GeomFromText(%20(%27POINT(" + lon + "%20" + lat + ")%27,4326))%20IN%20(SELECT%20MIN(st_distance(the_geom,%20ST_GeomFromText(%27POINT(" + lon + "%20" + lat + ")%27,4326)))%20FROM%20channels%20x))%20GROUP%20BY%20toxic_channels.cartodb_id%20&api_key=YOURCARTODBAPIKEYHERE&format=geojson&callback=?",
function(geojson) {
    $.each(geojson.features, function(i, feature)
    {
        toxicLayer.addData(feature);
    })
});
    map.addLayer(toxicLayer);
}

getToxic(-75.56111,39.72583);

//getToxic(-75.70993,39.69099);
</script>
</body>
</html>
```
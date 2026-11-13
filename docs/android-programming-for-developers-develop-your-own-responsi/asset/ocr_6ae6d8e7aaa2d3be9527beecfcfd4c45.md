```
map.addMarker(new
MarkerOptions().position(siliconValley).title("Silicon Valley"));
map.moveCamera(CameraUpdateFactory.newLatLng(siliconValley));
}
```

The app will now connect to the Google servers and get the required map data.
In centerMapCallback(), we catch the GLatLng object in the parameter and pass it to the map's setCenter() method to do the actual centering. The second parameter, whose value is 14, is the zoom level. When the API calls for a zoom level, you can supply an integer from zero to seventeen. The higher the number, the closer the zoom will be.

![](bbox=[0.17, 0.24, 0.83, 0.68])

We will not be doing any geocoding in this mashup, but you should still familiarize yourself with GClientGeocoder. We will be using GLatLng quite a bit. Both objects are very important to the Google Maps API. You will find that a mashup often needs both of these objects.
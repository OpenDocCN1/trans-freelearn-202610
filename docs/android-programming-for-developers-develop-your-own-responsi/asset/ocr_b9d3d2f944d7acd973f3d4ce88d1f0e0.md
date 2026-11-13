```
}
// pause the location manager when app is paused/stopped
@Override
protected void onPause() {
    super.onPause();
    locationManager.removeUpdates(this);
}
```

Now, all we need to do is add the required methods of the LocationListener interface.

In onLocationChanged, we get the latitude and longitude by calling getLatitude and getLongitude. Note that we store the result in a variable of the double type, which is just what we need for very precise fractions. Then, we update our TextView widgets.

In onStatusChanged, onProviderEnabled, and onProviderDisabled, we update the TextView widget that displays our provider.

Add the LocationListener methods that we just discussed:

```
@Override
public void onLocationChanged(Location location) {
    double lat = location.getLatitude();
    double lng = location.getLongitude();
    txtLat.setText(String.valueOf(lat));
    txtLong.setText(String.valueOf(lng));
    txtSource.setText("Source = " + provider);
}
```

```
@Override
public void onStatusChanged(String provider, int status, Bundle extras) {
    txtSource.setText("Source = " + provider);
}
```

```
@Override
public void onProviderEnabled(String provider) {
    txtSource.setText("Source = " + provider);
}
```

```
@Override
```
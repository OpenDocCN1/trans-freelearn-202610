```
@Override
public void onClick(View v) {
    double latitude = Double.valueOf(mCursor.getString
    (mCursor.getColumnIndex(DataManager.
    TABLE_ROW_LOCATION_LAT)));
    double longitude = Double.valueOf(mCursor.getString
    (mCursor.getColumnIndex(DataManager.
    TABLE_ROW_LOCATION_LONG)));
    // Create a URI from the latitude and longitude
    String uri = String.format(Locale.ENGLISH,
    "geo:%f,%f", latitude, longitude);
    // Create a Google maps intent
    Intent intent = new Intent(Intent.ACTION_VIEW,
    Uri.parse(uri));
    // Start the maps activity
    getActivity().startActivity(intent);
});
});
return view;
}
```
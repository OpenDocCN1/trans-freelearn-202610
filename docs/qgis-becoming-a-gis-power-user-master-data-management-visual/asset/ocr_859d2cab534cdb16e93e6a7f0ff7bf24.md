+   3. Whether you've downloaded your address data or made up your own, make sure to create a header row. Otherwise, MMQGIS fails to geocode.
+   
+   The following is an example of MMQGIS-friendly address data:

```
id,address,city,state,zip,country
1801300170,44 W CLEVELAND AV,NEWARK,DE,19711,USA
1801400045,85 N COLLEGE AV,NEWARK,DE,19711,USA
1802600068,501 ACADEMY ST,NEWARK,DE,19716,USA
```

+   4. Open the MMQGIS geocode dialog by navigating to **MMQGIS | Geocode | Geocode CSV with Google/OpenStreetMap.**

+   5. Once you've matched your fields to the address input fields available, you have the option of choosing Google Maps or OpenStreetMap. Google Maps usually have a much higher rate of success, while OpenStreetMap has the value of not having a daily limit on the number of addresses you can geocode. At this time, the OSM geocoder produces such poor results as to not be useful.

+   6. You'll want to manually select or input a filesystem path for a `notfound.csv` file for the final input. The default file location can be problematic.

+   7. Once your geocode is complete, you'll see how well the geocode address text matched with our geocoder reference. You may wish to alter addresses in the `notfound.csv` file and attempt to geocode these again.

![](bbox=[0.253, 0.503, 0.747, 0.828])
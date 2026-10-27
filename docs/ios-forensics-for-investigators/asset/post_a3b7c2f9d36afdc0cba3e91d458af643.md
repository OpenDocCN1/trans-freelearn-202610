
Those cell towers. Through this mechanism, Location Services can get a rough position in a few seconds and then report a more accurate GPS-sourced location when available.

Every iOS device also has an internal cache that stores a large quantity of locations, cell towers, and Wi-Fi networks; by accessing the local cache, the device is able to resolve locations from Wi-Fi networks without needing a data connection. To keep this data up to date, the cache database is purged periodically.

Other than this peer-based mechanism, Apple also relies on external sources to translate cell towers and wireless networks into physical locations; wardriving, for instance, is the act of searching for radio signals by a person usually moving in a vehicle, using a laptop or smartphone, recording their own GPS coordinates together with the data pertaining to the radio signals.

## Satellite GPS

Traditional GPS, also known as satellite GPS, is the most accurate source of location data for iOS devices.

This technology is based on a network of over 30 satellites orbiting the earth at an altitude of 20,000 kilometers. Each satellite emits a signal that is transmitted to earth via radio waves, which contains the satellite's identifier, its current position, and a timestamp. Therefore, when a device intercepts a satellite signal, it uses the time difference between the time of signal reception and the broadcast time to compute the distance, or range, from the receiver to the satellite. The device then uses the calculated distance and the satellite's location to calculate its approximate position.

It should be noted that to calculate a precise location, the receiver must be locked on to the signal of at least three satellites. With four or more satellites in view, the receiver can determine its 3D position: **latitude**, **longitude**, and **altitude**.

Certain atmospheric factors and other error sources can affect the accuracy of the device's GPS receiver; however, receivers are typically accurate to

![](img/0b17a17d4cb56b1c25763a396dd83117_171_0.png)

Figure 6.3 – Cell tower trilateration process

It's easy to see how the accuracy of the trilateration process varies so much depending on the location and density of the cell towers.

## Wi-Fi and Bluetooth

Similar to cell towers, Wi-Fi hotspots also broadcast a radio signal and the strength of this signal can be measured through the RSSI. Mobile devices are constantly scanning for wireless hotspots, searching for known networks, so it makes sense to use these to calculate a device's whereabouts. The range of a Wi-Fi signal is only about 70 m outdoors and 40-50 m indoors, so if a device picks up the signal, it must be physically near to the hotspot.

With this in mind, it's clear how mobile devices can evaluate the RSSI of Wi-Fi networks to establish an approximate distance between the device and the hotspot. Then, iOS resolves this data to a physical location using the peer-based mechanism described earlier on.

Bluetooth beacons can also be used in a similar way.

## Locating location artifacts

In the first part of this chapter, we introduced the Location Services API and learned how iOS uses different technologies to assess its position. Now, we'll take an in-depth look at the SQLite databases where geolocation data is stored.

The following is a list of files of interest that can only be found in a full filesystem extraction:

- `/private/var/mobile/Library/Caches/com.apple.routined/Cache.sqlite`

  This is the primary storage means for iOS location data. The ZRTCLLOCATIONMO table contains several useful records, such as GPS coordinates, timestamps, and a horizontal accuracy value, which is an indicator of how accurate the device believes the GPS coordinates to be. The ZRTWIFIACCESSPOINTMO table stores a list of Wi-Fi networks that the device has scanned. Generally speaking, data contained in this database should be considered up to date and accurate.

- `/private/var/mobile/Library/Caches/com.apple.routined/Local.sqlite`

  This SQLite database is identical to the `Cache.sqlite` database in its structure. Depending on the iOS version, the ZRTLEARNEDLOCATIONOFINTERESTSMO table usually contains details for any *frequent locations*, which we will look into later on.

- `/private/var/mobile/Library/Caches/com.apple.routined/Cloud-V2.sqlite`

  As the name suggests, this database stores location data that should be synced to iCloud.

- `/private/var/root/Library/Caches/locationd/cache_encryptedDB.db`

  This database contains harvested location data received by Apple, such as radio cells and Wi-Fi networks; these records refer to cell towers in close proximity to the device, although the accuracy of these geo-coordinates should always be checked.

- `/private/var/root/Library/Caches/locationd/cache_encryptedC.db`

  This database stores data received from the device's accelerometer, such as motion data and generic health-related artifacts.

- `/private/var/root/Library/Caches/locationd/consolidated.db`

  Finally, this database is used by the GeoFence API, and any application that creates a geofence will be stored here.

To gain a better understanding of how location-related data is stored in these databases, we'll analyze each of them separately and we'll learn what queries can be used to extract meaningful insights.

## Analyzing location data

As we've already learned, iOS devices track a multitude of different location-related artifacts. The investigator should have a clear understanding of how and where these are stored and which ones are locations that the user actually visited, versus locations that were simply in close proximity to the device.

The first step in a forensic analysis of location data is the Cache.sqlite database. This database contains an extremely accurate history of coordinates provided by the device's GPS receiver. Typically, this data is stored for just over a week.

We're going to start by looking at the ZRTCLLOCATIONMO table. The following is the table's structure:

- ZLATITUDE and ZLONGITUDE store the geocoordinates.
- The ZALTITUDE column indicates an estimate of the altitude. This data is provided by the device's accelerometer.
- The ZHORIZONTALACCURACY column indicates how accurate the device believes the coordinates to be, expressed in meters.
- ZVERTICALACCURACY indicates the altitude's accuracy, in meters.
- The ZTIMESTAMP column stores the timestamp, in Mac Absolute Time format.

A full history of the device's location data can be obtained by running the following query:

```
SELECT
    DATETIME(ZTIMESTAMP + 978307200, 'UNIXEPOCH') AS "TIMESTAMP",
    ZLATITUDE AS "LATITUDE",
    ZLONGITUDE AS "LONGITUDE",
    ZALTITUDE AS "ALTITUDE",
    ZSPEED AS "SPEED (M/S)",
    ZSPEED*2.23694 AS "SPEED (MPH)",
    ZSPEED*3.6 AS "SPEED (KMPH)",
    ZHORIZONTALACCURACY AS "HORIZONTAL ACCURACY",
    ZVERTICALACCURACY AS "VERTICAL ACCURACY"
FROM
    ZRTCLLOCATIONMO;
```

First, we use the datetime SQL command to convert the Mac Absolute timestamp to a readable string. Then, we select the relevant columns from the table and calculate the speed at which the device was moving.

The following figure shows the query results:

![](img/0b17a17d4cb56b1c25763a396dd83117_175_0.png)

Figure 6.4 – The query results show the device's location

As you can see, location artifacts provide the investigator with an accurate idea of where a certain user's device was at a given moment. Typically, this data is extremely granular and it's not uncommon to see entries logged every second. The differences in the data's granularity can be explained by the fact that GPS is not available everywhere, so on these occasions, the device reverts to using Wi-Fi networks and cell towers to establish its position, if there are any in close proximity.

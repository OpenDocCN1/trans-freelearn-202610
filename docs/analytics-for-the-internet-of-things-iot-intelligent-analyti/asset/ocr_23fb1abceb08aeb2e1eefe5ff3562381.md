+   - Processing geospatial data:
+     - Geospatial analysis software:
+       - ArcGIS
+       - QGIS
+       - ogr2ogr
+     - PostGIS spatial functions
+     - Geospatial and big data
+   - Solving the pollution reporting problem

# Why do you need geospatial analytics for IoT?

Imagine that your company sells a device that measures airborne pollutants. It is internet-enabled and reports data back to your company at regular intervals using MQTT. The target market for this product is environmentally-minded consumers who want to both measure pollutants near their home and contribute to the collective monitoring of the environment.

The value proposition is that they get free analysis of their local air quality in exchange for donating their data to support a cause they probably believe in anyway. Your company is planning to aggregate and package analytics of high-quality air pollution data to sell it to government and private organizations.

Since the device is sold to consumers indirectly through various retail outlets, your company is not initially aware of the location of the devices. The consumer connects the device to the internet after it is purchased, and then enters their addresses. At this point, the location can be determined.

The device has multiple sensors that measure the level of different contaminants in the air. One of the sensors measures the level of nitrogen dioxide (NO2). NO2 is not only a toxic gas by itself but has even more damaging side effects. It facilitates the creation of acid rain and photochemical smog and is a precursor to other harmful secondary air pollutants such as ozone.
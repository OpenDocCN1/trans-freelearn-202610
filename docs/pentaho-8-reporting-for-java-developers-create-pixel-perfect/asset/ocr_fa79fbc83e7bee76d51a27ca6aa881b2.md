properties not so different from the category dataset. They are described in detail, as follows:

+   - In the common properties, the XY series dataset and the XYZ series dataset define `x-value-columns` and `y-value-columns` as the field names of the data source representing the values for the x axis and the y axis respectively. Only for the XYZ series dataset is the `z-value-columns` property also provided.
+   - The series properties, group properties, and optional properties are the same as of the category dataset. Check the category dataset for a detailed explanation of the properties included in the groups.

![](bbox=[0.14, 0.35, 0.21, 0.4])

*If you take a look at Pentaho Reporting 8.0 (as an example), another collector is provided with the name `IntervalXY-SeriesCollector`. Unfortunately, Report Designer does not work for this type of dataset, so we can only suppose that it will be something closed to the datasets detailed here.*
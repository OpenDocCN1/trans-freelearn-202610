DateTime column within the model. The Date tables have the following columns:

+   - Date
+   - Year
+   - Quarter
+   - Month
+   - Day

The last four columns create date hierarchies for each DateTime column. The Date column in the created Date table starts from January 1 of the minimum year of the related column in our tables. It ends on December 31 of the maximum number of years for that column. It is a common practice in data warehousing to use 10/01/1900 for unknown dates in the past and 31/12/9999 for unknown dates in the future. Imagine what happens if we have only one column having only one of the preceding unknown date values. To avoid this, it is a best practice to disable this feature in Power BI Desktop. The following steps show how to disable the auto date/time feature:

+   1. In Power BI Desktop, click the File menu.
+   2. Click Options and settings.
+   3. Click Options.

The following image shows the preceding steps:
+   - If a **Date** table is already available in the source system, we do not need to create another using DAX. It is best to avoid creating multiple **Date** tables unless we have a firm justification to do so.
+   - The **Date** table must have at least one column with the `Date` or `DateTime` data type.
+   - The column containing the date values must be at day granularity (not year/month).
+   - The **Date** column must start from 1st January of the starting year and go up to 31st December of the ending year.
+   - The date range in the **Date** table must be continuous, so there are no gaps between the dates.

It is a good practice to ask the business to provide the date range they would like to cover. We can find the start date from the data model by looking at the fact tables to find the minimum date. But the end date is usually not that simple. So we have to ask the business to understand the date-related requirements. Some prefer the end date to be the 31st December in the current year, while others require a broader date range.

The following two DAX functions can help us to identify the date range:

+   - `CALENDARAUTO()`: Searches across the data model, among all columns with `Date` or `DateTime` data types, and finds the minimum and maximum dates. Finally, it generates one column named **Date**, which starts on 1st January as the first date and finishes on 31st December as the last date. This is quite handy, but you need to be careful. It also considers the *date of birth* or *deceased date*, which may result in irrelevant dates in the model. If the data contains an unknown date from the past (01/01/1900) and an unknown date from the future (31/12/9999), the `CALENDARAUTO()` function also considers those dates, resulting in an unnecessarily large **Date** table. Therefore, we have to tailor the results.
+   - `CALENDAR()`: Accepts a start date and an end date. Both the start and end dates must be in either `Date` or `DateTime` data types. Like
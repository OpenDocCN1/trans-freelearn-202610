Let's consider how the preceding worksheet functions:

+   - First: FIRST() starts at 0 and counts down to the last row of the dataset in the cache. In the preceding screenshot, note that the first instance of Canon imageCLASS occurs on 05/23/24. The fact that FIRST() ranges from 0 to -4 communicates that there are five instances of Canon imageCLASS in the dataset in the cache or within the window cache.
+   - Last: LAST() starts at the last row of the dataset in the cache and counts down to 0. In the preceding screenshot, note that the last instance of Canon imageCLASS occurs on 11/17/25. The fact that LAST() ranges from 4 to 0 communicates that there are five instances of Canon imageCLASS in the dataset in the cache.
+   - First or Last: FIRST() = 0 OR LAST() = 0, when placed on the Filters shelf and set to True, hides all instances of matching rows except the first and last.
+   - The Row ID field is included in the view to make sure that the very first and last instances of Canon imageCLASS display. Otherwise, if there are multiple instances of Canon imageCLASS on the first or last date, the sales numbers will reflect multiple values. It's important to set Compute Using to Table (down) for each table calculation in the view. Compute Using is the same as Addressing, which will be discussed in detail in the Application of functions section.

Real-world scenario: You could use this type of calculation for the time between the last 5 orders per quarter, to see whether the time between orders is increasing or decreasing during the day. This can then help to improve marketing or schedule help desk hours more efficiently.

# Index

The objective of the following worksheet is to list those states in the USA with over 50 postal codes represented in the underlying dataset.

Notice how the following example demonstrates that the INDEX function behaves directionally and can be seen as a count of rows:

+   1. In the workbook associated with this chapter, navigate to the Index worksheet.
+   2. Set the Marks card to Circle.
+   3. Place State on the Rows shelf and Postal Code on the Detail shelf.
+   4. Create a calculated field named Index with the code: INDEX(). Drag Index onto the Filters shelf and select the Range of values filter. Choose to view only values that are 50 or greater by moving the slider to a minimum value of 50.
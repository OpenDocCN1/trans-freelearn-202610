4. Let's see how the filter works. Save your changes and build the Tropical Green project. Once the build is complete, open your browser and navigate to http://www.tropicalgreen.net/TropicalGreen/SearchResults.aspx. Enter ficus in the textbox, check the Only Search Plant Catalog CheckBox, and click the Go button:

![](bbox=[0.08, 0.185, 0.92, 0.49])

Fantastic! We now only see records inside our Tropical Green plant catalog! This gives a good idea of what filtering brings to the table. We could filter by so many things, such as displaying only postings that have been updated in the last month or week. The possibilities are almost endless.

Let's see if we can't clean up those search results by getting rid of the DataGrid and replacing it with a Repeater. At the same time, we'll add filtering of the search results so users will only see postings that they have rights to access.

Although we listed numerous SharePoint index properties in our full text query, we will only use the DAV:href property when analyzing the results in our main results page to obtain a reference to the specified MCMS channel or posting to determine if the user has rights to browse the resource and also to determine and return the actual posting's name and description.

+   1. Open the SearchResults.aspx page in Design view. Delete the DataGrid.
+   2. Drag a Repeater object onto the page where the DataGrid was. Assign the Repeater an ID of rptSearchResults.
+   3. While in Design view, select our new Repeater and open the Properties window. At the top of the window, click the Events button to show all possible events we can use. Double-click the box to the right of ItemDataBound to create an empty event handler that will fire every time an item is bound to the repeater.
+   4. Switch back to HTML view for the SearchResults.aspx and scroll to our new Repeater.
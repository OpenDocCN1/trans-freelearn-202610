4. After clicking OK, Business Central will auto-generate a test scenario based on the values it can detect in our BOM decision model. We show this in Figure 10.19. But note that we’ve already populated some test values on the screen and deleted some suggested columns to make it clearer how things work.

![](bbox=[0.13, 0.17, 0.89, 0.31])

Figure 10.19 – Test values in our Scenario Simulation editor

This format is familiar; it’s the same scenario testing tool we saw in Chapter 9:

+   - Each line is a separate scenario that we want to try out, and we can give each line a name in the Scenario description column.
+   - We pass in the GIVEN columns in the middle of the values during each scenario. The scenario testing tool has recognized our model’s inputs (Product and Qty) and automatically set them up for us.
+   - The columns under the EXPECT heading are the values that should come back from that test run. We’ve tidied up some of the columns suggested and are using the FEEL syntax to represent the key-value pairs we expect back. For example, the first scenario should return this:
+     ```
+     { Grams of Cocoa in Product : 10, Grams of Cocoa in Order : 100, Name : "Crunch" }
+     ```
+   - This syntax is very compact (so it works well in a book example). But you could also use the Test Tools editor (as shown in Figure 20) to create multiple columns and test for a single value in each of them.
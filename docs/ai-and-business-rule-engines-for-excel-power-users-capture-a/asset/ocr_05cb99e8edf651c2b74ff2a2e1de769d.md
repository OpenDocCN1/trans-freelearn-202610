+   3. Open the properties of **Customer Birth Day** to give it a data type of **string**.
+   
+   4. Edit the **Customer Birthday** decision node, and set **Logic type** to **Context**. Of course, there are many different ways of calling a function, but this is a useful excuse to play with context decision nodes.
+   
+   5. In the screen that appears once you’ve chosen the context type, add entries as follows, so it looks like *Figure 4.23*. You will need to insert new lines and select the data type of **string** from the drop-down menus. `DayCalc(Customer.Date of Birth)` and `Day of Week` are literal expressions:

![](bbox=[0.254, 0.294, 0.769, 0.509])

+   6. Open the **Run** panel and select a date from the date picker on the **Date of Birth** input field – for example, **2000-01-01**.
+   
+   7. Immediately, KIE will evaluate the decision model, including the **Customer Birth Day** node we just created. We’ll see what day of the week the customer was born on; in this case, it was a Saturday.

> **Tricks of the trade**
> 
> Adding an output node so we can see what is going on is a useful tool for debugging (trying to find why something is not working the way we want it to). Since nobody gets things right the first time, we talk more about helpful testing techniques in *Chapter 9*.

The **context** node we used was pretty simple: one calculation and an output line. If needed, we could have added additional lines and have KIE build up the result step by step. Again, it’s worth playing with this example in the KIE editor before we move on to build a more sophisticated Decision Table.
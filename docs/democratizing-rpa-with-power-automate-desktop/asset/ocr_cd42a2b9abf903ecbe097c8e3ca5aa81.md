# Figure 6.10 – Invoicing orders into Contoso Invoicing

Again, let’s walk through the steps to understand the details:

+   1. *Step 1* launches the application.
+   2. *Step 2* waits for the main application window to appear.
+   3. *Step 3* clicks the **Invoices** tree node. We are ready to insert invoices now.
+   4. *Step 4* starts a loop and iterates through the data table for new orders.

Notice that I chose to use the indexed version for iteration. This makes it easier at the end to fill in the data for the generated invoice ID because I can reference the current iteration step via a loop index.

In the loop, we do the same for each of the data rows in the table:

+   1. Click the **New** button (*step 6*).
+   2. Fill in the data for the date, account, contact, and amount (*steps 8 to 14*). Here, we use the **Populate text field** action from the **Form filling** actions subgroup. There is also one special thing we need to take care of. Because we extracted the data from the database as text, the currency field contains a value that the Invoicing app does not understand. A conversion is needed to put in the right value. To convert the amount value, we could use the **Recognize entities in text** action. In this case, an entity is something such as an email, an IP address, or a currency value. This action uses artificial intelligence and analyzes the given text, along with the rules on what to detect. The action returns a data table containing all the detected values. It can be configured like this:
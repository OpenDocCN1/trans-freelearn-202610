
![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_166_0.png)

**Figure 6.3 – The "Contoso Invoicing" application**

A desktop icon is also created during the installation, which we will make use of shortly. One additional thing to mention here is that an Excel workbook is also provided by the installation, which contains all the data for this application. This is located in the standard user's documents folder under Contoso Invoicing - 1.0.15.0. We can modify this Excel workbook to our needs and, for example, remove our sample data again.

To be able to create invoices, we must call up the Invoices menu item in the navigation tree, as shown in the preceding screenshot, and then enter the data. This outlines a thoroughly typical example of the setup in small and medium-sized companies.

## Deconstructing the workflow

This is a non-trivial flow, so it is best practice to cut this into smaller pieces. Those smaller flows will be easier to maintain and test since with the interaction of UI elements, a lot of actions will always be used:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_167_0.png)

**Figure 6.4 – Deconstructing the Orders to Invoices workflow**

1. To identify which new invoices need to be created, we first need to identify these in the Northwind database and store the orders in a data table. Next, we must iterate through this data table and create new invoices in the Contoso Invoicing application. Invoice IDs get generated for each new invoice that we can also save in the data table.
2. The last routine takes these new invoice IDs and updates the database with that information.

Hint: All three routines are separated into the following subflows. The reason is to identify issues related to a specific action and improve the maintenance of flows:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_167_1.png)

**Figure 6.5 – Subflows for Orders to Invoices**

To create the whole workflow, we will need to apply all the knowledge that has been discussed throughout this book, which is flow control, variables, and UI elements. Let’s take a look at the different subflows we can use to accomplish these tasks.

### The GetNorthWindNewOrders subflow

In this subflow, we want to start the database application and, in the end, get a list of all the new orders that have arrived in the database.

To remote control the application, we need to define or capture the corresponding UI elements that we want to use to achieve the desired result. In the previous chapter, we learned how to do that. In addition, some variables are defined. This is the detailed flow to get new orders:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_168_0.png)

**Figure 6.6 – Subflow to extract new orders**

These are some comments on the different steps (line numbers).

*Step 2* starts Microsoft Access with the Northwind Database. I used two variables here. `MSAccessApp` contains the full path to the executable of Microsoft Access, while `NorthWindDB` contains the path to the database itself.

In *step 3*, we use the first UI action, called **Wait for window**. Old programs or computers with less power (processor, RAM, and so on) might have some latency when starting a program. This action will pause the flow until the given window appears. In this case, the flow waits for the login screen to appear. The login window was captured as a UI element previously.

*Step 4* triggers the click on the login button. As shown in *Figure 6.2*, the database application first wants a user to log in. Here, we just take the first user and press the login button.

Now we should wait for the main application window to appear. That’s why we use the **Wait for window** action again in *step 5*.

In *step 6*, I used an image to be able to address the **view orders** link on the right-hand side of the application. The recorder was not able to detect this as a separate UI element, so I had to use the image functionality of PAD to move the mouse to this link and click on it. This is a good example showing that even those cases can be addressed by PAD.

In *steps 8 to 11*, I am using MS Access ribbon buttons to filter the list of orders so that it only contains new orders, selecting all of them, and copying them to the clipboard. This allows you to parse the content in the next subflow.

Please notice that we are not closing the application at this point.

### The ExtractNewOrders subflow

At this point, we only have some text in the clipboard. But to be able to iterate through the list of new orders, we somehow need to convert this text into a data table. The following figure shows the flow and how to accomplish this:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_170_0.png)

**Figure 6.7 – Generating a data table from clipboard text**

After we stored the clipboard text in step 1 into a text variable by using Get clipboard text in the Clipboard actions group, we can now split this text into a text list (step 3).

Next, we create a data table variable with the right columns. These are all columns from the MS Access orders table, plus one additional column for the invoice ID, which gets generated later by the Contoso Invoicing application:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_171_0.png)

**Figure 6.8 – The internal order data table definition**

In step 5, we start to iterate through the text list, and for each entry in that list, we need to split the text again to get the single values for each column. These get stored in the data table in step 7.

Finally, we need to clean up the data table and delete the first two rows (row 1 was empty and created while defining the data table, while row 2 contains the column header information from the clipboard text).

Now, we have a data table containing all the new orders that we can work with.

### The InvoiceOrders subflow

This subflow is responsible for creating the invoices in the Contoso Invoicing application. Here, we also need to identify the corresponding UI elements first, such as the Invoice tree node, the new (add button icon) and save (add button icon) buttons, and so on. The following figure shows the required UI elements:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_172_0.png)

**Figure 6.9 – UI elements in Contoso Invoicing**

As we can see, we need more UI actions to fill in and also extract the data. The following figure shows the flow that does the required steps for us:

- Save
- Run
- Stop
- Run next action
- Recorder
- Subflows
- Main
- ExtractNewOrders
- GetNorthWindNewOrd...
- InvoiceOrders
- WriteBackInvoiceIDs

| 步骤 | 动作类型 | 动作详情/注释 |
|------|---------|---------------|
| 1 | Run application | Run application ContosoInvoiceApp with arguments and store its process ID into ContosoInvoiceAppID |
| 2 | Wait for window | Wait for window Window 'Contoso Invoicing' to open |
| 3 | Click UI element in window | Click on UI element Treeviewbutton 'Invoices' |
| 4 | Loop | LoopIndex from 0 to NewOrders.RowsCount - 1 with step 1 |
| 5 |  | Click the 'New' Button |
| 6 | Click UI element in window | Click on UI element Button 'New' |
| 7 |  | Fill in Date, Account, Contact and Amount |
| 8 | Populate text field in window | Populate text box Textbox 'Date' with NewOrders [ LoopIndex ][1] |
| 9 | Populate text field in window | Populate text box Textbox 'Account' with NewOrders [ LoopIndex ][4] |
| 10 | Populate text field in window | Populate text box Textbox 'Contact' with NewOrders [ LoopIndex ][3] |
| 11 | Recognize entities in text | Recognize all the Currency entities in text NewOrders [ LoopIndex ][8] and store the results into RecognizedEntities |
| 12 | Set variable | Assign to variable AmountNumber the value RecognizedEntities [0][0] |
| 13 | Replace text | Replace text '\' with '' in AmountNumber and store the result into AmountNumber |
| 14 | Populate text field in window | Populate text box Textbox 'Amount' with AmountNumber |
| 15 |  | Extract InvoiceID |
| 16 | Extract data from window | Extract all records from the UI element Textbox 'InvoiceID' and store the result(s) into InvoiceID |
| 17 |  | Fill in InvoiceID in Datatable |
| 18 | Update data table item | Updates the data table NewOrders at row index LoopIndex and column 9 with the value InvoiceID |
| 19 |  | click the 'Save' button |
| 20 | Click UI element in window | Click on UI element Button 'Save' |
| 21 | End |  |
| 22 | Terminate process | Terminate process with ID ContosoInvoiceAppID |

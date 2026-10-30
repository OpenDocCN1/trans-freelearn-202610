
**Figure 6.10 – Invoicing orders into Contoso Invoicing**

Again, let’s walk through the steps to understand the details:

1. *Step 1* launches the application.
2. *Step 2* waits for the main application window to appear.
3. *Step 3* clicks the **Invoices** tree node. We are ready to insert invoices now.
4. *Step 4* starts a loop and iterates through the data table for new orders.

Notice that I chose to use the indexed version for iteration. This makes it easier at the end to fill in the data for the generated invoice ID because I can reference the current iteration step via a loop index.

In the loop, we do the same for each of the data rows in the table:

1. Click the **New** button (*step 6*).
2. Fill in the data for the date, account, contact, and amount (*steps 8 to 14*). Here, we use the **Populate text field** action from the **Form filling** actions subgroup. There is also one special thing we need to take care of. Because we extracted the data from the database as text, the currency field contains a value that the Invoicing app does not understand. A conversion is needed to put in the right value. To convert the amount value, we could use the **Recognize entities in text** action. In this case, an entity is something such as an email, an IP address, or a currency value. This action uses artificial intelligence and analyzes the given text, along with the rules on what to detect. The action returns a data table containing all the detected values. It can be configured like this:

> **Recognize entities in text**
> Recognizes entities in text, such as numbers, units, date/time and others expressed in natural language across multiple languages More info
>
> Select parameters
>
> Text to recognize from %NewOrders[LoopIndex][8]% {x}
> Entity type: Currency
> Language: English
>
> Variables produced RecognizedEntities
>
> Save Cancel

**Figure 6.11 – The Recognize entities in text configuration**

3. Extract data for the generated invoice ID and store it in the last element of the data row. Here, we make use of the Extract data from window action to capture the generated invoice ID.
4. Step 18 updates the orders data table with the extracted invoice ID.
5. In step 20, we click the Save button.

At this stage, we have established the communication between the two applications and inserted new orders as invoices into the Contoso Invoicing app. In addition, we now know all the invoice IDs for each order so that we can update the MS Access database itself for reference. Let’s see what we need to do here.

### The WriteBackInvoiceIDs subflow

Finally, we want to update the MS Access database so that users can see that invoices have been created. Now, this could be facilitated in many ways. Unfortunately, the database itself does not provide us with an easy way to mark an order to be invoiced. That’s why we can just choose an easy way to pass this information back into the database by updating the Notes field in each order. We will do that by dynamically creating a SQL statement that contains the order ID, as well as the invoice ID (both are in our data table). This statement could be like this, for example:

```sql
UPDATE Orders SET Notes = "Invoice ID: 1159" WHERE [Order ID] = 59
```

The flow that does this for us is displayed in the following figure:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_176_0.png)

**Figure 6.12 – Navigating to the SQL pane**

To be able to paste the query text into MS Access, we need PAD to navigate to a place where we can work with queries. You can do this by clicking the Create ribbon, then the Query Design button. After that, we need to switch to the SQL view to be able to enter the query there. This is what the flow does in *steps 3 to 6*.

Please notice that we are running through this routine for every record in the orders data table. In *step 8*, we create the updated SQL statement and again use the form-filling UI action called **Populate text field in window**.

There is a **Run** button in MS Access that can be used to execute the query. In *step 9*, we instruct PAD to press this button. The default behavior of MS Access is to make a dialog appear, after which we have to confirm the action. In *step 12*, we tell PAD to do so.

Next, we want to close the query window and start over again. In *step 13*, we press the button to close the query window; again, we need to let PAD confirm that we do not want to store the query. This marks the end of the loop so that we can start over again:

```text
Subflows

NewOrders
InvoiceOrders
WriteBackInvoiceIDs

For each CurrentItem in NewOrders

navigate to SQL input pane

Click UI element in window
Click on UI element Tab Item 'Create'

Click UI element in window
Click on UI element Button 'Query Design'

Click UI element in window
Click on UI element MenuItem: More Options

Click UI element in window
Click on UI element MenuItem: SQL View

create dynamic SQL statement

Populate text field in window
Populate text box Pane 'Query1' with 'UPDATE Orders SET Notes = "Invoice ID: ' + CurrentItem[9] + '" WHERE [Order ID] = ' + CurrentItem[0]

Click UI element in window
Click on UI element Button 'Run'

Wait for window
Wait for window Window 'Microsoft Access' to open

close the query and start over

Click UI element in window
Click on UI element Button 'Yes'

Click UI element in window
Click on UI element Button 'NetUIDocumentTabCloseButton'

Wait for window
```

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_179_0.png)

**Figure 6.13 – Updating MS Access with invoice information**

This whole procedure seems a bit cumbersome at first glance, but it reveals the following:

- PAD is fully capable of interacting with applications and confirming dialogs
- It does not matter whether it is an application of Microsoft or not, because the concept of UI elements is universal
- PAD can extract data from other applications and use them with UI elements to create further actions
- If the task presented here has to be performed once or even several times a day (which could well be the case), then the integration and automation shown here pay off several times more

In addition to these important points, I would like to make the following comments:

- As already mentioned, there is also a billing function in the database. However, before an invoice can be created, further conditions must be fulfilled, which I have ignored for the benefit of this example.
- When handing over the amount, we have not performed any currency conversion for the sake of simplicity.
- All the UI elements that we used were previously inserted into PAD via the recorder. [Chapter 5](Chapter 5) described how this can be achieved in detail.

In this section, we learned how PAD bridges the gap between two different applications that need to exchange data with each other. By performing data extraction, we can read information from one application, convert it into another format, and insert it back into a second application by using form-filling UI actions.

## Summary

In this chapter, we learned how PAD allows two applications to exchange data that would otherwise have no connection. By defining UI elements and using UI actions, any application can be remotely controlled and thus integrated into a larger automation context. Here, PAD acts as a high-level entity that allows the application landscapes to communicate with each other; otherwise, no data exchange would be possible at all unless you were to do it manually.

In the next chapter, we will look at other actions that can be used to manage and remotely control a computer, its files, and its services.

## Further reading

The following walkthrough, *Microsoft ‘RPA in a Day’ Training Material Update*, provides a full course with different modules that also leverage the sample application used in this chapter. The article contains a link for you to download the material. In the `Lab data packages` folder, there is another folder called `Prerequisites` that contains the installer for the Windows application: https://powerautomate.microsoft.com/en-us/blog/announcing-microsoft-rpa-in-a-day-version-2/.

UI automation reference: https://learn.microsoft.com/en-us/power-automate/desktop-flows/actions-reference/uiautomation

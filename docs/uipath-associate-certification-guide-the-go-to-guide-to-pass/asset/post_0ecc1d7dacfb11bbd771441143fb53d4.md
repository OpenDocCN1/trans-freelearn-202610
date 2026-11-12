

- 12. Now that we have captured the invoice date, let’s save it into our already created argument. Under the **Properties** panel, pass out_invoiceDate in the **Value** section of the **Output** property.

We have successfully extracted the invoice date. Now, let’s extract the invoice number.

- 13. Drag and drop the Get Text activity below the Get Text activity of the invoice date. You will use this Get Text activity to capture the invoice number from the PDF.

- 14. Click on **Indicate element inside window**, hover the mouse cursor on the **Invoice number** field of the invoice, and then record it by doing a right-click.

- 15. Let’s look at the selector of this Get Text activity. In this case, the invoice number mentioned in the invoice is captured by the selector. Here is the part of the selector:

```
<ctrl name=' Invoice No # 5321 ' role='text' />
```

As we want the selector to be dynamic to work with any other invoice number provided, we will use the dynamic selector concepts again. Replace the 5321 part with * and click on the **Validate** button. The **Validate** button turns green, which confirms that the new selector is correct. Your new selector code should look as follows:

```
<ctrl name=' Invoice No # * ' role='text' />
```

* will make sure that it works for any invoice number provided in the invoice:

![](img/84b06ad2832c53c5f677f8ce4c80f184_455_0.png)

Figure 14.33 – The Selector Editor window of the Get Text activity for the invoice number

- 16. Now that we have captured the invoice number, let's save it into our already created argument. Under the **Properties** panel, pass out_invoiceNumber in the **Value** section of the **Output** property.

> > **Note**
> >
> > The selector for the invoice date and invoice number is partially generated because both Get Text activities are used inside the Attach Window activity.

Let's extract the invoice particulars now. Since the data is structured data, presented in the form of a table, we will use the data scraping wizard tool of UiPath Studio to extract it:

- 1. Click on the Data Scraping button from the DESIGN ribbon bar. A new Extract Wizard window opens up; then, click on the Next button to start the data scraping process:

![](img/84b06ad2832c53c5f677f8ce4c80f184_456_0.png)

Figure 14.34 – The data scraping wizard

- 2. Hover the cursor over any of the table cells and right-click on it:

| Qty | Description | Unit Price | Line Total |
| --- | --- | --- | --- |
| 2 | Avocado and chickpea pasta | 10.00 | 20.00 |
| 5 | Avocado and grapefruit bagel | 8.00 | 40.00 |
| 1 | Apricot and date crumble | 10.00 | 10.00 |
| 1 | Cheddar cheese and jujube salad | 15.00 | 15.00 |
| 1 | Mascarpone and mulberry salad | 20.00 | 20.00 |
| | | Subtotal | 105.00 |
| | | Shipping | 2.00 |
| | | Total | $107.00 |

Figure 14.35 – Selecting a table cell

- 3. A new window appears, which shows all the extracted table cell values. Enter 0 (zero) in the **Maximum number of results** field and click on the **Finish** button:

### Preview Data

| Qty | Description | Unit Price | Line Total |
| --- | --- | --- | --- |
| 2 | Avocado and chickpea pasta | 10.00 | 20.00 |
| 5 | Avocado and grapefruit bagel | 8.00 | 40.00 |
| 1 | Apricot and date crumble | 10.00 | 10.00 |
| 1 | Cheddar cheese and jujube salad | 15.00 | 15.00 |
| 1 | Mascarpone and mulberry salad | 20.00 | 20.00 |
| | | Subtotal | 105.00 |
| | | Shipping | 2.00 |
| | | Total | $107.00 |

Edit Data Definition Maximum number of results (0 for all) 0

Help < Back Finish Cancel

- 4. Clicking on the **Finish** button places the **Extract Structured Data** activity in Studio inside an **Attach Window** activity:

![](img/84b06ad2832c53c5f677f8ce4c80f184_458_0.png)

Figure 14.37 – The Extract Structured Data activity

- 5. Edit the selector for the **Attach Window** activity, since it contains the customer's name. The edited selector should look as follows:

```
<wnd app='acrord32.exe' cls='AcrobatSDIWindow'
title='Invoice-Henry FM-*.pdf - Adobe Acrobat Reader DC
(32-b*' />
```

- 6. The data scraping wizard will extract the data in a **DataTable** format. Save it into your already created **DataTable** argument. Under the **Properties** panel, pass **out_dt_invoiceParticulars** in the **DataTable** section of the **Output** property.

- 7. Finally, drag and drop the **Close Application** activity, click on **Indicate element inside window**, hover the mouse on Adobe Acrobat Reader, and right-click on it. This will close the PDF reader after extracting all the required fields from it.

- 8. Return to the Read_Invoice sequence. From the **Project** panel, drag and drop the Extract_Invoice_Particular.xaml workflow below the InvokeExtract_Customer_Details workflow activity in the body of the For Each activity:

![](img/84b06ad2832c53c5f677f8ce4c80f184_459_0.png)

Figure 14.38 – Invoking Extract_Invoice_Particular.xaml

- 9. Click on the **Import Arguments** button and pass the `invoicePath` variable as the value to the `in_InvoicePath` argument.

- 10. Create three new variables using `Ctrl + K`, named `invoiceDate`, `invoiceNumber`, and `dt_invoiceParticulars`, and pass them as **Value** to all the **Out** arguments, as shown in the following screenshot:

| Name | Direction | Type | Value |
| :--- | :--- | :--- | :--- |
| in_invoicePath | In | String | invoicePath |
| out_invoiceDate | Out | String | invoiceDate |
| out_invoiceNumber | Out | String | invoiceNumber |
| out_dt_invoiceParticulars | Out | DataTable | dt_invoiceParticulars |

Figure 14.39 – The import arguments

Congratulations! You have extracted all the necessary fields from the invoice. Let’s upload all these values to the Orchestrator queues.

## Uploading the invoice data to the Orchestrator queues

In this section, you will upload the customer details and invoice particulars to the Orchestrator queues. The following diagram explains the high-level process of uploading data to the queues:

![](img/84b06ad2832c53c5f677f8ce4c80f184_460_0.png)

Figure 14.40 – Uploading to the Orchestrator queue

Uploading the customer details and invoice particulars to the queue will help you build the performer model of this process where you can consume the data from the queue and then process it.

Let's get started:

- 1. First, let's create a queue in Orchestrator. Click on the queue section and create a new queue. Type in HenryFoodMartInvoice as the queue name and click on the Add button to create the queue:

![](img/84b06ad2832c53c5f677f8ce4c80f184_461_0.png)

Figure 14.41 – Creating a queue

- 2. In Studio, create a new sequence and name it Upload_To_Queue.

- 3. Then, click on the Imports panel and search for the Newtonsoft.Json namespace and add it. You will use this for serializing the in_dt_invoiceParticulars arguments so that the entire DataTable can be added in a single transaction in the queue:

![](img/84b06ad2832c53c5f677f8ce4c80f184_462_0.png)

Figure 14.42 – Adding the Newtonsoft.Json namespace

- 4. Create seven In arguments, as shown in the following screenshot:

| Name | Direction | Argument type | Default value |
|---|---|---|---|
| in_invoiceDate | In | String | Enter a VB expression |
| in_invoiceNumber | In | String | Enter a VB expression |
| in_customerName | In | String | Enter a VB expression |
| in_customerAddress | In | String | Enter a VB expression |
| in_customerPhoneNo | In | String | Enter a VB expression |
| in_customerEmail | In | String | Enter a VB expression |
| in_dt_invoiceParticulars | In | DataTable | Enter a VB expression |

Figure 14.43 – The Arguments panel

- 5. Drag and drop a Try Catch activity as the first activity.

- 6. Then, drag and drop the Add Queue Item activity inside the Try block. Then, under the Properties panel of the Add Queue Item activity, type in "HenryFoodMartInvoice" as the queue name in the QueueName section.

- 7. In this scenario, the queue was created under the folder named Packt in Orchestrator, so you must pass the folder name in the Add Queue Item activity properties. In the Folder Path section, type in "Packt":

![](img/84b06ad2832c53c5f677f8ce4c80f184_463_0.png)

Figure 14.44 – The Properties panel of the Add Queue Item activity


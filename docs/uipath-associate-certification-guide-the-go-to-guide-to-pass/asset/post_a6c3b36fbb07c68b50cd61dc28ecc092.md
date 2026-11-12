
8. Under the **Properties** panel, click on the ellipsis icon of the **ItemInformation** property:

| Property | Value |
|---|---|
| Deadline | The date before which the queue... |
| ItemInformation | (Collection) |
| ItemInformationCollection | A collection variable of addition... |
| Postpone | The date after which the queue... |
| Priority | Normal |
| QueueName | "HenryFoodMartInvoice" |

Figure 14.45 – The ItemInformation collection property

9. In the **ItemInformation** window, create seven arguments called invoiceDate, invoiceNumber, customerName, customerAddress, customerPhoneNo, customerEmail, and invoiceParticulars of the String type.

10. Let's assign values to all the arguments:

- invoiceDate = in_invoiceDate.Split(":", StringSplitOptions.None)(1).ToString.Trim
- invoiceNumber = in_invoiceNumber.Split("#", StringSplitOptions.None)(1).ToString.Trim
- customerName = in_customerName
- customerAddress = in_customerAddress
- customerPhoneNo = in_customerPhoneNo
- customerEmail = in_customerEmail
- invoiceParticulars = JsonConvert.SerializeObject(in_dt_invoiceParticulars)

After assigning all the values in the **Value** section, your **ItemInformation** window should look as follows:

| Name | Direction | Type | Value |
|---|---|---|---|
| invoiceDate | In | String | in_invoiceDate.Split(":", StringSplitOptions.None)(1).ToString.Trim |
| invoiceNumber | In | String | in_invoiceNumber.Split("#", StringSplitOptions.None)(1).ToString.Trim |
| customerName | In | String | in_customerName |
| customerAddress | In | String | in_customerAddress |
| customerPhoneNo | In | String | in_customerPhoneNo |
| customerEmail | In | String | in_customerEmail |
| invoiceParticulars | In | String | JsonConvert.SerializeObject(in_dt_invoiceParticulars) |

Figure 14.46 – The ItemInformation window with all the values assigned

11. The invoice date and invoice number have extra characters in them. To get rid of them, use the Split() function with the appropriate delimiter. As mentioned earlier, the DataTable arguments have to be serialized before uploading to the queue. The SerializeObject() function of the JsonConvert class will help serialize it into a JSON object.

12. Finally, inside the Catches block, catch a SystemException.

13. Also, log a message, with Log Level as Error and Message as "Unable to Upload to Queue" + vbLf + exception.Message + vbLf + exception.Source.

14. Return to the Read_Invoice sequence, and from the **Project** panel, drag and drop the Upload_To_Queue.xaml workflow below the Invoke_Extract_Invoice_Particular workflow activity in the body of the For Each activity:

![](img/84b06ad2832c53c5f677f8ce4c80f184_466_0.png)

Figure 14.47 – Invoking Upload_to_Queue.xaml

15. Click on the **Import Arguments** button and pass the variables for all the seven In arguments, as shown in the following screenshot:

| Name | Direction | Type | Value |
|---|---|---|---|
| in_invoiceDate | In | String | invoiceDate |
| in_invoiceNumber | In | String | invoiceNumber |
| in_customerName | In | String | customerName |
| in_customerAddress | In | String | customerAddress |
| in_customerPhoneNo | In | String | customerPhoneNo |
| in_customerEmail | In | String | customerEmail |
| in_dt_invoiceParticulars | In | DataTable | dt_invoiceParticulars |

Figure 14.48 – Importing the arguments

Now that you have completed the uploading to the queue section, it is time to assemble the Main.xaml file to run the process.

## Assembling Main.xaml

It’s time to run the process, but let’s invoke the Download_Invoice.xaml and Read_Invoice.xaml workflows in the Main.xaml file before running the process:

1. In the Main.xaml workflow, create a variable named isMail of the boolean type. Then, drag and drop the Download_Invoice.xaml workflow in the Main.xaml workflow from the Project panel.

2. Click on the **Import Arguments** button and pass the isMail variable as the value to the out_isMail argument:

![](img/84b06ad2832c53c5f677f8ce4c80f184_468_0.png)

| Name | Direction | Type | Value |
|---|---|---|---|
| out_isMail | Out | Boolean | isMail |
| Create Argument | | | |

Figure 14.49 - The Invoke Download_Invoice workflow

3. Next, drag and drop the **If** activity and pass the **isMail** variable in **Condition**. If the **isMail** variable value is **True**, then you can start reading the invoices; otherwise, you will stop the process.

4. Drag and drop the **Read_Invoice.xaml** workflow inside the **Then** block of the **If** activity from the **Project** panel.

5. Below that, add a **Log Message** activity at the **Info** level and set the message to "All invoices details uploaded to queue".

6. In the Else block, add a Log Message activity at the **Info** level and set the message to "No invoice to process":

![](img/84b06ad2832c53c5f677f8ce4c80f184_469_0.png)

Figure 14.50 – The Main.xaml workflow

7. Now, let's run the entire process by clicking on the **Run** button from the **DESIGN** ribbon bar. The process completes without any errors, and all the invoice details are uploaded to the Orchestrator queue:

![](img/84b06ad2832c53c5f677f8ce4c80f184_470_0.png)

Figure 14.51 – The transaction item from the queue

Congratulations! You have successfully built the automation for the dispatcher model of invoice processing. Now, you can publish the code to Orchestrator to run the bot from the Orchestrator page or the UiPath Assistant.

## Summary

In this chapter, you developed an automation process that scrapes relevant data from invoices for further processing. First, the robot read through emails and downloaded the invoices, received in the form of an email attachment as a PDF. Then, it extracted specific data from the invoices and uploaded them to the Orchestrator queue.

In the next chapter, you will learn how to build the performer model to consume values from the Orchestrator queues and punch them into the order entry CRM.

## 15

## Invoice Processing - Performer

In Henry's Food Mart, the finance and accounting department has selected the Invoice Processing process for automation. The Invoice Processing process reads through emails and downloads the invoices that have been received in an email attachment as a PDF. Then, it extracts customer information and invoice particulars from the invoices and enters them onto the Order Entry CRM web page.

You started building this process by following the Dispatcher-Performer model. In Chapter 14, Invoice Processing – Dispatcher, you learned how to build an automation that downloads the unread invoices from emails, extracts the customer details and invoice particulars from each invoice, and then uploads them to the Orchestrator queue named HenryFoodMartInvoice.

In this chapter, you will build the Performer model to enter the invoice data onto the Order Entry CRM web page.

The following is a list of what you will learn as part of this Invoice Processing – Performer project:

- Browser automation
- Working with different types of workflows
- Consuming Orchestrator queues
- Modifying selectors
- Logging in UiPath Studio
- Handling exceptions
- Deserializing JSON
- Working with DataTables
- Updating the queue status

### Technical requirements

Let's understand the requirements that you will need for this project:

- UiPath Studio Community/Enterprise/Academic Alliance Edition version 20.10 and greater, which must be connected to the UiPath Orchestrator service.
- Access to the UiPath Orchestrator service
- The Order Entry CRM web page
- A Google Chrome browser
- Access to the internet

You can get the entire code for the invoice processing dispatcher from GitHub at https://github.com/PacktPublishing/UiPath-Associate-Certification-Guide/tree/main/Chapter15/Solution.

### Project overview

In this chapter, you will be designing the performer model of the invoice processing process. Let's understand this process in detail. Here are the following steps that you should perform:

1. Program a robot to start the Order Entry CRM web page.
2. Program the robot to read transaction items from the queue named **HenryFoodMartInvoice** in the Orchestrator workflow.
3. Program the robot to extract the following relevant fields from the transaction item that is read from the queue:
    - **Customer Details:**
        - Customer name
        - Customer address
        - Customer phone number
        - Customer email ID
    - **Invoice Particulars:**
        - Invoice date
        - Invoice number
    - **Order Information:**
        - Item quantity
        - Item description
        - Item unit price
        - Line total amount
        - Subtotal amount
        - Shipping amount
        - Total amount

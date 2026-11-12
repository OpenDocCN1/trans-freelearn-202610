
- 29. Pass the `flag` variable under the **Condition** property of the `Flow Decision` activity. Doing this will ensure that if the outcome of the `flag` variable is True, you can proceed further with the new transaction item. Else, if the outcome of the `flag` variable is False, that means there was an exception while entering the details of the invoice particulars onto the Order Entry CRM web page:

![](img/84b06ad2832c53c5f677f8ce4c80f184_500_0.png)

Figure 15.31 – The Flow Decision activity

Now you have completed entering invoice particulars details in the Order Entry CRM web page. In the next section, let's set the status of the transaction item depending upon the outcome of the `flag` variable in the `Flow Decision` activity.

## Setting the transaction item status

In this section, for every transaction item read and processed, you will set the status to **Successful** or **Failed**:

- For the `True` branch of the `Flow Decision` activity from the `Invoke Enter_Invoice_Particulars` workflow activity, you will set the transaction item's **Status** to **Successful**.
- For the `False` branch of the remaining `Flow Decision` activity, you will set the transaction item's **Status** to **Failed**.

So, let's set the status of the transaction item:

- 1. Drag and drop the `Set Transaction Status` activity that connects the `True` branch:

![](img/84b06ad2832c53c5f677f8ce4c80f184_501_0.png)

Figure 15.32 – A successful transaction

- 2. Edit the properties of the Set Transaction Status, as follows:

  - Set Status to Successful.
  - Pass the transactionItem variable into the TransactionItem text field.
  - For this process, queues were created in the folder named Packt inside the Orchestrator workflow. Hence, you will type "Packt" into the Folder Path section:

![](img/84b06ad2832c53c5f677f8ce4c80f184_501_1.png)

Figure 15.33 – The Properties panel of the successful Set Transaction Status activity

- 3. Connect the Set Transaction Status activity to the Fetch transaction item sequence. Doing this will allow the bot to go and fetch the next transaction item from the queue and process it.

- 4. Drag and drop a new Set Transaction Status activity connecting the False branch, as shown in the following screenshot:

![](img/84b06ad2832c53c5f677f8ce4c80f184_502_0.png)

Figure 15.34 – A failed transaction

- 5. Edit the properties of Set Transaction Status, as follows:

  - Set Status to Failed.
  - Pass the transactionItem variable into the TransactionItem text field.
  - Type "Packt" into the Folder Path section.
  - Set ErrorType to Application.
  - Type "Failed while retrieving the transaction item or entering invoice data" into the Reason section:

![](img/84b06ad2832c53c5f677f8ce4c80f184_503_0.png)

Figure 15.35 – The Properties panel of the failed Set Transaction Status activity

Setting the transactionItem status to Failed will update the status in Orchestrator from In Progress to Retried. A new transaction item with a New status will be added to the queue for every Retried status.

Now that the status of transactionItem has been set to Failed, let's look at how you can fetch the next transaction item from the queue.

## Closing and opening the CRM

Since the status of the transaction item has been set to fail, before fetching the next transaction from the queue, you will need to close the Order Entry CRM web page and open the CRM again:

- 1. In the Read_From_Queue workflow, drag and drop a Sequence activity connecting to the Set Transaction Status (Failed) activity. Then, rename it to Close and Open the CRM.

- 2. Inside the Close and Open the CRM sequence, drag and drop the Element Exists activity and indicate Henry's Food Mart Order Entry on the Order Entry CRM web page. In the Properties panel, create a new variable under Exists, named isCrmExists, using Ctrl + K.

The following screenshot shows Henry's Food Mart Order Entry being indicated by the Element Exists activity:

![](img/84b06ad2832c53c5f677f8ce4c80f184_504_0.png)

Figure 15.36 – The Element Exists activity on the CRM page

Before closing the CRM, you can check whether the CRM is open or has already been closed. The Element Exists activity will return a Boolean as the output:

- TRUE means the CRM is still open.
- FALSE means the CRM is not available.

TRUE means the robot will need to close the CRM and open the CRM again, and FALSE means the robot will only need to open the CRM again because it is already closed.

- 3. To achieve the previously mentioned logic, you will use the If activity below the Element Exists activity. Pass isCrmExists into the Condition field.

- 4. In the Then block of the If activity, drag and drop the Attach Browser activity, attach the already opened Order Entry CRM web page, and place the Close Tab activity inside the scope of the Attach Browser activity.

- 5. Below the Attach Browser activity, drag and drop the Open Browser activity and type Environment.CurrentDirectory+"\\CRM\\Order Entry.html" into the Url field. Then, place the Maximize Window activity inside the scope of the Open Browser activity.

- 6. In the Else block of the If activity, drag and drop the Open Browser activity and type Environment.CurrentDirectory+"\\CRM\\Order Entry.html" into the Url field. Then, place the Maximize Window activity inside the scope of the Open Browser activity:

![](img/84b06ad2832c53c5f677f8ce4c80f184_505_0.png)

![](img/84b06ad2832c53c5f677f8ce4c80f184_505_1.png)

Figure 15.37 – The Close and Open the CRM sequence

- 7. Navigate back to the Read_From_Queue workflow and connect the Close and Open the CRM sequence to the Fetch transaction item sequence.

If the transaction item fails, you have successfully built a piece of logic to close and open the CRM again. Next, it's time to assemble the Main.xaml file to run the process.

## Assembling the Main.xaml file

It's time to run the process, but let's invoke the Read_From_Queue workflow inside the Main.xaml file before running the process:

- 1. In the Main.xaml workflow, from the **Project** panel, drag and drop the Read_From_Queue workflow underneath the Maximize Window activity.
- 2. Add a Close Tab activity below it:

![](img/84b06ad2832c53c5f677f8ce4c80f184_506_0.png)

Figure 15.38 – The Main.xaml workflow

Now, let's run the entire process by clicking on the **Run** button from the **Design** ribbon bar. The process completes without any errors, and all the new transaction items from the queue have been processed and marked as successful:

## Queues / Transactions: HenryFoodMartInvoice

| Status | Reference | Revision | Priority |
| :--- | :--- | :--- | :--- |
| Successful | | None | Normal |
| Successful | | None | Normal |
| Successful | | None | Normal |
| Successful | | None | Normal |

Figure 15.39 – A successful transaction item in the queue

Congratulations! You have successfully built the automation for the performer model of the invoice processing process. Now you can publish the code to Orchestrator to run the bot from the Orchestrator page or using the UiPath assistant.

## Summary

In this chapter, you developed an automation process that fetches transaction items from a queue named **HenryFoodMartInvoice**. Later, you retrieved all of the specific data from the transaction item and stored it in a local variable. Once all the data was retrieved from the transaction item, you entered the customer details and invoice particulars on the Order Entry CRM web page.

Finally, you completed the dispatcher-performer model of the invoice processing process for Henry's Food Mart. In _Chapter 14, Invoice Processing – Dispatcher_, you downloaded the invoices from the mail and extracted the customer details and invoice particular details from the invoices, and uploaded them to the queue. In this chapter, you retrieved the customer details and invoice particular details from the queue and punched them into the Order Entry CRM.

In the next chapter, you will learn more about the **UiPath RPA Associate Exam (UiRPA)**.

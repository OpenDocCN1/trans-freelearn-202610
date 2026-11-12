# Queues / Transactions: HenryFoodMartInvoice

| Status | Reference | Revision | Priority |
| :--- | :--- | :--- | :--- |
| Successful | | None | Normal |
| Successful | | None | Normal |
| Successful | | None | Normal |
| Successful | | None | Normal |

Figure 15.39 – A successful transaction item in the queue

Congratulations! You have successfully built the automation for the performer model of the invoice processing process. Now you can publish the code to Orchestrator to run the bot from the Orchestrator page or using the UiPath assistant.

# Summary

In this chapter, you developed an automation process that fetches transaction items from a queue named **HenryFoodMartInvoice**. Later, you retrieved all of the specific data from the transaction item and stored it in a local variable. Once all the data was retrieved from the transaction item, you entered the customer details and invoice particulars on the Order Entry CRM web page.

Finally, you completed the dispatcher-performer model of the invoice processing process for Henry's Food Mart. In _Chapter 14, Invoice Processing – Dispatcher_, you downloaded the invoices from the mail and extracted the customer details and invoice particular details from the invoices, and uploaded them to the queue. In this chapter, you retrieved the customer details and invoice particular details from the queue and punched them into the Order Entry CRM.

In the next chapter, you will learn more about the **UiPath RPA Associate Exam (UiRPA)**.
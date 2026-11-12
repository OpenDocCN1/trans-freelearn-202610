7. Now, let's run the entire process by clicking on the **Run** button from the **DESIGN** ribbon bar. The process completes without any errors, and all the invoice details are uploaded to the Orchestrator queue:

![](bbox=[0.135, 0.175, 0.865, 0.395])

Figure 14.51 – The transaction item from the queue

Congratulations! You have successfully built the automation for the dispatcher model of invoice processing. Now, you can publish the code to Orchestrator to run the bot from the Orchestrator page or the UiPath Assistant.

# Summary

In this chapter, you developed an automation process that scrapes relevant data from invoices for further processing. First, the robot read through emails and downloaded the invoices, received in the form of an email attachment as a PDF. Then, it extracted specific data from the invoices and uploaded them to the Orchestrator queue.

In the next chapter, you will learn how to build the performer model to consume values from the Orchestrator queues and punch them into the order entry CRM.
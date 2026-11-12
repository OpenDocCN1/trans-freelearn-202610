# Closing and opening the CRM

Since the status of the transaction item has been set to fail, before fetching the next transaction from the queue, you will need to close the Order Entry CRM web page and open the CRM again:

+   1. In the Read_From_Queue workflow, drag and drop a Sequence activity connecting to the Set Transaction Status (Failed) activity. Then, rename it to Close and Open the CRM.

+   2. Inside the Close and Open the CRM sequence, drag and drop the Element Exists activity and indicate Henry's Food Mart Order Entry on the Order Entry CRM web page. In the Properties panel, create a new variable under Exists, named isCrmExists, using Ctrl + K.

The following screenshot shows Henry's Food Mart Order Entry being indicated by the Element Exists activity:

![](bbox=[0.254, 0.401, 0.744, 0.439])

Figure 15.36 – The Element Exists activity on the CRM page

Before closing the CRM, you can check whether the CRM is open or has already been closed. The Element Exists activity will return a Boolean as the output:

+   • TRUE means the CRM is still open.
+   • FALSE means the CRM is not available.

TRUE means the robot will need to close the CRM and open the CRM again, and FALSE means the robot will only need to open the CRM again because it is already closed.

+   3. To achieve the previously mentioned logic, you will use the If activity below the Element Exists activity. Pass isCrmExists into the Condition field.

+   4. In the Then block of the If activity, drag and drop the Attach Browser activity, attach the already opened Order Entry CRM web page, and place the Close Tab activity inside the scope of the Attach Browser activity.

+   5. Below the Attach Browser activity, drag and drop the Open Browser activity and type Environment.CurrentDirectory+"\\CRM\\Order Entry.html" into the Url field. Then, place the Maximize Window activity inside the scope of the Open Browser activity.
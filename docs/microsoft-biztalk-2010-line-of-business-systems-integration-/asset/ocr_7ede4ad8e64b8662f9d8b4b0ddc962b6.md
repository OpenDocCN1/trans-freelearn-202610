2. BizTalk will receive this file and then create the sales order in SAP, commit the transaction, and then send a response to our quoting system called **SalesOrderResponse{GUID}.xml**. This document will contain our **SalesOrderNumber** that was just created in SAP.

![](bbox=[0.28, 0.22, 0.72, 0.32])

3. To execute a rollback scenario, we can copy and paste the CreateSalesOrderRequest_Rollback.xml as found in the C:\LOBintegration\Chapter05\FileDrop folder. The difference between this file and the CreateSalesOrderRequest_Commit.xml is the **isCommitted** flag is set to **false**.

![](bbox=[0.3, 0.43, 0.72, 0.69])

4. When this file is processed through BizTalk a sales order will be created and a Sales Order Number will be returned. The difference is that this transaction is rolled back instead of committed. Also, in our response message to our Quoting system, our **isCommitted** flag is set to **false**.
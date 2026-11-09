21. If you are unsure of the values for your organization, please contact your BASIS administrator.

![](bbox=[0.34, 0.19, 0.66, 0.52])

22. Click the OK button to close this dialog.

23. We now need to update the SOAP Action header with the names of the operations that were created in our Orchestration. Since this Send port is responsible for Sending the Create Sales Order, Commit, and Rollback messages, we will need three entries in this text box:

```
<BtsActionMapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Operation Name="Operation_SendCreateSalesOrderCommit" Action="http://Microsoft.LobServices.Sap/2007/03/Bapi/BUS2032/BAPI_TRANSACTION_COMMIT/BAPI_TRANSACTION_COMMIT" />
<Operation Name="Operation_SendCreateSalesOrderRollback" Action="http://Microsoft.LobServices.Sap/2007/03/Bapi/BUS2032/BAPI_TRANSACTION_ROLLBACK/BAPI_TRANSACTION_ROLLBACK" />
<Operation Name="Operation_SendSalesOrderRequestsToSAP" Action="http://Microsoft.LobServices.Sap/2007/03/Bapi/BUS2032/CREATEFROMDAT2/BAPI_SALESORDER_CREATEFROMDAT2" />
</BtsActionMapping>
```
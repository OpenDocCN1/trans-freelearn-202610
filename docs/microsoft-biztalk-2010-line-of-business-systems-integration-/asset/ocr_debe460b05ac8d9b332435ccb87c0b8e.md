13. If `msgCreateSalesOrderRequest.isCommitted` is set to false, we want to rollback the transaction instead of committing the transaction.

+   a. In order to rollback the transaction, we need to create an instance of a **BAPI_TRANSACTION_ROLLBACK** message by transforming our CreateSalesOrderRequest message.

+   b. Once this message has been transformed, we need to update its message context properties in a Message Assignment shape. Within this shape, we want to set the **ConnectionState** property to "ABORT". By setting this property to **ABORT**, we are instructing the adapter to abort the transaction and close the channel.

![](bbox=[0.27, 0.33, 0.73, 0.55])

+   c. We now want to send our **BAPI_TRANSACTION_ROLLBACK** message to SAP.
16. Inside the Chapter05-RFCBAPI application, we need to add a Receive Port that will receive messages from our Quoting system called ReceiveCreateSalesOrderRequests. We now need a Receive Location for this Receive Port called ReceiveCreateSalesOrderRequestsFromQuotingSystem that will use the FILE Adapter, the BizTalkServerApplication Receive handler, and the XMLReceive Receive pipeline. Since we are sharing a hot folder with the previous example, we need to provide a file mask of CreateSales*Copy*.xml.

![](bbox=[0.28, 0.29, 0.72, 0.55])

17. We now need to create our Send ports. The first Send port that we will create will be used to communicate with SAP. The Send port that we will create is called SendCreateSalesOrdersToSAP and is responsible for sending the BAPI_SALESORDER_CREATEFROMDAT2, BAPI_TRANSACTION_COMMIT, and BAPI_TRANSACTION_ROLLBACK messages.

18. This Send port will use the WCF-SAP adapter, the BizTalkServerApplication Send handler, the XMLTransmit Send pipeline, and the XMLReceive Receive pipeline.
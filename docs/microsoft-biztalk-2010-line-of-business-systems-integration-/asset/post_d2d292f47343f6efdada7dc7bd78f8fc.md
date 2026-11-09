
| Field Name | Purpose |
|---|---|
| /Header/RequestedDate | The date that the products, or services, are required by. |
| /Header/PurchaseNo | Text that describes the Purchase Number. |
| /Header/Currency | The currency that the transaction will take place in. |
| /Material/MaterialID | The ID of the Material item that we would like to include in our purchase order. |
| /Material/TargetQU/ | Target unit of measure. |
| /Partner/PartnerRole | A business partner role is used to classify a business partner in business terms. The roles you assign to a business partner reflect the functions it has and the business transactions in which it is likely to be involved. |
| /Partner/Number | A number used to identify a customer. |
| isCommitted | This field is for demonstration purposes. It will trigger whether or not we want to commit the order or roll the order back. In order to easily determine whether or not this value is true or false, this field has been marked as a distinguished field. |

![](img/23d1402649b6528b085cd6b900a2c9c3_253_0.png)

3. Our next step is to create a map that will transform our CreateSalesOrderRequest from our quoting system to our SALESORDER_CREATEFROMDAT2 SAP BAPI call. The name of this map is called CreateSalesOrderRequest_to_SALESORDER_CREATEFROMDAT2.btm. The following image contains the detailed mapping instructions. Note that the Material node is a recurring node so a looping functoid has been added to ensure the correct amount of BAPISDITM nodes are created in the destination message.

4. We also want to ensure that the **BAPIRET2** node is instantiated so that SAP can return any errors. **BAPIRET2** is a common table that is used for returning the status of the BAPI that was just executed. Even if you are calling a custom BAPI, it is a good practice to return this node. In order to provide this node in our request message, we will simply drag a link from root node to the **BAPIRET2** node.

![](img/23d1402649b6528b085cd6b900a2c9c3_254_0.png)

5. Once we have sent our SALESORDER_CREATEFROMDAT2 request to SAP and received a response, we are going to need to provide our Quoting system with a response message. A schema called **CreateSalesOrderResponse.xsd** has been created to handle this responsibility. Inside this schema, we will find three elements. **CustomerNumber** and **SalesOrderNumber** are strings while **isCommitted** is a Boolean.

| Field Name | Purpose |
|---|---|
| CustomerNumber | Since we will be using asynchronous messaging between the Quoting system and BizTalk, we want to provide the customer number that the Sales Order belongs to. |
| SalesOrderNumber | This is the identifier that SAP will provide when a Sales Order has been successfully created. |
| isCommitted | This field is also for demonstration purposes and will provide an indicator to the quoting systems as to whether the transaction was committed or rolled back. We will also mark this field as a distinguished field so that we can conveniently update this flag within our ProcessCreateSalesOrder.odx orchestration. |

![](img/23d1402649b6528b085cd6b900a2c9c3_255_0.png)

6. The map responsible for transforming the SAP Response into the Quoting System response is called **SALESORDER_CREATEFROMDAT2Response_to_CreateSalesOrderResponse.btm**.

7. Since the **isCommitted** field is not being returned from SAP, we will set an initial value of true from a String Concatenate functoid and subsequently update this flag in a Message Assignment shape within our `ProcessCreateSalesOrder.odx` orchestration.

![](img/23d1402649b6528b085cd6b900a2c9c3_255_1.png)

8. In the Schema Generation section of this chapter, we discussed when Creating, Updating, and Deleting data via a BAPI that we need to either Commit or Rollback the transaction. In order to satisfy the requirement of committing the transaction, we need to create a map that will instantiate an instance of a BAPI_TRANSACTION_COMMIT message. The source message for this map can really be anything as we are not using any of the message body payload data in the Commit message. With this in mind, the CreateSalesOrderRequest message should work. A map called CreateSalesOrder_to_BAPI_TRANSACTION_COMMIT.btm has been included in the source code for this chapter that will satisfy our needs.

![](img/23d1402649b6528b085cd6b900a2c9c3_256_0.png)

9. Similarly, we need to create a map that deals with Rollback scenarios. A map called CreateSalesOrderRequest_to_BAPI_TRANSACTION_ROLLBACK.btm has been created for this purpose and it follows the same logic as the Commit map.

![](img/23d1402649b6528b085cd6b900a2c9c3_256_1.png)

10. We are now at the point where we can assemble all of these artefacts within an orchestration called ProcessCreateSalesOrder.odx. Since this orchestration is larger than the other orchestrations in the SAP chapters, we will break it down into several parts.

![](img/23d1402649b6528b085cd6b900a2c9c3_257_0.png)

11. Our initial steps will involve the following actions:
- a. Receiving an instance of our CreateSalesOrderRequest message.
- b. Transforming our CreateSalesOrderRequest message into our SALESORDER_CREATEFROMDAT2 SAP Request message.
- c. Updating the message context to set the ConnectionState to "OPEN". By setting it to OPEN, we are instructing the adapter to open a new channel for the transaction. In order to set this message context, we need to add an assembly reference for Microsoft.Adapters.SAP.BiztalkPropertySchema.dll. You can find this dll in the following locations:
- i. 32 bit: C:\Program Files (x86)\Microsoft BizTalk Adapter Pack\bin
- ii 64 bit: C:\Program Files\Microsoft BizTalk Adapter Pack (x64)\bin\
![](img/23d1402649b6528b085cd6b900a2c9c3_258_0.png)
- e. Send the SALESORDER_CREATEFROMDAT2Request to SAP.
- f. Receive the SALESORDER_CREATEFROMDAT2Response from SAP.

![](img/23d1402649b6528b085cd6b900a2c9c3_259_0.png)

12. We now want to perform a check to determine whether or not we should be committing the transaction in SAP. To do this, we will add a Decide shape to the orchestration and then check to see if our Request message has its isCommitted flag set to true:

![](img/23d1402649b6528b085cd6b900a2c9c3_259_1.png)
- a. If msgCreateSalesOrderRequest.isCommitted is set to true, then we will proceed with calling a transformation that will convert our CreateSalesOrderRequest message into an instance of our BAPI_TRANSACTION_COMMIT message.
- b. Once this message has been transformed, we need to update its message context properties in a Message Assignment shape. Within this shape, we want to set the **ConnectionState** property to "**CLOSE**". By setting this property to **CLOSE**, we are instructing the adapter to commit the transaction and close the channel.
![](img/23d1402649b6528b085cd6b900a2c9c3_260_0.png)
- c. We now want to send our **BAPI_TRANSACTION_COMMIT** message to SAP.
- d. Once the transaction has been committed, SAP will send us a response message acknowledging that the transaction has been completed.
![](img/23d1402649b6528b085cd6b900a2c9c3_260_1.png)

13. If `msgCreateSalesOrderRequest.isCommitted` is set to false, we want to rollback the transaction instead of committing the transaction.
- a. In order to rollback the transaction, we need to create an instance of a **BAPI_TRANSACTION_ROLLBACK** message by transforming our CreateSalesOrderRequest message.
- b. Once this message has been transformed, we need to update its message context properties in a Message Assignment shape. Within this shape, we want to set the **ConnectionState** property to "ABORT". By setting this property to **ABORT**, we are instructing the adapter to abort the transaction and close the channel.
![](img/23d1402649b6528b085cd6b900a2c9c3_261_0.png)
- c. We now want to send our **BAPI_TRANSACTION_ROLLBACK** message to SAP.
- d. Once the transaction has been aborted, SAP will send us a response message acknowledging that the transaction has been canceled.
![](img/23d1402649b6528b085cd6b900a2c9c3_262_0.png)

14. The remaining steps include transforming our SALESORDER_CREATEFROMDAT2Response into our CreateSalesOrderResponse that we will send to our Quoting system.
- a. For demonstration purposes, we added a flag on the CreateSalesOrderResponse message to indicate whether or not Commit flag was set. In a Message Assignment shape, we will simply assign the value that was populated in the CreateSalesOrderRequest message.
![](img/23d1402649b6528b085cd6b900a2c9c3_262_1.png)

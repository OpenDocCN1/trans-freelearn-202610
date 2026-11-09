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

![](bbox=[0.42, 0.44, 0.58, 0.64])

3. Our next step is to create a map that will transform our CreateSalesOrderRequest from our quoting system to our SALESORDER_CREATEFROMDAT2 SAP BAPI call. The name of this map is called CreateSalesOrderRequest_to_SALESORDER_CREATEFROMDAT2.btm. The following image contains the detailed mapping instructions. Note that the Material node is a recurring node so a looping functoid has been added to ensure the correct amount of BAPISDITM nodes are created in the destination message.
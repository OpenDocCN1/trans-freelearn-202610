The **SalesOrderStatus** table tracks the status of the Order from a systems integration perspective. Since we do not want BizTalk to process orders multiple times, we need to manage the records that have been consumed. This table provides us the ability to mark records as being processed by specifying a GUID in the **ProcessingIndicator** field. When it is set to NULL it identifies that the rows need to be processed by BizTalk Server.

The **OrderStatus** column is used to identify different stages of the sales order processing, and SalesOrderID is the link to the tables actually holding sales order information:

![](bbox=[0.315, 0.295, 0.685, 0.425])

The **SalesOrderDetail** table provides us with some supplementary information about the order including the **UnitPrice**, **Discount**, **CarrierTracking Number**, and so on:

![](bbox=[0.295, 0.485, 0.705, 0.715])
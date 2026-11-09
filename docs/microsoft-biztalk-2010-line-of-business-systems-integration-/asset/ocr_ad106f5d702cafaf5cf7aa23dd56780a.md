Once we clicked **OK** the Map designer showed up containing our three schemas packed in one destination schema.

The following image shows the fully configured map used in our sample application:

![](bbox=[0.21, 0.22, 0.79, 0.57])

To set the **Query** property of the **ExecuteReader** request message, we use a string concatenate functoid to build a select statement made up of multiple table joins to return address information for a customer. We then link the CustomerID element from the source message to finalize the statement with the WHERE clause:
![](bbox=[0.235, 0.147, 0.765, 0.5])

For the select statement we want to run against the vSalesOrderDetailsProduct view, we need to specify the required columns and a WHERE clause. A string concatenate functoid is used to set the Columns element to *. This will tell the adapter that all columns from the view should be selected. Another string concatenate functoid is used to populate the Query element. We use the SalesOrderID element from the input schema to build the WHERE clause where SalesOrderID =SalesOrderID.

Lastly, we need to specify the input parameter for the ufnGetContactInformation table valued function. This function has only one parameter: ContactID. So we drag the ContactID from the source schema to the ContactID element of the destination schema.

A nice feature of BizTalk is the ability to reuse logical ports within an orchestration. This allows us to bind our logical port to a single physical port in the BizTalk Administration Console, for all three lookups. This reduces the amount of send ports that we have to create and maintain. We can also use a parallel shape in the orchestration to execute all three operations at the same time.
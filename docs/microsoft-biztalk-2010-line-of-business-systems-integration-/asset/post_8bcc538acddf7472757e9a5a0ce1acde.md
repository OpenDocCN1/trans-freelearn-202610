
![](img/23d1402649b6528b085cd6b900a2c9c3_413_0.png)

2. The schema for the ERP message that we will be using is called SharePointListVehicleMaintenance_FF.xsd and it is a Flat File Schema. A sample of the message that we want to generate exists in the SampleDocs folder that may be found in this chapter's sample code. The name of the file is called Vehicle_Maintenance_For_ERP.csv.

![](img/23d1402649b6528b085cd6b900a2c9c3_413_1.png)

3. You will notice the structure of the flat file schema resembles the Custom SharePoint List that we previously created.

![](img/23d1402649b6528b085cd6b900a2c9c3_414_0.png)

4. Since this is a flat file schema, we will also need a flat file pipeline to assemble this message as it is moving through the Send Port. The Send pipeline that will take care of this for us is called SendVehicleMaintenance_FF.btp. Inside the Assemble stage, we want to drag a Flat File assembler component onto the pipeline's surface. We then want to set the Document schema to our flat file schema: Chapter8ManipulatingLists.SharePointListVehicleMaintenance_FF.

![](img/23d1402649b6528b085cd6b900a2c9c3_414_1.png)

5. We now want to drop a **Send** shape onto the orchestration and create a Logical Send Port. In the **Send** shape, we want to specify a message called msgVehicleMaintenance_FF, which has a type of **Chapter8ManipulatingLists.SharePointListVehicleMaintenance_FF** and a logical **Send Port** that is one-way and that has a port binding that is set to **Specify Later.**

![](img/23d1402649b6528b085cd6b900a2c9c3_415_0.png)

## Deploying and configuring the BizTalk solution

We are now ready to deploy and configure our BizTalk solution.

- 1. To deploy our solution, right mouse click on it and then click on **Deploy Solution.**
- 2. Inside the BizTalk Admin console, we should find an application called **Chapter8-ManipulatingLists**. A binding file called `Chapter8-ManipulatingLists.xml` exists for this solution in the `Bindings` folder within this Chapter's source code. If we import this file, we will discover that we have one Receive Port, one Receive location, and two Send Ports. Do note that the endpoints that are included in this binding file may not match the targets on your machine.

3. Our Receive Port, called **RetrieveSharePointGUIDs**, has one Receive Location called **RetrieveSharePointGUIDsFromSQL**. The Receive Location will use the WCF-Custom Adapter, the **BizTalkServerApplication** Host Instance, and the **XMLReceive** pipeline.

![](img/23d1402649b6528b085cd6b900a2c9c3_416_0.png)

> Note: We can also use the WCF-SQL Adapter in this solution as well. Since the Consume Adapter Service Wizard specifies WCF-Custom Adapter in the binding file that it automatically generates we will use it. Essentially, both adapters are leveraging the sqlBinding so the underlying functionality is the same.

4. Within this Receive Location, we need to provide an **Address (URI)**. In this case, we can provide the following: `mssql://localhost//SharePointList?InboundId=1`. **Localhost** represents the Database Server hosting the **SharePointList** database. We also have to provide a unique **InboundId** for this connection in the event we have multiple polling Receive Locations configured to use this same database. For more information regarding the **InboundId** property, please refer to Chapter 2.

![](img/23d1402649b6528b085cd6b900a2c9c3_417_0.png)

5. In the **Binding** tab, there are a few properties that we need to modify which are described below:

| Property Name | Property Value | Description |
|---|---|---|
| Binding Type | sqlBinding | Since we are using the WCF-Custom Adapter, we need to provide a binding and since we are connecting to a SQL Server, we need to provide **sqlBinding**. |
| inboundOperation Type | TypedPolling | Our scenario uses TypedPolling as we are expecting a strongly typed message returned from our **pollingStatement** query. |
| polledData Available | Select count(*) from Lists | This property needs a query that when executed will determine whether or not to proceed with executing the **pollingStatement**. If this query does not return any data then the **pollingStatement** query will not be executed. |
| pollingIntervalIn Seconds | 30 | The interval in which we want to contact our SQL Server to retrieve data. This value is probably not valid in a production scenario, but for our demonstration purposes it will work. In a production scenario, we may not want to poll this frequently since each poll puts some stress on the database. |
| pollingStatement | Select ListGUID from Lists | This query will return the GUID that we will use when contacting SharePoint in order to retrieve our Custom List data. |

![](img/23d1402649b6528b085cd6b900a2c9c3_418_0.png)

- 6. The remaining properties that have been set are default values for the **WCF-Custom** Adapter that uses the **sqlBinding**.
- 7. Our send port that we will use to communicate with SharePoint is called **cfSendPort_Lists_ListsSoap_Custom**. This send port will also use the **WCF-Custom** Adapter, the **BizTalkServerApplication** Host Instance, the **PassThruTransmit** Send Pipeline, and the **XMLReceive** Receive Pipeline.

![](img/23d1402649b6528b085cd6b900a2c9c3_419_0.png)

8. We now need to specify the URL of our Lists.asmx Web Service and our Soap Action header. The URL that is provided below represents the URL that was used to build this example. You will need to update this URL to reflect your SharePoint server. When we used the Consume WCF Service Wizard, it also generated a Binding file that we can use to populate our send port. One of the key benefits of this approach is the generated Binding file will include all of the Soap Action Headers including GetListItemChanges as follows:

```
<Operation Name="GetListItems" Action="http://schemas.microsoft.com/sharepoint/soap/GetListItems" />
```

![](img/23d1402649b6528b085cd6b900a2c9c3_420_0.png)

9. On the Binding tab, there are a few significant properties that we need to modify, which are described as follows:

| Property Name | Property Value | Description |
|---|---|---|
| Binding Type | basicHttpBinding | This binding is our best option when consuming classic ASP.Net ASMX Web Services. |
| Security | mode=Transport CredentialOnly | Since the data that we are consuming is not sensitive we do not require any special message-based security for this scenario. We do need to provide credentials for SharePoint so that we can be authenticated and authorized in order to manipulate SharePoint Lists. In this scenario, our BizTalk Host Instance account will be passed. Our BizTalk Host instance account also requires Contribute access to the site that we are connecting to. |
| Security-Transport | clientCredential Type=Ntlm | Since our BizTalk Server and SharePoint Server exist in the same domain, we can utilize Ntlm credential types. |

![](img/23d1402649b6528b085cd6b900a2c9c3_421_0.png)

10. In the Behavior tab, we need to ensure that we have an EndpointBehaviour that has a type of clientCredentials and enable Windows Impersonation.

![](img/23d1402649b6528b085cd6b900a2c9c3_421_1.png)

11. The remaining settings for this send port leverage the WCF-Custom Adapter's default properties for the basicHttpBinding.

12. Our final send port is responsible for sending our flat file to our ERP System. The name of this send port is SendVehicleMaintenanceListToERP. It will leverage the FILE Adapter, the BizTalkServerApplication Host Instance, and our custom SendVehicleMaintenance_FF pipeline.

![](img/23d1402649b6528b085cd6b900a2c9c3_422_0.png)

13. We now need to configure our Orchestration so we can set our Host Instance and link our Logical Ports with our Physical Ports.

![](img/23d1402649b6528b085cd6b900a2c9c3_422_1.png)

## Testing BizTalk application

Before enabling our BizTalk application, let's go back to our List to ensure we have some data that we can retrieve via the SharePoint Lists.asmx Web Service.

- 1. Once we are at the list, we can add new records by clicking on the Add new item link.

![](img/23d1402649b6528b085cd6b900a2c9c3_423_0.png)

- 2. SharePoint provides a nice interface for us to add data to our Custom List.

![](img/23d1402649b6528b085cd6b900a2c9c3_423_1.png)

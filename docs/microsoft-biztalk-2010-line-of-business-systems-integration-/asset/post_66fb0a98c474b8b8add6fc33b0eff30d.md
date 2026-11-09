
![](img/23d1402649b6528b085cd6b900a2c9c3_453_0.png)

This completes the steps for sending a USD - CAD exchange rate to our USD-based Dynamics AX Company. To complete our example, we'll now add in the logic for the USD - CAD exchange rate to our CAD-based Dynamics AX Company. So, we'll repeat the above from the Orchestration Setup step using the msgExchangeRatesCADUSD and the daxCADCompany in place of msgExchangeRatesUSDCAD and daxUSDCompany. See the complete code example.

In the example source code provided, there's no error handling built into the orchestration. If there's an error in sending a message to Dynamics AX, the port will throw an exception. Quite often, especially in versions of Dynamics AX prior to AX 2009, the error message can be quite generic or cryptic at times. Connectivity errors will have some meaningful error message; however, logical errors will not. This is because the AIF module is an external facing interface, so the detailed error message most helpful is found in the Exceptions (Basic | Periodic | Application Integration Framework | Exceptions) form in the AIF module.

Now that we can build and deploy the solution, the next step will be to set up our Port configuration in the BizTalk Server Administration Console.

## Port configuration

We'll begin by creating a Static Solicit-Response Send Port and type in a Name of AXExchangeRates_SendReceivePort.

- 1. We'll set the Type to Microsoft Dynamics AX 2009, and set the Send pipeline and Receive pipeline to XML Transmit and XML Receive respectively.

![](img/23d1402649b6528b085cd6b900a2c9c3_454_0.png)

- 2. Click Configure to bring up the Microsoft Dynamics AX 2009 Transport Properties window.
- 3. Here, we'll select an Authentication Type to Host User from the drop-down list and set the Gateway User to our Active Directory service account that our BizTalk Host instance is running under. We'll set the AOS Port to 2712 and AOS Server to our Dynamics AX 2009 server name AX 2009-SHARED. We can leave the default Synchronous Timeout to 20 minutes.

![](img/23d1402649b6528b085cd6b900a2c9c3_455_0.png)

- 4. Click Ok to save this configuration.
- 5. Next, we'll continue with the port configuration by creating a One-way Receive Port, named ReceiveClosingFxRates_ReceivePort, with a receive location named ReceiveClosingFxRates_ReceiveLocation. We'll use a Type of Schedule and set the Receive pipeline to XML Receive.

![](img/23d1402649b6528b085cd6b900a2c9c3_455_1.png)

- 6. Next, click **Configure** to bring up the Schedule Transport Properties window. Here, we'll type in a **Name** of **Bank of Canada**, and set the configuration **Schedule** to **Daily Schedule** with a **Start Time** of **5:00 pm** (assume our server time is EST). Note we'll configure it to run daily, regardless of weekends or bank holidays (when no new rates are available), as our orchestration does the query to AX before attempting to create a new exchange rate.

![](img/23d1402649b6528b085cd6b900a2c9c3_456_0.png)

![](img/23d1402649b6528b085cd6b900a2c9c3_456_1.png)

- 7. Now, click on the **Task properties** field from the **Schedule Transport Properties** window to bring up the **Task Properties** window.

![](img/23d1402649b6528b085cd6b900a2c9c3_457_0.png)

- 8. Here, click Find Task, which brings up the Assembly Qualified Type Name window. Click on the Browse button, select the ScheduledTask.TaskComponents.dll assembly and click open. Now, double click HttpDownload and click Select. Click Ok. Finally, in the Task Properties window, type in the URL http://www.bankofcanada.ca/rss/fx/close/fx-close.xml, leave the user name blank as no authentication is required. Click OK twice and save.

![](img/23d1402649b6528b085cd6b900a2c9c3_457_1.png)

- 9. On the Orchestration Properties, we'll assign the logical receive port to the physical port we just created and set both Outbound Logical Ports to the physical send port to connect to Dynamics AX 2009.

![](img/23d1402649b6528b085cd6b900a2c9c3_458_0.png)

- 10. After we start our application, it's now time to test it. We can either wait until 5:00 pm, or simply change the Start Time in the Schedule Properties window to a couple minutes past the current time. You'll see a log message in the Windows event log indicating the configuration has changed for the schedule task.
- 11. After the scheduled task adapter has fired, and the orchestration has successfully completed, navigate to the Document history form (Basic | Periodic | Application Integration Framework | Document History) in Dynamics AX 2009. Here, you'll see a log for all four messages that were sent and received from our BizTalk application. You can also view the XML message by highlighting one of the Document history log entries, clicking Document logs to bring up the Document log form, and finally clicking View XML.

![](img/23d1402649b6528b085cd6b900a2c9c3_459_0.png)

- 12. Verify that the exchange rate has been created in the table (General Ledger | Setup | Exchange Rates).
- 13. Note that the Document history form is company specific and will log all transactions both successful and those that went to error. However, the Exceptions form log is not company specific. Thus, attempting to link an exception to the document history entry can prove to be difficult.

## Asynchronous walkthrough example—Dynamics AX message outflow

Now that the setup for sending data using AIF XML documents to AX is complete, we can also use the BizTalk adapter to retrieve data from Dynamics AX via this AIF module. This is done using the AIF Queue manager. Throughout Dynamics AX, there are Send electronically buttons that can allow you to push data into the AIF Queue with an Outbound direction. Similarly, asynchronous incoming messages have an Inbound direction parameter.

We found that rather than repeating code in several BizTalk solutions when you need to retrieve data from the AIF Queue, it's relatively simple to create a general solution to accomplish this. This solution will retrieve all data via the BizTalk Dynamics AX adapter by polling the Queue at a set interval of time. The minimum polling interval is 1 minute, thus any messages you put in the AIF Queue will not be immediately consumed by BizTalk. The complete solution (Chapter9-AXMessageOutflow) is included with the source code for this chapter.

We'll start by creating a new BizTalk project, Chapter9-AXMessageOutflow, in Visual Studio. Add in a new orchestration, ProcessOutboundAXMessage.odx, which will be the only orchestration required for this example. Also, we'll need to add reference to the Microsoft.Dynamics.BizTalk.Adapter.Schemas assembly and sign the project with a strong name key.

### Message setup

Next, we'll add two messages to our orchestration: **msgAxOutboundMessage** and **msgAXDocument**. These will be the only two messages required in this example.

![](img/23d1402649b6528b085cd6b900a2c9c3_460_0.png)

The first message, **msgAXOutboundMessage**, is of type **DynamicsAX5.Message.Envelope**. The schema is located in the referenced Microsoft.Dynamics.BizTalk.Adapter.Schemas assembly.

![](img/23d1402649b6528b085cd6b900a2c9c3_460_1.png)

All outbound messages from the AIF Queue are of this type. As you can see from the sample screenshot below, we have some metadata in the header node but what we are really interested in is the XML contents of the Body node. The contents of the **MessageParts** node in the Body node will be of type **ExchangeRatesService_ExchangeRates.xsd** that we used in the previous example to send this same message to AX. Thus, all the schemas we require for both inbound and outbound transactions can be generated using the adapter.

![](img/23d1402649b6528b085cd6b900a2c9c3_461_0.png)

For the second message, since we don't want to specify a document type, we will use System.Xml.XmlDocument for the Message Type.

![](img/23d1402649b6528b085cd6b900a2c9c3_461_1.png)

Using the System.Xml.XmlDocument message type allows for great flexibility in this solution. We can push any message to the AIF queue, and no changes to this BizTalk application are required. Only changes to consuming applications may need to add the AX schema of the message in order to process it.

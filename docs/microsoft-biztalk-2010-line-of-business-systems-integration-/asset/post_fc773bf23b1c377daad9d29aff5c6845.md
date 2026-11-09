
The first step to generating the artifacts is establishing a connection to the target system. Most LOB systems require valid client credentials as part of the connection procedure and our Echo system is not an exception, though it is quite forgiving – after selecting **Username** for the **Client credential type** drop down, you can type in anything you like for the **User name** and **Password**, but please don't leave them blank.

![](img/23d1402649b6528b085cd6b900a2c9c3_43_0.png)

Further, besides valid credentials, in order to establish a connection we have to provide additional parameters such as host name, port number, and other LOB application specific parameters – this is what the **URI Properties** tab is for. Switch to this tab and note the properties presented there. As with the user name and password, you can type in anything you like or accept the defaults.

![](img/23d1402649b6528b085cd6b900a2c9c3_43_1.png)

While the Echo Adapter requires only three elements to build the connection URI, the real-world adapters from the BizTalk Adapters Pack 2010 require more details in order to connect to their LOB systems. For comparison, the following image shows the properties required to establish a connection to SAP:

![](img/23d1402649b6528b085cd6b900a2c9c3_44_0.png)

- Finally, we need to configure the bindings exposed by the adapter by setting the properties presented on the Binding Properties tab. For our scenario, we can accept the default values for all properties.

![](img/23d1402649b6528b085cd6b900a2c9c3_44_1.png)

Now, click **OK** to close the Configure Adapter dialog and return to the **Consume Adapter Service** window. Note that the **Configure a URI** text box now contains a configured connection string, which is built from the URI properties tab values.

![](img/23d1402649b6528b085cd6b900a2c9c3_45_0.png)

Provided the connection string we've built is correct, clicking the **Connect** button establishes an active connection to the Echo system. If the connection is accepted, we will see the category tree populated with outbound categories – the outbound contract type is what the Consume Adapter Service plugin chooses by default.

![](img/23d1402649b6528b085cd6b900a2c9c3_45_1.png)

Next, we need to select the operations that we want to use in our application and add them to the list of desired operations. In the Select a category tree click the Echo Category node, then in the Available categories and operations panel, select EchoString and click the Add button. This action adds EchoString to the list of desired operations at the bottom of the screen. The last step is to help the Consume Adapter Service add-in to generate descriptive file names by setting the file name prefix at the bottom of the screen to EchoOutbound.

![](img/23d1402649b6528b085cd6b900a2c9c3_46_0.png)

Finally, click OK to close the Consume Adapter Service Wizard window and return to Visual Studio, where we see two new files added to our BizTalk project. One file is the XSD schema for the EchoString operation, the other is an XML binding file. After deploying the application, you will import this file using the BizTalk Admin Console to create a physical WCF-Custom send port configured to perform the EchoString operation.

Unfortunately, one of the inconveniences we have to deal with is that the **Consume Adapter Service add-in** doesn't allow generating artifacts for different contract types in one run. To generate the schema and binding file for the inbound operation, we need to open the **Consume Adapter Service add-in** again and repeat the procedure. On the **Security** tab, specify the username and password, for the properties on the **URI Properties** and **Binding Properties** tabs, you can accept the default values.

![](img/23d1402649b6528b085cd6b900a2c9c3_47_0.png)

- Then, once you've established a successful connection, select **Service** in the **Select contract type** drop down, click **EchoCategory** in the **Select a category** panel, and add **OnReceiveEcho** to the list of desired operations. Set the **Filename Prefix** at the bottom of the screen to **EchoInbound**.
- Click **OK** to return to Visual Studio. Our Visual Studio solution with two new added files should now look like this:

![](img/23d1402649b6528b085cd6b900a2c9c3_48_0.png)

![](img/23d1402649b6528b085cd6b900a2c9c3_49_0.png)

Finally, to complete the schemas, double click the EchoOutbound.xsd file to open the schema editor and distinguish the EchoStringResult field. We will use this field in our orchestration.

![](img/23d1402649b6528b085cd6b900a2c9c3_49_1.png)

Technically speaking, at this point, we have all bits and pieces required to build our BizTalk application. Before we proceed, let's make a few cosmetic changes.

First, rename the WcfSendPort_EchoAdapterBinding_Custom.bindinginfo.xml to SendPort.Bindinginfo.xml. Similarly, rename the WcfReceivePort_EchoAdapterBinding_Custom.bindinginfo.xml to ReceivePort.Bindinginfo.xml.

Next, we need to give more descriptive names for the receive port and receive location. Open the ReceivePort.bindinginfo.xml file, and change the ReceivePort and ReceiveLocation Name attributes as follows:

```
<DistributionListCollection />
<ReceivePortCollection>
  <ReceivePort Name="Echo_ReceiveNotification" IsTwoWay="false" Bin
    <Description>ReceivePort for EchoAdapterBinding.</Description>
    <ReceiveLocations>
      <ReceiveLocation Name="EchoWcfCustom_ReceiveNotification">
        <Description xsi:nil="true" />
```

For the SendPort.bindinginfo.xml file, locate the SendPort node and change the Name attribute to SendEchoString.

```
<SendPortCollection>
  <SendPort Name="SendEchoString" IsStatic="true" IsTwoW
    <Description>SendPort for EchoAdapterBinding.</Descr
```

4. Now that we have prepared all the artifacts, we can proceed and build our sample application.

5. Add a BizTalk orchestration to the project and name it ProcessEcho.

![](img/23d1402649b6528b085cd6b900a2c9c3_50_0.png)

6. Switch to the **OrchestrationView** and create three orchestration messages with the following values:

- Identifier: **msgReceiveNotification**, Message Type: **Walkthrough.EchoInbound**
- Identifier: **msgEchoStringRequest**, Type: **Walkthrough.EchoOutbound.EchoString**
- Identifier: **msgEchoStringResponse**, Type: **Walkthrough.EchoOutbound.EchoStringResponse**

![](img/23d1402649b6528b085cd6b900a2c9c3_51_0.png)

7. To construct the outbound message, add a map to the project and name it.

8. **Inbound_To_Outbound**.

![](img/23d1402649b6528b085cd6b900a2c9c3_52_0.png)

9. Specify **OnReceiveEcho** as the source message type, for the destination schema, specify **EchoOutbound**, and select **EchoString** as the root node. Once the source and destination schemas are set, connect the **content** and **aName** elements.

![](img/23d1402649b6528b085cd6b900a2c9c3_52_1.png)

10. Next, we need to sketch the orchestration flow. From the toolbox, drag on the orchestration surface the shapes in the order and with the properties as follows:

- Shape: **Receive**, Name: **ReceiveNotification**, Activate: True, Message: **msgReceiveNotification**

- Shape: **Construct**, Name: **ConstructEchoOutbound**, Message Constructed: **msgEchoStringRequest**. Inside the Construct drag the Transform shape and configure it with the following values: Name: **Transform**, Input Messages: **msgReceiveNotification**, Map Name: **Inbound_To_Outbound**, Output Messages: **msgEchoStringRequest**
- Shape: **Send**, Name: **SendEchoString**, Message: **msgEchoStringRequest**
- Shape: **Receive**, Name: **ReceiveEchoResponse**, Message: **msgEchoStringResponse**
- Shape: **Expression**, Name: **WriteToLog**

11. Configure the **WriteToLog** expression shape to execute the following code:

![](img/23d1402649b6528b085cd6b900a2c9c3_53_0.png)

12. To enable the orchestration for receiving and sending messages, we need to add two logical orchestration ports. The first port will be a one-way port for receiving an initial inbound message; the second will be a Request-Response port for sending an outbound message to the adapter for processing and receiving a response. Right-click on the **Port Surface** and select **New Configured Port** to start the **Port Configuration Wizard**. Here are the settings for the first one-way port.

- Name: **ReceiveInitialEcho**

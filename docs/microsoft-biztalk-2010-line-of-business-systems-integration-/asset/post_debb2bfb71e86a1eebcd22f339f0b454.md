
![](img/23d1402649b6528b085cd6b900a2c9c3_205_0.png)

4. The following table contains the values that have been provided to each node. This information is generally static, but it is a good idea to store this information in a common Helper class so that if you have to make a change you can do so in one place. For the purpose of this chapter, we will simply embed this information with String Concatenate functoids. Do note that some values like SNDPOR, SNDPRN, and RCVPRN will most likely be different on your SAP system.

| Node | Value |
|------|-------|
| /EDI_DC40/TABNAM | EDI_DC40 |
| /EDI_DC40/IDOCTYP | CONF32 |
| /EDI_DC40/CIMTYP | CONF32 |
| /EDI_DC40/MESTYP | CONF32 |
| /EDI_DC40/SNDPOR | MTSIDOC |
| /EDI_DC40/SNDPRT | LS |
| /EDI_DC40/SNDPRN | MTSIDOC |
| /EDI_DC40/RCVPRT | LS |
| /EDI_DC40/RCVPRN | MTSIDOC |
| /E2CONF5/SATZA | I20 |
| /E2CONF5/ISMNE | H |

5. The next tab that we want to create is the **Data** tab. Within this tab, we will focus on the data that is being passed from the Work Order Management system and perform any formatting required for SAP.

![](img/23d1402649b6528b085cd6b900a2c9c3_206_0.png)

Note: A new feature of the BizTalk 2010 mapper is the ability to display only nodes that are currently mapped. To enable this feature, click on the **Switch to relevance tree view for destination schema** button. When selected, the BizTalk mapper will only display nodes that are currently being mapped. This feature is very helpful when mapping SAP IDOCs that have many nodes.

![](img/23d1402649b6528b085cd6b900a2c9c3_206_1.png)

6. With the BizTalk Map complete we can focus on the BizTalk Orchestration that will be responsible for orchestrating the events between the Work Order Management System and SAP. The Orchestration will be responsible for the following functions:

- a. Receiving the Work Order Management request that includes the Vehicle Time.
- b. Transforming this request into the SAP CONF32 IDOC request.
- c. Receiving the SAP CONF32 IDOC Response.
- d. Writing the SAP CONF32 IDOC Response to disk so that we can verify whether the exchange with SAP was successful.

![](img/23d1402649b6528b085cd6b900a2c9c3_207_0.png)

7. Since we are not using a Flat File schema and will opt to use the SOAP messaging capability of the SAP adapter, we can now deploy our application by right mouse clicking on the solution and selecting Deploy Solution.

8. Once inside the BizTalk Administration Console, we will discover that an application called Chapter04-SAPIDOCs now exists.

9. Within this application, we can find a Receive Port called **ReceiveVehicleTimeSheet** and a Receive Location called **ReceiveVehicleTimeSheetFromDisk**. This Receive Location will use the **FILE** adapter, the **BizTalkServerApplication** Receive handler and the **XMLReceive** Receive Pipeline.

![](img/23d1402649b6528b085cd6b900a2c9c3_208_0.png)

10. We will have two Send Ports with the first one being the Send Port that will be used to communicate with SAP. The Consume Adapter Wizard generated a binding file that we could use to generate a Send Port automatically. The issue with this approach is that the binding file will use the WCF Custom adapter, which does not allow us to manipulate the SAP connection details easily. Since we often create our SAP IDOC schemas in non-Production environments, it is not likely that we will be able to take full advantage of the generated Binding file anyways as our targets will be different within each BizTalk environment.

11. The name of the SAP Send port is **SendCONF32IDOC**. It will use the WCF-SAP custom adapter, the **BizTalkServerApplication** Send Handler, the **XMLTransmit** Send pipeline, and the **XMLReceive** pipeline. By clicking the Configure button, we can further explore the properties that are exposed by the SAP Adapter.

![](img/23d1402649b6528b085cd6b900a2c9c3_209_0.png)

12. The General tab will expose our Endpoint Address and SOAP Action header. To further explore the Address URI we can click on the Configure button.

![](img/23d1402649b6528b085cd6b900a2c9c3_209_1.png)

13. The configure button on the General tab is one of the key differentiators between using the WCF-Custom adapter, with SAP Binding, and the WCF-SAP adapter. As you can see in the following image the WCF-SAP Adapter provides us more options when configuring our SAP configuration. The key properties that we want to populate are:

- a. Application Server Host
- b. System Number
- c. Client

14. If you are unsure of the values for your organization, please contact your BASIS administrator.

![](img/23d1402649b6528b085cd6b900a2c9c3_210_0.png)

15. The next tab that we need to be concerned with is the Credentials tab. On this tab we will provide the BizTalk credentials that will be used to communicate with SAP. We do have the ability to specify a Username/Password combination or an SSO Affiliate application. For the purposes of this chapter, we will simply provide a Username/Password.

![](img/23d1402649b6528b085cd6b900a2c9c3_211_0.png)

16. The next Send Port that we need to create is the Send Port that will write the SAP IDOC response to disk. The name of this Send Port is **SendIDOCResponseToDisk**, and it will use the **FILE** Adapter, a Send Handler of **BizTalkServerApplication**, a Send Pipeline of **PassThruTransmit**.

![](img/23d1402649b6528b085cd6b900a2c9c3_211_1.png)

17. We now need to configure our Orchestration to use the Receive Port and Send Ports that we just created.

![](img/23d1402649b6528b085cd6b900a2c9c3_212_0.png)

18. We can now start our application so that it is ready for Application Testing.

## Testing BizTalk Application

Our Application Testing involves copying and pasting our sample file inside the hot folder as configured in the Receive Location. The file contents will include:

- VehicleID
- Order Number
- Order Duration
- Order Operation
- Order Date
- Order Time

![](img/23d1402649b6528b085cd6b900a2c9c3_213_0.png)

- 1. To initialize the test, copy and paste the VehicleTimeSample.xml file as found in the C:\LOBIntegration\Chapter04\Chapter04-SAPIDOCs\FileDropfolder.
- 2. BizTalk will consume this file and begin to process the file.
- 3. Once BizTalk has communicated with SAP, SAP should return a response to BizTalk.
- 4. BizTalk will take this response and generate a file called IDOCoutput{GUID}.xml.

![](img/23d1402649b6528b085cd6b900a2c9c3_213_1.png)

## Custom IDOCs

The previous example included an "out of the box" CONF32 SAP IDOC. While the idea of being able to integrate with SAP out of the box is extremely compelling, the reality is it just isn't realistic to expect an out of the box IDOC for each integration scenario that your organization is interested in. Most organizations will attempt to use the out of the box IDOCs before looking to build a custom IDOC. When forced to build, there are a few options that SAP Developers have:

- Extend existing IDOCs.
- Copy an IDOC structure and modify as you see fit.
- Start a new IDOC from Scratch.

Extending an IDOC involves keeping an IDOC structure intact, but adding additional segments that are required in your scenario. This is an ideal solution when you are only missing a few segments and want to leverage SAP's existing efforts.

If SAP has an IDOC that is somewhat similar to your scenario, you can copy that existing structure and then add or remove segments as required.

While SAP does provide a rich platform in which to run your business on, it is impossible for them to envision every integrated scenario. This leaves us the option of custom building an IDOC that addresses our unique integration scenario.

BizTalk is able to integrate with SAP through all three of these mechanisms. Using the out of the box IDOCs is the preferred option as it requires no customization and you can ensure of support by SAP in future versions. If you write a custom IDOC and it performs functions that conflict with SAP's upgrade path, your organization will be on its own in order to resolve any issues.

When generating a custom IDOC, there is nothing special that needs to occur from a BizTalk perspective. We continue to use the Consume Adapter Service Wizard just as we would if we were trying to generate a schema for an out-of-box IDOC.

![](img/23d1402649b6528b085cd6b900a2c9c3_214_0.png)

> Note: If the SAP developer who built the custom IDOC says the IDOC is available but you do not see it in the list of available IDOCs, within the Consume Adapter Service Wizard, ask them to ensure that the IDOC has been released.

Below is an image of a custom IDOC. As you can see, it does not look all that different from an SAP out of box IDOC. The schema will have both **idocData** and **EDI_DC40** nodes. The remaining nodes, or segments, have been built by the SAP developer.

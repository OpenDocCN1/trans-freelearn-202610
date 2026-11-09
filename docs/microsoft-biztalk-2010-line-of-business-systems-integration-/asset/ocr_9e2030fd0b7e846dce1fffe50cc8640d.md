+   7. Back in our original orchestration, `ApprovalProcess.odx`, create a new configured port named **CallSFDCPort** that uses the existing, generated port named **Chapter10-SFDC.AccountProcessing.ReimbApproval.Soap**. Set it to send a request and receive a response and specify the port binding later.
+   
+   8. When the orchestration port configuration wizard is complete, we see every operation in that service exposed on the orchestration's port surface. We have two choices here. First, go back to wizard-generated orchestration and delete these unused operations from the port type. This is not overly practical if we were using this Salesforce.com reference in a shared assembly that other BizTalk projects plan on using. Our second, and best, option is to simply ignore the operations that we don't need and deal with the additional clutter on our orchestration surface.
+   
+   9. Create the input and output messages expected by the Salesforce.com service. The first message, named **SFDC_QueryRequest**, is of multi-part type **Chapter10-SFDC.AccountProcessing.ReimbApproval.queryRequest** and the second message, named **SFDC_QueryResponse**, is of multi-part type **Chapter10-SFDC.AccountProcessing.ReimbApproval.queryReponse**.
+   
+   10. Add a **Transform** shape to the existing **Group** placeholder and set the **Construct Message** shape's **Message Constructed** value to the **SFDC_QueryRequest** message. This map creates our service query message.
+   
+   11. Double-click the **Transform** shape and create a new map named **ReimbRequest_To_SFDCQuery** and choose the **ReimburseInput** message as the map's **Source Transform** and the **SFDC_QueryRequest.parameters** message as the map's **Destination Transform**.
+   
+   12. The destination schema's only field (`queryString`) holds the Salesforce Object Query Language (SOQL) statement necessary to retrieve the Account attributes we need. Drag a **Scripting** functoid to the map's grid surface. Drag the `AccountId` field from the source schema into the functoid.
+   
+   13. Double-click the functoid and switch to the **Script Functoid Configuration** tab and set the **Select script type** value to **Inline C#**.

![](bbox=[0.36, 0.63, 0.62, 0.74])
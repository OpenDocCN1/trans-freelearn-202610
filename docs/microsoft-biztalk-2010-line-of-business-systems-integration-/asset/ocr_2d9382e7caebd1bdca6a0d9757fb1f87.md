11. Once we have made this change to the web.config file, we can refresh our web browser and will be presented with the Service Operations.

![](bbox=[0.32, 0.19, 0.68, 0.35])

12. The BTSharePointAdapterWS.asmx Web Service exposes four Web Methods:

+   - FinalizeDocuments
+   - GetDocuments
+   - IsAlive
+   - PutDocuments

The Windows SharePoint Services Adapter will call these methods when interacting with SharePoint. The implementation of these Web Services is abstracted from BizTalk developers making integrating with SharePoint a great experience.

Each Web Method has a distinct purpose, so let's further investigate each method.

# FinalizeDocuments

When receiving documents from SharePoint, the default behavior of the adapter is to remove the file from the source location. This behavior models that of the FILE and FTP Adapters in order to prevent an endless loop and message duplication. If a person has loaded a message, or document, in SharePoint then there is a good chance that they do not want to lose the message entirely when BizTalk picks it up. To avoid this situation, Microsoft has included a property in the Adapter called Archive Location URL. This feature will store a copy of the message that is currently being processed and place it into a different Document or Forms library. The Archive Location URL feature leverages the FinalizeDocuments Web Method to accomplish the archival of messages that are being processed.
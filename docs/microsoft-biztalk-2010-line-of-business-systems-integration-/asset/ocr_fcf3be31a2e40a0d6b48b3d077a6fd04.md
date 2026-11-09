20. Navigate to the **Behavior** tab and add a new Endpoint behavior. Select the **transportClientEndpointBehavior** from the list. This behavior provides the credentials that BizTalk needs to connect to, and listen on, the Service Bus endpoint address. Pick the **SharedSecret** credential type and populate the **issuerName** and **issuerSecret** values of your AppFabric Service Bus account.

![](bbox=[0.32, 0.24, 0.68, 0.45])

21. Save the receive location configuration.

22. Create a new one-way send port (named **Chapter10.SendBillingChange.FILE**) that our orchestration will bind to and send the billing change message to. The port should send its output to the file system.

23. Bind the **ProcessSFDCMessage** orchestration to the host, the relay receive location and finally FILE send port.

24. Start the dependent send port, orchestration, and lastly the receive location. If the receive location is configured correctly, you should now be listening on the cloud.

# Triggering the outbound message from Salesforce.com

With our configuration complete, we can initiate an outbound message and confirm that BizTalk is capable of receiving it:

+   1. Go back to your Salesforce.com site and edit the outbound message configuration so that the Endpoint URL now points to `https://<yournamespace>.servicebus.windows.net/AccountProcessing/BillingChange.svc`. Make sure that our workflow rule is activated.
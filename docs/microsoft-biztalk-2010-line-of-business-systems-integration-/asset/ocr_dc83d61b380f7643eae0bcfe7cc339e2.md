+   - Shape: **Construct**, Name: **ConstructEchoOutbound**, Message Constructed: **msgEchoStringRequest**. Inside the Construct drag the Transform shape and configure it with the following values: Name: **Transform**, Input Messages: **msgReceiveNotification**, Map Name: **Inbound_To_Outbound**, Output Messages: **msgEchoStringRequest**
+   - Shape: **Send**, Name: **SendEchoString**, Message: **msgEchoStringRequest**
+   - Shape: **Receive**, Name: **ReceiveEchoResponse**, Message: **msgEchoStringResponse**
+   - Shape: **Expression**, Name: **WriteToLog**

11. Configure the **WriteToLog** expression shape to execute the following code:

![](bbox=[0.23, 0.36, 0.77, 0.6])

12. To enable the orchestration for receiving and sending messages, we need to add two logical orchestration ports. The first port will be a one-way port for receiving an initial inbound message; the second will be a Request-Response port for sending an outbound message to the adapter for processing and receiving a response. Right-click on the **Port Surface** and select **New Configured Port** to start the **Port Configuration Wizard**. Here are the settings for the first one-way port.

+   - Name: **ReceiveInitialEcho**
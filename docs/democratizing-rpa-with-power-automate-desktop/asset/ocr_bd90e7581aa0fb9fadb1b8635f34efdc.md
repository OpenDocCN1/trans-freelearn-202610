The overall process is divided into two parts: the cloud flow and the Desktop flow.

The Desktop flow provides the following functionality, which should already be very familiar to us:

+   - Receive the incoming values and start the invoicing program
+   - Remotely control the process of entering the invoicing data
+   - Extract the generated invoice ID and return it to the cloud flow

Receiving and providing values is ensured via **Input / output variables**. To enter a new invoice into the Contoso Invoicing application, we need to provide an account name, a contact name, and an amount. Here, we come full circle to our chapter on variables. As shown in the following screenshot, three input variables and one output variable have been defined in the flow:

![](bbox=[0.24, 0.47, 0.76, 0.8])

Figure 9.7 – Input/output variables for cloud communication
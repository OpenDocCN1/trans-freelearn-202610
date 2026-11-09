# Port configuration

We'll begin by creating a Static Solicit-Response Send Port and type in a Name of AXExchangeRates_SendReceivePort.

+   1. We'll set the Type to Microsoft Dynamics AX 2009, and set the Send pipeline and Receive pipeline to XML Transmit and XML Receive respectively.

![](bbox=[0.201, 0.274, 0.799, 0.648])

+   2. Click Configure to bring up the Microsoft Dynamics AX 2009 Transport Properties window.

+   3. Here, we'll select an Authentication Type to Host User from the drop-down list and set the Gateway User to our Active Directory service account that our BizTalk Host instance is running under. We'll set the AOS Port to 2712 and AOS Server to our Dynamics AX 2009 server name AX 2009-SHARED. We can leave the default Synchronous Timeout to 20 minutes.
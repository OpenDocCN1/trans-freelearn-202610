# ASDK SQL Adapter in the BizTalk Administration Console

After we have deployed an application, and using the BizTalk Admin Console imported the binding files generated for us by the Consume Adapter Service Wizard, we may need to adjust adapter configuration for a particular environment.

# Inbound operations

To access adapter properties for the inbound scenarios, we have to navigate to the appropriate receive location and double-click it to bring up the **Receive Location Properties** window. Then click **OK** to open the **WCF-Custom Transport Properties** multi tab dialog.

The **General** tab offers us the Address (URI) text control to set or modify connection string:

![](bbox=[0.19, 0.42, 0.81, 0.6])

The **Bindings** tab, as its name suggests, is where we can get access to the binding properties:
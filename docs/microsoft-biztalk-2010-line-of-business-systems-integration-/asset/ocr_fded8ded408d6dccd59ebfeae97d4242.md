25. On the **Credentials** tab, we need to provide our SAP credentials. In this scenario, we are using **Username/Password** credentials.

![](bbox=[0.35, 0.19, 0.65, 0.5])

26. The final Send port that we need to create will take care of sending our Sales Order status back to the Quoting System. The Send port is called **SendCreateSalesOrderResponsesToQuotingSystem**, and uses the **FILE** Adapter, the **BizTalkServerApplication** Send handler, and the **PassThruTransmit** Send Pipeline. You can obtain the binding file for this solution in the C:\LOBIntegration\Chapter05\BindingFiles of the Source code so that you do not have to type out all of this configuration.

27. Lastly, we need to bind our **ProcessCreateSalesOrder** orchestration to the physical Receive and Send Ports that we just created.
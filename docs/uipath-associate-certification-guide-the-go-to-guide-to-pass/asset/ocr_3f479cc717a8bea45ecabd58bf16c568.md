Now that you have finished entering the customer details on the Order Entry CRM web page, let's move on enter the invoice particulars in the CRM.

# # Entering the invoice particulars

In this section, you will enter the details of the invoice particulars onto the Order Entry CRM web page. To do this, perform the following steps:

+   1. Create a new Sequence workflow with the name of Enter_Invoice_Particulars.
+   2. Create three In direction arguments and one Out direction argument, as shown in the following screenshot:

| Name | Direction | Argument type | Default value |
|---|---|---|---|
| in_invoiceDate | In | String | Enter a VB expression |
| in_invoiceNumber | In | String | Enter a VB expression |
| in_invoiceParticulars | In | String | Enter a VB expression |
| out_flag | Out | Boolean | Default value not supported |

Figure 15.22 – The Arguments panel

+   3. Drag and drop the Try Catch activity inside the Enter_Invoice_Particulars sequence.
+   4. Inside the Try block of the Try Catch activity, drag and drop the Deserialize JSON activity and configure the properties as follows:
+   * Pass in in_invoiceParticulars under the JsonString field.
+   * Select the TypeArguments property as System.Data.DataTable.
+   * Create a new variable, named dt_invoiceParticulars, under the JsonObject field using Ctrl + K:
After assigning all the values in the **Value** section, your **ItemInformation** window should look as follows:

| Name | Direction | Type | Value |
|---|---|---|---|
| invoiceDate | In | String | in_invoiceDate.Split(":", StringSplitOptions.None)(1).ToString.Trim |
| invoiceNumber | In | String | in_invoiceNumber.Split("#", StringSplitOptions.None)(1).ToString.Trim |
| customerName | In | String | in_customerName |
| customerAddress | In | String | in_customerAddress |
| customerPhoneNo | In | String | in_customerPhoneNo |
| customerEmail | In | String | in_customerEmail |
| invoiceParticulars | In | String | JsonConvert.SerializeObject(in_dt_invoiceParticulars) |

Figure 14.46 – The ItemInformation window with all the values assigned

+   11. The invoice date and invoice number have extra characters in them. To get rid of them, use the Split() function with the appropriate delimiter. As mentioned earlier, the DataTable arguments have to be serialized before uploading to the queue. The SerializeObject() function of the JsonConvert class will help serialize it into a JSON object.

+   12. Finally, inside the Catches block, catch a SystemException.

+   13. Also, log a message, with Log Level as Error and Message as "Unable to Upload to Queue" + vbLf+exception.Message + vbLf + exception.Source.
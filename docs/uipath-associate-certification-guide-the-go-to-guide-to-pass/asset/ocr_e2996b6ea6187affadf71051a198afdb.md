![](bbox=[0.17, 0.09, 0.8, 0.51])

# Figure 15.27 – The If activity

+   18. Below the `For Each Row` activity, drag and drop the `Type Into` activity and indicate the **SubTotal** text field on the Order Entry CRM web page. Then, type in `dt_invoiceParticulars.Rows (dtCount- 3) (3) .ToString.Trim` underneath the **Text** property.
+   
+   19. Below the subtotal `Type Into` activity, drag and drop a new `Type Into` activity and indicate the **Shipping** text field in the Order Entry CRM web page. Then, type in `dt_invoiceParticulars.Rows (dtCount- 2) (3) .ToString.Trim` underneath the **Text** property.
+   
+   20. Below the shipping `Type Into` activity, drag and drop a new `Type Into` activity and indicate the **Total** text field on the Order Entry CRM web page. Then, type in `dt_invoiceParticulars.Rows (dtCount- 1) (3) .ToString.Trim.Split ({ "$" }, StringSplitOptions.None) (1)` underneath the **Text** property.
+   
+   21. Use a `Click` activity and indicate the **Submit** button. The bot will click on the **Submit** button, and all the values entered in the CRM will be saved.
+   
+   22. Below the `Attach Browser` activity, drag and drop the `Assign` activity. Pass `out_flag` into the **To** section, and type `True` into the **Value** section.
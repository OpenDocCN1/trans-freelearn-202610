+   - Data table visualization:
+     A. **Custom Search – Firewall Events** source
+     B. **Split rows** data bucket, **Aggregation** set to **Terms**, and **Field** set to **source.port**
+     C. Set **Size** to **50**
+     D. Save as `Firewall - Source Port Summary`

+   - Data table visualization:
+     A. **Custom Search – Firewall Events** source
+     B. **Split rows** data bucket, **Aggregation** set to **Terms**, and **Field** set to **destination.ip**
+     C. Set **Size** to **50**
+     D. Save as `Firewall - Destination IP Summary`

+   - Data table visualization:
+     A. **Custom Search – Firewall Events** source
+     B. **Split rows** data bucket, **Aggregation** set to **Terms**, and **Field** set to **destination.port**
+     C. Set **Size** to **50**
+     D. Save as `Firewall - Destination Port Summary`

To wrap up the firewall logs dashboard, we will add some existing panels – stuff I tend to add to every dashboard:

+   1. Click on the **Add from library** button and search for and select the Security Onion **Navigation** panel.

![](bbox=[0.31, 0.66, 0.66, 0.85])

Figure 9.41 – Exercise 5 – Adding a widget from the library
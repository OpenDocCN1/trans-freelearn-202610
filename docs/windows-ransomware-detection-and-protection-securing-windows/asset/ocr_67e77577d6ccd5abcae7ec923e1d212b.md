Labels can be assigned to the following objects:

+   - **Items:** This applies to emails, Office files, and other non-Office files that are manually labeled using the AIP client
+   - **Groups & sites:** This applies to Microsoft Teams sites, Microsoft 365 groups (formerly Office 365 groups), and SharePoint sites
+   - **Schematized data assets:** Since this feature is in preview, here is a link explaining what kind of data it applies to since it might change: `https://docs.microsoft.com/en-us/azure/purview/how-to-automatically-label-your-content#scan-your-data-to-apply-sensitivity-labels-automatically`

Next, we can define whether we want to either apply encryption or mark the files with custom content such as headers, footers, or watermarks. In this example, we chose to encrypt items, which will give us the settings shown in the following screenshot.

![](bbox=[0.188, 0.391, 0.771, 0.779])

Figure 7.5 – Encryption mechanism for AIP

Click **Assign permissions to specific users and groups** and assign a permission to the label. In this example, as seen in the following screenshot, let’s choose **Add all users and groups in your organization** and click **Save**.
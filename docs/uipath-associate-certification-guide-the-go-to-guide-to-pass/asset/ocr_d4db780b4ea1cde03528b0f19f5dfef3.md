+   2. Click on the **Import Arguments** button and pass the isMail variable as the value to the out_isMail argument:

![](bbox=[0.15, 0.155, 0.85, 0.345])

| Name | Direction | Type | Value |
|---|---|---|---|
| out_isMail | Out | Boolean | isMail |
| Create Argument | | | |

Figure 14.49 - The Invoke Download_Invoice workflow

+   3. Next, drag and drop the **If** activity and pass the **isMail** variable in **Condition**. If the **isMail** variable value is **True**, then you can start reading the invoices; otherwise, you will stop the process.

+   4. Drag and drop the **Read_Invoice.xaml** workflow inside the **Then** block of the **If** activity from the **Project** panel.

+   5. Below that, add a **Log Message** activity at the **Info** level and set the message to "All invoices details uploaded to queue".
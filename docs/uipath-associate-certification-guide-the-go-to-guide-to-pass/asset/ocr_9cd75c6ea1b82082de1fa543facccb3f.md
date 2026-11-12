15. Click on the **Import Arguments** button and pass the variables for all the seven In arguments, as shown in the following screenshot:

| Name | Direction | Type | Value |
|---|---|---|---|
| in_invoiceDate | In | String | invoiceDate |
| in_invoiceNumber | In | String | invoiceNumber |
| in_customerName | In | String | customerName |
| in_customerAddress | In | String | customerAddress |
| in_customerPhoneNo | In | String | customerPhoneNo |
| in_customerEmail | In | String | customerEmail |
| in_dt_invoiceParticulars | In | DataTable | dt_invoiceParticulars |

Figure 14.48 – Importing the arguments

Now that you have completed the uploading to the queue section, it is time to assemble the Main.xaml file to run the process.

# Assembling Main.xaml

It’s time to run the process, but let’s invoke the Download_Invoice.xaml and Read_Invoice.xaml workflows in the Main.xaml file before running the process:

+   1. In the Main.xaml workflow, create a variable named isMail of the boolean type. Then, drag and drop the Download_Invoice.xaml workflow in the Main.xaml workflow from the Project panel.
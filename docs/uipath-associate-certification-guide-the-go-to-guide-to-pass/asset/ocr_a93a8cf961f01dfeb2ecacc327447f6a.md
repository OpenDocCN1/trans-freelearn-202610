5. Next, drag and drop the `For Each` activity so that you can iterate over each invoice and extract information from them. Then, pass `invoices` in the **Values** section and change `item` to `invoice` in the `For Each` activity.

6. Add an `Assign` activity inside the body of the `For Each` activity and pass the `InvoicePath` variable in the **To** section and `invoice.ToString` in the **Value** section:

![](bbox=[0.21, 0.235, 0.76, 0.62])

Figure 14.16 – The Read_InInvoice sequence

Now that we can iterate over each invoice, let’s first extract customer details from the invoice. After that, we’ll extract invoice particulars too.

# ## Extracting customer details

In this section, you will extract the customer details from the read invoice. The following diagram explains the high-level process of extracting customer details from the invoice:

![](bbox=[0.28, 0.81, 0.68, 0.88])

Figure 14.17 – Extracting customer details
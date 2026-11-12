![](bbox=[0.192, 0.096, 0.775, 0.423])

Figure 15.14 – Fetching Invoice Date

+   5. Drag and drop a Sequence activity underneath the Fetching Invoice Date sequence and rename it to Fetching Invoice Number.

+   6. Add an Assign activity inside the Fetching Invoice Number sequence. Under the **Property** panel, type `transactionItem.SpecificContent ("invoiceNumber") .ToString` into the **Value** section. Next, in the **To** section, create a new variable, named `invoiceNumber`, of the `String` type.

+   7. Drag and drop a Sequence activity underneath the Fetching Invoice Number sequence and rename it to Fetching Customer Name.

+   8. Add an Assign activity inside the Fetching Customer Name sequence. Under the **Property** panel, type `transactionItem.SpecificContent ("customerName") .ToString` into the **Value** section. Next, in the **To** section, create a new variable, named `customerName`, of the `String` type.

+   9. Drag and drop a Sequence activity underneath the Fetching CustomerName sequence and rename it to Fetching CustomerAddress.

+   10. Add an Assign activity inside the Fetching CustomerAddress sequence. Under the **Property** panel, type `transactionItem.SpecificContent ("customerAddress") .ToString` into the **Value** section. Next, in the **To** section, create a new variable, named `customerAddress`, of the `String` type.
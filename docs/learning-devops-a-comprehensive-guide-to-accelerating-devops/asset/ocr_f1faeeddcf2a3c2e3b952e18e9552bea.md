It is for this reason, therefore, that we will also have to export the information from each
environment in JSON format so that we can also pass it on as an argument to Newman.

To export the environments and their variables, perform the following tasks:

+   1. Open **ENVIRONMENTS** from the left panel in Postman.

+   2. Then, click on the **Export** environment button.

These steps are shown in the following screenshot:

![](bbox=[0.115, 0.273, 0.836, 0.541])

Figure 11.24 – Postman Export environment

So, for each environment, we will export their configurations in a JSON file, which we
save in the same folder where we exported the collection.

Finally, we have a folder on our machine that contains three Postman JSON files:

+   • One JSON file for the collection

+   • One JSON file for the `Local` environment

+   • One JSON file for the `QA` environment
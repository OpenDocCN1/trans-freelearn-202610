To generate these PDFs, you can assume a batch MuleSoft job that runs every month, retrieves the invoice details from the ERPs, and passes them to the PDF Generator system to generate PDFs. MuleSoft would then retrieve all files stored on the SFTP and transfer them to Salesforce. You can use a specific file naming convention that includes the account ID. This will allow MuleSoft to attach the files to the correct Account record.

Add the Content Document object to your data model. Your data model could now look like the following:

![](bbox=[0.16, 0.255, 0.85, 0.85])

Figure 14.11 – Data model (second draft)
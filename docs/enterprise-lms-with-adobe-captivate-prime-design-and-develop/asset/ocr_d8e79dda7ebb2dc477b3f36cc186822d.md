+   6. Save the CSV file when you're done.

> **Double-check the CSV file before uploading it**
> Remember that, depending on the regional settings of your system, the separator that's used in the CSV file may have been switched to a semicolon (;) instead of a comma (,). To fix this potential issue, open the CSV file in a text editor (such as Notepad on Windows or TextEdit on Mac) and use the *Find and Replace* feature of the text editor to replace all semicolons (;) with commas (,).

+   7. Open a new browser window and browse to https://primedev.extravault.com/.

+   8. Log in to your Extravault account using the username you received via email and the password you created earlier.

+   9. Once logged in, browse to the import/user/internal folder of your Extravault account.

+   10. Upload the users/internalUsers.csv file using either drag and drop or the Upload button in the top-right corner of the Extravault interface.

> **Note**
> When using this workflow in production, you will probably use an FTP client such as FileZilla to upload the CSV file to Extravault.

After completing this section, the CSV file will have been correctly uploaded to your Extravault account. The last step is to ask Captivate Prime to connect to your Extravault account and fetch this CSV file regularly. This is what you will do in the next section.

# Configuring a data source

There is one last piece you must configure for this workflow to be up and running. In this section, you will return to the administrator application and configure the periodic data import by Captivate Prime from your Extravault FTP account. Let's get started:

+   1. Make sure you are connected to your Prime account as an Administrator and that you are on the Administrator home page.

+   2. From the Administrator home page, click the Settings link located in the Configure section of the left sidebar. Then, click the Data Sources link located in the Integrations section of the left sidebar.
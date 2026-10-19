Input the options shown in the following screenshot:

![](bbox=[0.13, 0.13, 0.86, 0.5])

Figure 12.46 – Create options for the Azure storage account SAS token

Click on Generate SAS and connection string.

Take note of the SAS token value, as shown in the following screenshot:

![](bbox=[0.13, 0.59, 0.86, 0.66])

Figure 12.47 – Azure storage account SAS token

2. We will now add secrets to the newly created Azure Key Vault. Using the Azure portal, navigate to Home > Resource groups > elz_prod > deploykvelz.

Click on Secrets. Then, create two new secrets using Generate/Import, like so:

+   - Name: ADLSKEY Value: Use the value of key1 from the preceding command

+   - Name: ADLSSASKEY Value: Use the value of SAS token from the preceding command:
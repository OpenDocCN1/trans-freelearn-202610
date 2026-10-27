5. Leave all the other fields at their default values, scroll to the bottom of the screen, and click on **Upload** to start uploading the files. There is a total of 12.5 MB and 6,553 files to upload. This may take a while depending on your internet upload bandwidth. An upload status page is displayed while the transfer is in progress.

> **Note**
> If you feel the transfer of 6,553 files is too slow using the browser, I recommend using the **AWS Command Line Interface** to optimize your upload speed:
> `https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html`. Another option would be to use **AWS CloudShell** to open a command line in your browser while connected to your console:
> `https://docs.aws.amazon.com/cloudshell/latest/userguide/welcome.html`.

6. Once this is done, you can click on the **Exit** button to be brought back to the objects list at the root of your newly created folder.

Once your upload is complete, you should see two subfolders in your objects listing, one called `backtest` and the other named `live`.

# Giving access to your data to Amazon Lookout for Metrics

By default, the security mechanisms enforced between different AWS services will forbid any service other than Amazon S3 to access your data. From your account, you can upload, delete or move your data from the bucket you just created. Amazon Lookout for Metrics, however, is a different service and will not be able to access this data. We need to specify that Amazon Lookout for Metrics can access any data in this bucket.

> **Note**
> You can configure this access directly from the Amazon Lookout for Metrics console during the ingestion step. However, if you want to have more control over the roles and the different accesses created within your account, you can read through this section. Otherwise, feel free to skip it and come back here later.
+   8. You will end up on the **Create target time series dataset** screen that you encountered in *Chapter 3, Creating a Project and Ingesting Your Data*, when you ingested the data the first time. Many fields are now grayed out and disabled. Scroll down to **Dataset import details:**

![](bbox=[0.137, 0.183, 0.862, 0.544])

Figure 6.6 – Configuring a new dataset import

On this screen, you will need to do the following:

+   I. Give a name to your import. This will help you distinguish between different import jobs. We will call it `energy_consumption_20130730` (I added the timestamp of the last date available in the new data).

> **Important Note**
> 
> Note that successive import jobs are not aggregated: only the most recent import will be considered when you use your datasets to train a predictor.

+   II. Enter the S3 URL for the target time series where you uploaded your new dataset earlier in Amazon S3.
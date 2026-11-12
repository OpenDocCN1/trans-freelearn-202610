True, it is probably simplistic to consider data that merely exceeds Excel's row limitation as big data; nevertheless, from the perspective of an individual for whom Excel is the traditional data-processing application, the preceding definition fits.

Talking about big data goes hand in hand with parallel processing. For Tableau and working with big data, it is very important to know the partitions that your IT team has put in place for parallel processing. It could be, for example, the YEAR, MONTH, and DAY columns. If you use those columns that are also used as partitions in the database, your Tableau dashboard will perform so much better than trying to query on a random field in the dataset. So, first, put filters in place—preferably partitions. Then, build your dashboard on a subset of the data. Once you are all done, load the data you need and check the performance again. More information regarding performance is given in Chapter 12, Improving Performance.

Many organizations opt for a cloud-based solution, such as AWS, Azure, or Google Cloud Platform (GCP). Within GCP, you will find BigQuery, which we will use for the next exercise.

# # Building a visualization with Google BigQuery

One big data solution is BigQuery from Google, next to many others from providers such as Amazon, Alibaba, Azure, IBM, and so on. For this section of the book, we have randomly chosen BigQuery, which you can test for free if you have a Gmail account. Other providers offer free tier accounts too. Since we are working with the cloud, you don't have to install anything, which makes Google Cloud a good platform for an exercise in this book.

To build a visualization with Google BigQuery, you will need to first set up access to BigQuery. The following exercise will point you in the right direction. Once you have set up access to BigQuery, you will be able to connect to the BigQuery sample datasets. In the remainder of the exercise, you will build a visualization while connected to BigQuery.

Assuming you have a good internet connection, the performance will likely exceed what you experience when working with a local copy of an extracted data source of a similar size.

Let's have a look at how we can use Google BigQuery in our Tableau dashboard:

+   1. Log in to your Google account, navigate to https://c1oud.google.com/bigquery/, and follow the provided instructions to try BigQuery for free.
+   
+   2. In Tableau Desktop, select Connect | To a Server | Google BigQuery.
+   
+   3. Use Authentication | Sign in using OAuth. Now, you can select your Google account to connect. On the Data Source page, you have the option to choose any billing project you have set up when registering on GCP. In my case, it's called BigDataProject.
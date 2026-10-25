# # Setting the dataset query mode

Earlier in this chapter, we configured a Redshift dataset as a Direct Query. Let’s assume  
that we realized that our data warehouse is busy serving many applications and producing  
reports for the organization, so we would prefer to import the data into SPICE, so that  
the QuickSight dashboards are using the SPICE storage, rather than accessing the data  
warehouse directly. After importing the data into SPICE, our data warehouse will only be  
used for data imports, and not for querying the data to populate the dashboards.

Now let’s add our dataset into SPICE and configure its import duration:

+   1. First, you will need to log in as the author user we created earlier.
+   2. Then, select datasets on the left-hand side menu to get a list of all the datasets.
+   3. For this hands-on example, we will import the `yellowtrips_3mo` dataset into SPICE. Click on `yellowtrips_3mo` and then click **Edit dataset** on the pop-up window you see in the following screenshot.

![](bbox=[0.1719, 0.4414, 0.7949, 0.7031])

Figure 3.6 – Dataset pop-up screen
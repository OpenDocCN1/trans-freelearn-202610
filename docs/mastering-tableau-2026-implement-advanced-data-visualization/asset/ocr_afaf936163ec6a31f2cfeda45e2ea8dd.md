![](bbox=[0.29, 0.07, 0.71, 0.4])

Figure 5.8: Final index

Let's consider how the preceding worksheet functions:

**Index**: INDEX() counts from 1 to *n*. As such, it behaves directionally. In this case, as a result of the partitioning and addressing settings, **Index** is counting postal codes. (Partitioning and addressing will be discussed in detail in the *Application of functions* section.) Setting the **Index** filter to display only values of 50 or more ensures that only those states with 50 or more postal codes are in the partition/dataset in the cache display.

Real-world scenario: most often, I use **Index** interchangeably with **First** and **Last**.

# # Rank

The objective of the following worksheet is to display the top three selling items in each region. This example will demonstrate how RANK interacts directionally with the dataset in the cache:

+   1. In the workbook associated with this chapter, navigate to the **Rank** worksheet.

+   2. Place **Product Name** on the Filters shelf, select the **Top** tab, and choose **Top 3** by **Sum of Sales**:

![](bbox=[0.3, 0.73, 0.7, 0.86])

Figure 5.9: Filter
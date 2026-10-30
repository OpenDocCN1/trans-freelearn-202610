# # Diving into data joins

We looked briefly at joining and augmenting your data in *Chapter 4, Building Data Recipes.*
In this section, we will dive much deeper into this important area of data transformation.
First, let's begin by understanding the various types of data joins.

# ## Understanding the various types of joins

There are four common ways to join your data, as seen in the following diagram:

![](bbox=[0.252, 0.272, 0.714, 0.557])

Figure 5.1 – The four types of Structured Query Language (SQL) data joins

The four types of joins can be understood as follows:

+   - **Left join:** A left join includes all rows from the left (recipe) data stream and only adds matching rows from the right data stream.
+   - **Right join:** A right join includes all rows from the right data stream and only adds matching rows from the left (recipe) data stream.
+   - **Inner join:** An inner join only includes matching rows from the left (recipe) data stream and right data stream.
+   - **Full (outer) join:** A full outer join includes all rows from the left (recipe) data stream and right data stream, regardless of whether or not they have matches.

> **Note**
> 
> These joins include all matched rows in the target table when multiple rows match.
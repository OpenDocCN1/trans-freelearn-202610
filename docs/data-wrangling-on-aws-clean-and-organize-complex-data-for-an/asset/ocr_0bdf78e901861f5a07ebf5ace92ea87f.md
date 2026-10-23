**Challenge**

Can you also update the script to process nested lists? One example could be that a stadium has a list of food outlets and each food outlet has a list of menu items.

**Hint**

You can try to store the primary key separately for each table in the hierarchy and then add them to the child table along with the iterator column specific to that table.

In the next section, will learn more about using pandas’ operations for performing data cleaning activities.

# Data cleaning

Data cleaning is an important step in the process of data wrangling. A good amount of time is spent on identifying the right data source and cleaning the data. Pandas provides a lot of functionalities for cleaning your data.

The exact activities that are required during this phase are different for each type of dataset. Certain data sources will have data that requires only minimal cleaning and certain other data sources might require a lot of cleaning activities before the dataset can be used in your project. You could also use the output of data exploration activities to understand the level of cleaning activities to be performed on the data.

# Data cleansing with Pandas

In order to demonstrate the data cleaning steps, we will use the `seat_type` table from our database. This table only has minimal data volume, so we will insert some data before we proceed with data cleaning.

The data in `seat_type` looks like the screenshot here. It has three columns for the seat type name, a description of that seat type, and a column that provides a relative ranking of the quality of that seat type:
+   - **Currency:** A dimension that links the currency name with its alphabetic code, and eventually other info, such as symbol, number of decimals to use, and emitting country/entity
+   - **Country:** A dimension that has both the two-letter and three-letter country codes, country name, region, sub-region, and so on

This is just an example list; you can probably find more depending on your interest.

We suggest that you pick one or more and implement all the steps yourself to have them in use in your data mart:

+   1. Load the CSV as a seed, including the generation of LOAD_TS.
+   2. Define the source for the CSV as another seed data table.
+   3. Create the STG model to adapt the incoming data.
+   4. Create the snapshot to store the changes in the dimension data.
+   5. Create the refined model, reading the current data out of the snapshot.
+   6. Create the DIM model in the data mart.

In the book’s Git repository, under the folder for this chapter, you will find a csv folder that contains three CSV files with data about currencies, countries, and exchanges.

# Summary

In this chapter, we learned about dimensional data and how it enhances the usability of data platforms and data marts. We learned how to incorporate dimensional data in our data platform, using the basic features of dbt.

We reinforced our understanding of the layers of our Pragmatic Data Platform that we learned about in the previous chapters and applied those principles to work with dimensional data.

In the next chapter, we will start exploring more advanced features that will allow us to replace some of the basic code to produce a more resilient and easier-to-write and easier-to-maintain data platform.
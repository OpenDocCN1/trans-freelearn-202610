amounts of rows as the **Sales** table (the fact table). Therefore, we do not improve the performance of the data model by adding a new dimension. Indeed, we are forcing unnecessary costs, such as the extra development costs of adding an **Index** column to the **Sales Orders** dimension, adding that **Index** column as a foreign key into the **Sales** table, and creating the relationship between the two tables. The overhead development costs aside, we actually degrade the compression of the model and its performance.

Now, let us analyze the necessity of having the **Currency** column in the fact table. Looking at the data reveals that there are no other attributes for the **Currency** column. As the xVelocity engine uses columnstore indexing technology, it best compresses the low cardinality data. Therefore, the fact that the data type of the **Currency** column is **Text** does not hugely affect the data compression and the performance of the data model. So, it does not make sense to create a new dimension for it.

So far, we are convinced that keeping the degenerate dimensions in the **Sales** table is a simpler implementation, which also aligns with the second point of Webster’s definition of the term “degenerate.” Conceptually, degenerate dimension can be a tricky concept to internalize and apply to real-world scenarios, which some developers get wrong. They put dimensions into the fact tables and name them degenerate dimensions, which can impact performance negatively.

The next chapter focuses on another important data modeling tool available in Power BI: Dataflows.

# Summary

In this chapter, we learned about the concept of SCD and how to implement some related scenarios. We also learned more about degenerate dimensions and how to deal with them in our data models in Power BI. The next chapter explains the purely cloud-based data transformation capability in Power BI called Dataflows.
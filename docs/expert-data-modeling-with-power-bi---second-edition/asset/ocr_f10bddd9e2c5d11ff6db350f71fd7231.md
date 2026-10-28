# Data Analysis eXpressions and Data Modeling

The previous chapter discussed that Power BI has different layers: data preparation, data modeling, and data visualization. This chapter discusses Data Analysis eXpressions (DAX), and their relationship to data modeling. Although data modeling and DAX are connected such that you cannot imagine one without the other, our goal in this book is not to focus only on DAX. Data modeling encompasses much broader concepts, while DAX is the expression language developers must use to implement business logic in the data model. We assume that you have an intermediate level of knowledge of DAX; therefore, we do not cover basic DAX concepts in this book such as DAX syntax. Instead, we focus on more advanced ones with hands-on scenarios.

This chapter covers the following topics:

+   - Understanding virtual tables
+   - Understanding relationships in virtual tables
+   - Time intelligence

# Understanding virtual tables

The concept of virtual tables in DAX is somewhat confusing and misunderstood, yet it is one of the most powerful and important concepts in DAX. When we talk about virtual tables, we refer to in-memory tables that we build using certain DAX functions or constructors. The data in a virtual table is either derived from the data within the data model or the data we construct for specific purposes.
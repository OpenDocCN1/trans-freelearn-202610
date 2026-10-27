
## Why do perfectly good data models generate these problems?

The short answer is that two tables that have an FK in common, such as a customer key or a product key, are probably related in some way, but the common key alone is not enough to express the possibly multiple and complex relations between the two tables. Trying to extract information without enough data in general does not provide meaningful results.

Let's look at an example: take the case of orders and deliveries, but the same would also apply if you also considered related tables, such as returns or payments.

They all have a reference to Customer, Product, or both, but what would be the point of joining orders and deliveries by customer? Having each order from a customer paired with each delivery to that customer does not make much sense.

It makes sense to match the grouped orders for the customer with the grouped deliveries to that same customer, so you can compare summarized data for that customer and do all calculations you want with the summarized measures. Having a one-to-one match between the summarized tables will provide perfectly good results, without any duplication.

It also makes sense to match a single order with the deliveries done to deliver that specific order, but to do that, you need more data than just the customer and/or product keys. If the delivery has the order key, then you are all set because you have the relation you are looking for.

If you do not have that key, you might be lucky enough to be able to use some logic to try to do some exact or approximate match between orders and deliveries, but you must use more info than just the customer and/or product with the ultimate goal to try to allocate each order and delivery so that you do not have duplicates. A real relation provided by the data is one thing; it's another thing to have an approximate one based on some heuristics.

As always, the semantics of the data that you have and of what you want to do should be your best guide. Refrain from joining two tables just because they have an FK in common.

Now that you know about data model types, how to represent a data model, and its possible problems, we will close the chapter by looking at different modeling styles and architectures that frame the use of data models.

## Modeling styles and architectures

In this chapter, we have introduced data modeling and seen how it is used to describe the data we have or how we want it to become.

Data modeling is the basic tool to describe the data but building a data platform is more than describing the data hosted in the platform and the relationships between the various entities. As an example, how you load new data into the tables and how you update all the intermediate tables to keep your data marts up to date is a key point that is not captured by a data model.

In the next chapter, we will look in more detail at the overall data life cycle, but in this section, we want to introduce a few design styles that are heavily connected to how you develop data models for your data platform.

The great news is that with dbt, there are no preferences or limitations, and you can adopt any of these different paradigms and implement whichever will work best.

We will start with the Kimball method, which introduces the star schema, which most of the time will be the model style for the data marts used by your BI tools to access the data.

A word of caution before we begin: each of the following subjects has been explained in more than one book, so we will not try to teach you them in any detail. The goal of the following sections is to give you a starting idea about some of the most common paradigms you might encounter and discuss some of their features and problems.

At the end of this chapter, there is a list of books if you want to look in more detail into these topics.

Do not worry if you do not understand or cannot follow all topics. We will come back to many of them in the rest of the book, as we will try to pick the best out of these approaches.

### Kimball method or dimensional modeling or star schema

Probably the most ubiquitous design style in the data warehousing world is the **dimensional model** from **Ralph Kimball**, which is the de facto standard in the BI area.

The signature feature of the dimensional modeling design is to produce a data model structure called a **star schema**, which is the access interface to the data for BI tools.

In a star schema, a **fact table** containing the events or records containing the measures of interest is surrounded by several dimension tables. A **dimension table** is a denormalized table that contains the descriptive data at the lowest level of detail that you want to access and should not contain measures (in the original Kimball definition) to avoid the fan trap issue. A **measure** is a number that you want to use for calculations.

The following figure shows a typical star schema organized around an **Order** fact:

![](img/8868d272a75eee8b407a8e0f3434d10f_159_0.png)

Figure 3.23: Example of a star schema

The main advantage of a star schema is its simplicity, which makes it very simple to query and easy to understand even for non-technical people.

The simple pattern in use is that each row in the fact table contains the FK to the dimensions, such as **Product_ID** or **Country_Code**, and the measures, such as **Amount_ordered**, **Price**, and **Discount**.

The relevant FKs are used to join on the needed dimension to retrieve the descriptive data desired in the specific report, such as the product name, the customer’s name or address, or the VAT code.

Given its simple structure and exclusive use of many-to-one relations from the fact to the dimension, it is also very performant to query and therefore well suited to provide data for interactive reporting.

The dimensional modeling design style has the objective to build these facts and dimensions starting from the source data. In principle, each fact might be joined with its own set of dimensions with different keys and columns, but you can see that having a different customer and product for each different fact will create a huge number of similar but different dimension tables.

The concept of **conformed dimension** is used to avoid the multiplication of dimension tables and integrate the data from different systems. A conformed dimension is just a dimension, but it is designed to be used by all or at least most of the fact tables that are interested in such descriptive data. It must therefore be at the smallest granularity needed and must contain all columns of interest in all reports that have to be built on all facts that use the dimension.

The Kimball data warehouse methodology uses a matrix with facts in rows and dimensions in columns to prioritize the addition of conformed dimensions and facts. The matrix describes the content of the star schema that surrounds each fact.

The data processing in a Kimball data warehouse is based on transforming the incoming source data to build the needed conformed dimensions. The transformations are done as directly from the data sources to the facts and dimensions as possible and the facts and dimensions are the structures that store the data. This network of facts connected to dimensions is the central data model of the data platform in a Kimball warehouse.

> **Tip**
> A *data mart* is a collection of data, generally a small subset of all the available data, that is selected to satisfy the BI needs of a specific business area, such as a department, or use case.
> 
> The idea is to provide a limited number of tables, and often a reduced number of attributes in those tables, which makes it easier for analysts to find the desired data and produce correct reports.
> 
> A data mart often comes in the form of a handful of facts and the relevant dimensions to provide one or more star schemas that are used to analyze these facts in the context of the selected subject.
> 
> The collection of the available data marts is the only external interface that most users have to access the data platform, so it is extremely important that the names and semantics of the available tables, such as facts and dimensions, and attributes are clear.

We have seen the main concepts of a Kimball warehouse, so let’s also look at some of its downsides:

- According to the original definition, there should not be measures in dimensions, because if you have them and you use them for calculations involving the fact, you always have a fan trap. Not having or not using important metrics like the price in a product dimension is very limiting, so in practice, we have measures in the dimensions, then you should be aware when a Fan Trap is triggered.
- Another limitation according to the Kimball rules is that facts are only connected to dimensions, not other facts. But some of the most interesting analyses are built by using data from multiple facts, such as orders and deliveries or returns. Kimball star schemas do not help here, so you can look at other methodologies, such as the USS, or write your own SQL to produce a new joined fact to be analyzed.
- The Kimball method is oriented to report business processes, not build a data view of the enterprise, so the facts and conformed dimensions might not handle other reporting needs besides the ones that the star schemas were designed for.
- The process of loading and storing the data from the data sources is deeply intertwined with the implementation of the business rules because the conformed dimensions and the facts are both the goal of the business transformations and the primary storage for all the data.

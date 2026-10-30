
These fields can also be renamed in the **Transform** node, as illustrated in the following screenshot:

![](img/06379e43783b9ddfee45e9da5192efe1_147_0.png)

Figure 5.28 – Renaming fields in the Transform node

Regarding the two highlighted columns, the following applies:

- **Role ID Path**: This column has all the role IDs that are above the role ID in the row listed, separated by a backslash. If **Include child node** is enabled, it lists the role ID of the row as well in the path (self).
- **Role ID Nodes**: This column has all the role IDs in the hierarchy of the role ID in the row listed as a multi-value.

The **Transform** node can then be used in the join to map the owner ID of the opportunity record and the user ID from the user role to determine the accessibility of each record, as seen here:

![](img/06379e43783b9ddfee45e9da5192efe1_147_1.png)

Figure 5.29 – Use of the Transform node in the join to map the owner ID of the opportunity record and the user ID from the user role

This **Role ID Path** field or **Role ID Nodes** field is then used in the security predicate to determine the accessibility of a row in the dataset. See *Chapter 7, Security in Tableau CRM*, for more information about security predicates.

## Predict Missing Values – how and when to use

When can you use the **Predict Missing Values** transform to fill in missing values in a dimension column? First, in order to make an accurate prediction, each column must have fewer than 200 unique values. Also, you must verify that these predictive columns contain clean, high-quality data, or the predicted values will be misleading.

How can you use the **Predict Missing Values** transform? Note the following:

- When you click **Apply** to add a transformation to the **Transform** node, the preview pane will show the original column with the missing values and the new column with **Predict** at the end of the header.
- You will see **Prediction TBD** for predicted values in the new column.
- The predicted values will not appear until after you run the recipe.
- Run the recipe to generate the predictions—you can then view the dataset as a values table to see the predictions.
- Add the **Drop Columns** transformation after the **Predict Missing Values** transformation to drop the original column from the dataset, as required.

> **Limitations to Keep in Mind**
> 
> Firstly, if there are not enough rows to make accurate predictions, TCRM will not insert any predicted values. Secondly, you cannot apply any transformations on predicted columns. Finally, recipes that predict values can sometimes take longer to run.

## Sentiment detection explained

Using **natural language processing (NLP)** and ML, sentiment detection can extract valuable information from text fields, such as reviews, case comments, and social media posts. Use the **Detect Sentiment** transformation in a Data Prep recipe to categorize that information into one of the following three sentiments:

- **Positive**
- **Negative**
- **Neutral**

In other words, you don't need a new ML project, an ML system, new infrastructure, or a separate data pipeline to detect sentiments from your unstructured data. The **Detect Sentiment** transformation in TCRM Data Prep does it all!

Some potential use cases for the **Detect Sentiment** transformation include the following:

- Customer service/**customer satisfaction (CSAT)** based upon survey responses
- Sales coaching and trend identification
- Social media brand management

The **Detect Sentiment** transformation supports English text only and works on a single-dimension column. It processes non-English text as English and ignores all images. The transformation returns `null` when the input value is `null` and returns `neutral` when the input value is an empty string (`''`).

## Advanced hands-on exercise

The purpose of this exercise is to create a single dataset that contains a summary of every account that meets all of the following conditions: has open opportunities, at least one activity, and an open case attached to the account.

The resulting dataset should allow the user to determine the following via direct exploration of the lens based on the dataset:

- Account name
- Age of account
- Owner name
- No. of open opportunities
- No. of open cases
- Average case duration
- No. of activities
- Total amount
- Total expected amount
- Total amount (opportunity with line items)
- Total amount (opportunity without line items)

### Resources required

Data sources that are available in your TCRM environment will be used for the purpose of this exercise. Please use the datasets found under the My First Analytics App app so that the results will be consistent with the following steps:

- Opportunities
- Cases
- Activities
- Accounts
- Users

### Steps to follow

Build a new recipe in your demo TCRM environment and follow these steps:

1. Select Opportunities, Accounts, Activities, Cases, and Users for your recipe input data. The panel on the right allows the selection of columns, and although you can proceed by selecting all columns, ideally you would only want to work with the columns required.
2. Add filters that are required as per the exercise objectives.
3. Add some aggregation nodes as you know that Opportunities, Cases, and Activities have a lower granularity than Accounts. As such, you will need to aggregate all of them to the same granularity (Accounts) so that you are able to perform joins in the later steps to meet the objectives.
4. Your aggregation on the Opportunities data input will split the opportunity and expected amounts into two columns—with line items and without, as that was a requirement. However, this also means that you do not have a field that has the total amount and total expected amount. Create a Transform node that helps you bring the totals back into the dataset build.
5. Make the necessary joins so that you can work toward your final dataset.

When you open the completed dataset as a lens, it will look like this:

![](img/06379e43783b9ddfee45e9da5192efe1_152_0.png)

Figure 5.31 – Completed dataset for the hands-on exercise: bar view

Looking at the dataset as a values table, it will look like this:

## Summary

In this chapter, you learned how to use the more advanced ETL tools available in Data Prep and TCRM recipes. You should now be able to join, aggregate, flatten, and transform your data with confidence using the TCRM data preparation GUI. This includes the use of complex and custom transformations using formulas.

In the following chapter, you will learn how to use the data exploration tool known as a TCRM lens, and you will understand how lenses are the building blocks of TCRM dashboards.

## Questions

Here is a list of questions to test your knowledge:

- What is the difference between a lookup join, an inner join, and a full join?
- Give one real-world example of how to use a full join.
- Why would you need to connect data from external sources with Salesforce objects? Give one practical example.
- Explain how to combine aggregation and grouping using two data streams in one recipe.
- The Multiple Row Formula option must be selected to perform window functions. What are window functions, and when might you use them?
- What is a flatten transformation, and what is a use case for this?

In this chapter, we will cover the following topics:

- Lenses – the investigative journalists of TCRM
- Creating, editing, and cloning lenses
- Using lenses to create a TCRM analytics dashboard
- Tips and hints

## Technical requirements

You will need the following to successfully execute the instructions in this chapter:

- A laptop or desktop with internet access (a tablet or phone is not sufficient)
- The latest version of the Google Chrome browser (Chrome is the preferred browser when working with TCRM)
- A working email address

Ensure that you're logged into your TCRM dev org.

## Lenses – the investigative journalists of TCRM

What exactly is a lens? Think of a lens as your data exploration tool in Tableau CRM, combining a data query with a visualization in a simple, powerful, and dynamic interface.

A lens enables you to perform the following tasks:

- Explore the various characteristics of your dataset, including dimensions, measures, field labels, and names.
- Slice and dice data using various measures, dimensions, filters, and formulas.
- Create visualizations and tables that can be clipped to a dashboard.
- Create visualizations and tables that can be shared with other users.

In the next section, you are going to get your hands dirty in TCRM and work with lenses.

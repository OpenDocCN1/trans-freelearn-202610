
- rank—Returns the rank of each record in the window based on order, while repeating the rank when the values are the same, and skips as many on the next non-match
- row_number—Returns the row number in the window, incrementing by 1 for every row in the window
- sum—Returns the cumulative sum from groups of value

An example of a window function would be to rank the deals per salesperson by the deal amount, as seen in the following screenshot:

![](img/06379e43783b9ddfee45e9da5192efe1_138_0.png)

## Additional functions

Two additional functions are available—the case function and the coalesce function.

### The case function

The case function handles if/then/else logic for formulas. This function can be used with TEXT, NUMBER, DATE_ONLY, and DATETIME data. The syntax for the case function is shown here:

```
case primary_expr(optional)
when condition
then result_expr
...
```
```
```text
else
default_expr
end
```
```

For example, you could use the case function to handle null values where missing values are replaced with the word Null. The case function can also be used to bucket data, although this is often better handled using the standard bucket function.

### The coalesce function

The coalesce function can do the following:

- Join two or more things or groups or dimensions to form a larger entity.
- Replace nulls with the first non-null value in the expression.

This function can be used with TEXT, NUMBER, DATE_ONLY, and DATETIME data. The syntax is shown here:

```
```text
coalesce(expr1,expr2)
```
```

For example, you can use the coalesce function to take two datasets or two streams from the same dataset (usually by filtering), and merge them into one.

> **Common Functions**
> 
> Common functions have their own buttons in the **Transform** toolbar—when used, these will automatically fill in the formula with the column name and predefined syntax to save time.

## Working with dates in the Data Prep tool

One of the more challenging areas of data preparation and transformation in TCRM has historically been working with dates. As the engineering team at Salesforce has continued to develop the new Data Prep ETL tool of TCRM, this is now easier than ever before.

Date fields are the key to understanding trends over time. Dates are critical for keeping teams aware of upcoming milestones and deadlines. You can now use **Date and Time** transformations in TCRM Data Prep to perform calculations based on dates, all in the GUI with *no code*.

Two common ways in which you may need to work with dates in TCRM is to add or subtract days or months and calculate a date difference.

## A deeper understanding of how and why to flatten your data

One of the most common use cases for flattening your data is to prepare data for use in security predicates—that is, data concerning role hierarchy is used to decide who sees what in a dataset or dashboard, based upon hierarchy data pertaining to the logged-in user.

See *Chapter 7, Security in Tableau CRM*, for more information about security predicates.

You will now learn about the hierarchy data type and how it can be used with a flatten transformation to prepare data for TCRM security purposes.

### Role hierarchy data type in Salesforce

The role hierarchy data type is associated with the standard Salesforce **User** object. It relates users to each other in a different hierarchy as an alternative to the standard manager user field.

The characteristics of the role hierarchy data type are outlined here:

- It is only allowed on the **User** object.
- It is a specialized version of the lookup relationship, in that it automatically self-references back to the **User** object, and it is similar to the standard lookup in that you can only create up to 40.
- In the user **interface (UI)**, a hierarchical field includes a dropdown of user types (for example, **User**, **Partner User**, **Customer Portal User**) that limits the lookup field to those users.
- Hierarchical fields support filters.
- Users selected in the hierarchy cannot be any users below that user in the hierarchy or the same user on the current record.
- It is supported in approval processes. Salesforce allows us to select from the standard user manager field or any hierarchical fields when configuring an approval process and determining the next approver. Also, you can create a new hierarchical field on the fly while configuring the approval process, without having to build it in advance.

A role hierarchy is created and configured via Salesforce Setup, as you can see in the following example:

![](img/06379e43783b9ddfee45e9da5192efe1_143_0.png)

Figure 5.22 – Creating a role hierarchy in Setup

### Flatten transformation

Detailed instructions on how to use a flatten transformation were given in Chapter 4, Building Data Recipes. In this section, we will look at how to flatten data in preparation for use in security predicates. This will enable you to add row-level security to a dashboard and dataset with the help of role hierarchy data.

Salesforce has provided a very powerful feature in Flatten Transformation to configure the data security in a customer relationship management (CRM) system to be incorporated in native TCRM.

Consider the use-case scenario of access to opportunity records for a sales organization configured using their role hierarchy in their Salesforce CRM. Let's walk through this example. The role hierarchy of the org is depicted here:

![](img/06379e43783b9ddfee45e9da5192efe1_144_0.png)

Figure 5.23 – Role hierarchy of the org in this example

In the preceding diagram, we have three sales reps, where **Sales Rep 1** and **Sales Rep 2** report to the **Regional Manager 1** role, and **Sales Rep 3** reports to the **Regional Manager 2** role. Access in this scenario works as follows:

- Opportunity records owned by **Sales Rep 1** and **Sales Rep 2** are accessible by **Regional Manager 1** and the roles shown in the preceding diagram.
- Opportunity records owned by **Sales Rep 3** are accessible to **Regional Manager 2** and the roles shown in the preceding diagram.
- **Regional Manager 2** should not be able to access the opportunities of **Sales Rep 1** and **Sales Rep 2** unless shared manually or by a sharing rule.
- **Regional Manager 1** should not be able to access the opportunities of **Sales Rep 3** unless shared manually or by a sharing rule.

The flatten transformation builds the table seen in the following screenshot to enable configuration of these access levels in TCRM:

| ROLE ID (SELF ID) | ROLE NAME | PARENT ROLE ID | ROLES | ROLEPATH |
|---|---|---|---|---|
| 1 | Salesperson 1 | 10 | 10, 20, 30 | \\10\20\30 |
| 2 | Salesperson 2 | 10 | 10, 20, 30 | \\10\20\30 |
| 3 | Salesperson 3 | 11 | 11, 20, 30 | \\11\20\30 |
| 10 | Regional Manager 1 | 20 | 20, 30 | \\20\30 |
| 11 | Regional Manager 2 | 20 | 20, 30 | \\20\30 |
| 20 | Vice President 1 | 30 | 30 | \\30 |
| 21 | Vice President 2 | 30 | 30 | \\30 |
| 30 | CEO | Not applicable | Not applicable | Not applicable |

Figure 5.24 – Flatten transformation table

How is this facilitated by the **Flatten Transformation** node? The `UserRole` object in Salesforce holds all the information on role hierarchy, and this object is used in **Flatten Transformation** as follows:

1. Select **Add Input Data** in the **Recipe**.
2. Select the `UserRole` object.
3. Add a **Transformation** node.
4. Select the **Role ID** column.
5. Select **Flatten**.

The **Transformation** node looks like this:

![](img/06379e43783b9ddfee45e9da5192efe1_145_0.png)

Figure 5.25 – The Transformation node in TCRM, prior to configuration

Now, let's look at the options to configure **Flatten Transformation**, as follows:

![](img/06379e43783b9ddfee45e9da5192efe1_146_0.png)

Figure 5.26 – Options to configure Flatten Transformation

Including the child node option will enable the role ID of the row to be part of the role ID path. The following screenshot presents a **Flatten** table with the **Include child node** option enabled:

| ROLE ID (SELF ID) | ROLE NAME | PARENT ROLE ID | ROLES | ROLEPATH |
|---|---|---|---|---|
| 1 | Salesperson 1 | 10 | 1, 10, 20, 30 | \1\10\20\30 |
| 2 | Salesperson 2 | 10 | 2,10, 20, 30 | \2\10\20\30 |
| 3 | Salesperson 3 | 11 | 3,11, 20, 30 | \3\11\20\30 |
| 10 | Regional Manager 1 | 20 | 10,20, 30 | \10\20\30 |
| 11 | Regional Manager 2 | 20 | 11,20, 30 | \11\20\30 |
| 20 | Vice President 1 | 30 | 20,30 | \20\30 |
| 21 | Vice President 2 | 30 | 21,30 | \21\30 |
| 30 | CEO | Not applicable | 30 | 30 |

Figure 5.27 – Flatten table with the Include child node option enabled

**Flatten Transformation**, once configured, adds the following two fields to the dataset:

- **Role ID Nodes**
- **Role ID Path**

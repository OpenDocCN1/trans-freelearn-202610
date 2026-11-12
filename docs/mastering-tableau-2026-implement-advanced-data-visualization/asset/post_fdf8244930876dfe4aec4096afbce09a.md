
To begin understanding data densification, four terms should be defined: data densification, sparse data, domain completion, and domain padding. In addition to the definitions, each term will be discussed in detail by using examples to help improve understanding:

- **Data densification**: Displays datapoints for which there is no corresponding underlying data (by using, for example, domain completion or domain padding)
- **Sparse data**: A dataset that is missing multiple datapoints (think Swiss cheese)
- **Domain completion**: Making sparse data dense — domain completion is a data densification method that uses table calculations to fill gaps, available in Tableau when two or more dimensions are in the view
- **Domain padding**: Adding datapoints to a sparse dataset — domain padding is a data densification method, available through the Show Missing Values feature in range pills (e.g., date and bin dimensions)

The definitions should be clear now; let's continue with hands-on exercises.

## Domain completion

There are two types of data densification: domain completion and domain padding. Domain completion is the more complex of the two and can be deployed cleverly to solve sparse data issues, but may also appear unexpectedly and prove a challenge to address.

Grasping domain completion requires a good understanding of **dimensions** and **measures** — discrete and continuous — and **partitioning** and **addressing** within table calculations. If you need a refresher on those topics, please review *Chapter 1, Reviewing the Basics* (for **dimensions** and **measures**), and *Chapter 5, Introducing Table Calculations* (for **partitioning** and **addressing**) before you continue to the next exercise.

Now, let's consider how domain completion can be deployed, when it's helpful, and when it can be a problem.

## Deploying domain completion

Domain completion can be activated in numerous and sometimes surprising and confusing ways. Adjusting the arrangement of pills on shelves, toggling dimensions between discrete and continuous, switching view types on the Marks card, adjusting partitioning, addressing, and other changes can impact domain completion activation. Although examples for every activation possibility will not be covered in this book, a review of typical domain completion scenarios should prove helpful.

## Activating domain completion in a crosstab

The following steps will guide you through an example of domain completion. We are going to use the OData endpoint as described in the *Using the OData connector section:*

- 1. Connect to the OData connector from the Chicago Data Portal as described in the *Using the OData connector section.*
- 2. Open a new worksheet and call it **DC - Crosstab**. This will automatically create an extract, which takes a bit since you are loading more than 8 million rows.
- 3. Once the extract was created, ensure that **Analysis | Table Layout | Show Empty Rows** and **Show Empty Columns** are both deselected.
- 4. In the Chicago Crime dataset, create a calculated field named **Index** with the code `INDEX()`.
- 5. Add a **Location Description** filter to view only a few locations. This will speed up rendering — for example, all the ones beginning with **AIRPORT**. Then, place **Location Description** on the **Rows** shelf.
- 6. Place **Year** on the **Columns** shelf. Change it to a discrete value. Note that, as shown in the following screenshot, the view is sparsely populated:

![](img/66cf6b89349fda3581a4f196cb3c98a4_258_0.png)

- 7. Place **Index** on the **Detail** shelf on the **Marks** card. Note that in the following screenshot, the view now reflects domain completion — that is, the view is fully populated:

![](img/66cf6b89349fda3581a4f196cb3c98a4_259_0.png)

Figure 6.6: Crosstab II

- 8. Click on Year and select Continuous. Note that data densification is deactivated:

![](img/66cf6b89349fda3581a4f196cb3c98a4_259_1.png)

Figure 6.7: Crosstab III

- 9. Reset Year to Discrete and then click on Index and select Edit Table Calculation.
- 10. In the resulting dialog box, select **Specific Dimensions** and then observe the results for each of the following selections:

| Select Specific Dimension | Data densification activated/deactivated |
|---|---|
| **Location Description** | Activated |
| **Year** | Activated |
| **Location Description and Year** | Deactivated |
| **No selection** | Deactivated |

### Table 6.1: Dialog box

The preceding exercise illustrates the following rule for deploying domain completion:

> *Given a crosstab with discrete dimensions on the Rows and Columns shelves, utilizing a table calculation (in this case, the Index field) in which at least one dimension (but not all dimensions) is addressed activates domain completion.*

A key term in the preceding rule may have been confusing: addressed. Partitioning and addressing were covered in Chapter 5, Introducing Table Calculations, but will be defined again here to ensure understanding of this rule. Consider the following information from the Tableau documentation (https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations.htm):

> *The dimensions that define how to group the calculation, that is, that define the scope of the data it is performed on, are called partitioning fields. The table calculation is performed separately within each partition. The remaining dimensions, upon which the table calculation is performed, are called addressing fields, and determine the direction of the calculation.*

When editing a table calculation, you can choose to select/deselect specific dimensions. When a dimension is selected, that dimension is used to address the table calculation. When a dimension is not selected, the dimension is used to partition the table calculation. The following screenshot of a Table Calculation editing dialog box demonstrates addressing on Location Description and partitioning on Year.

![](img/66cf6b89349fda3581a4f196cb3c98a4_261_0.png)

Figure 6.8: Table Calculation

### The usefulness of domain completion

Domain completion can be useful in many circumstances — for example, instead of filling in missing values manually, whenever you want to use a parameter, or when performing explanatory analysis. In fact, you may have gleaned some uses from the previous exercises even though they were designed merely for illustration purposes. The following exercise demonstrates using domain completion to display no data for cells without a value in a sparsely populated view.

#### Labeling nulls

Let us look at the following steps to begin the exercise:

- 1. Create a new worksheet, DC — Labeling Nulls.
- 2. Place Year on Columns and Location Description on Rows, and filter to all Airport records. Place SUM(Ward) and AGG(No Data) on Text in the Marks shelf. Adjust the duplicated worksheet so that the view type is set to Text. Also, ensure that only Location Description and Year are deployed on the view. Be sure to leave Location Description on the Filters shelf so that a few locations are displayed.
- 3. Create a calculated field named **No Data** with the following code:

```
IF ISNULL(COUNT([Case Number])) THEN 'No Data' ELSE 'Data' END
```

- 4. Place **Ward** and **No Data** on the **Text** shelf. Note that the **No Data** text does not display:

![](img/66cf6b89349fda3581a4f196cb3c98a4_262_0.png)

Figure 6.9: Labelling nulls

- 5. Place **Index** on the **Detail** shelf. Note that the **No Data** text does display. The domain completion portion of the exercise is now complete, but consider making the visualization more appealing by utilizing a shape:

![](img/66cf6b89349fda3581a4f196cb3c98a4_263_0.png)

Figure 6.10: Labelling nulls, with additional formatting

Congratulations — you know the ins and outs of domain completion now. Next, we will talk about domain padding: a concept that is most relevant when working with dates and bins.

## Domain padding

The second type of data densification is known as **domain padding**. We will now consider how domain padding is deployed and when it’s useful.

## Deploying domain padding

You may recall that one of the ways to deploy domain completion is through **Analysis | Table Layout | Show Empty Rows/Columns**. The same is true of domain padding, as illustrated in the following exercise. This exercise demonstrates how to toggle domain padding on and off:

- 1. Open a new worksheet called **DP - Show Missing Values**. Click on the **Show Start Page** Tableau icon at the top-left corner. Connect to the Sample Superstore dataset that ships with Tableau.
- 2. Place **Ship Mode** and **State/Province** on the **Rows** shelf. Next, place **Region** on the **Filters** shelf and select **West**. Lastly, put **SUM(Sales)** on the **Text** shelf. Note that, for example, **Wyoming** only appears for **Standard Class** shipping mode:

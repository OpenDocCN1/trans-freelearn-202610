
+   5. After committing this step, all columns in the **Product** table are immediately renamed.

The following image shows the results:

![](img/5d2bce74fd6fceebed14fd1488bf7088_172_0.png)

Figure 3.47: Renaming all columns at once

+   6. The last step is to rename the new step to something more meaningful. To do so, right-click the step and click **Rename** from the context menu, and type in a new name, as shown in the following image:

## Recursive functions

A recursive function is when we invoke a function within the function itself. **Factorial** is a mathematical calculation that multiplies all positive whole numbers from any chosen number down to 1. In mathematics, an exclamation mark shows a factorial calculation (for example, $n!$). The following formula shows the mathematical calculation of **Factorial**:

$n! = n * (n - 1)!$

As the preceding calculation suggests, a **Factorial** calculation is a recursive calculation. In a **Factorial** calculation, we can choose a positive integer (an integer larger than 0) when 0 is an exception; if 0 is chosen, then the result is 1. Here are some examples to make the **Factorial** calculation clearer:

```
10! = 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1 = 3,628,800
5! = 5 * 4 * 3 * 2 * 1 = 120
1! = 1
0! = 1
```

We need to use an @ operator to reference the function within itself. For example, the following function calculates the factorial of a numeric input value:

```
let
    Factorial =
        (ValidNumber as number) as number =>
            if ValidNumber < 0
                then error "Negative numbers are not allowed to calculate Factorial. Please select a positive number."
                else
                    if ValidNumber = 0
                        then 1
                        else ValidNumber * @Factorial(ValidNumber - 1)
in
    Factorial
```

We return 1 if the input is 0; otherwise, we calculate the **Factorial** recursively.

Did you notice the `error` part of the code? We can use the `error` keyword to raise a custom error message. So in the preceding code, we check if the input value is invalid, then we raise a custom error message. The following image shows the result of invoking a **Factorial** function with an invalid number:

![](img/5d2bce74fd6fceebed14fd1488bf7088_175_0.png)

Figure 3.49: Raising an error when invoking a Factorial function with an invalid value

There are cases when we do not want to raise any error messages even if the input is not valid. Instead, we want to show a constant value. The following code shows another version of the `Factorial` function checking if the input value is a positive value, then it calculates the factorial, otherwise, it returns -1:

```
let
    Factorial =
        (ValidNumber as number) as number =>
            if Number.Sign(ValidNumber) = 1
            then
                if ValidNumber = 0
                then 1
                else ValidNumber * @Factorial(ValidNumber - 1)
            else -1
in
    Factorial
```

The following image shows the result of invoking a Factorial function to calculate 10!:

![](img/5d2bce74fd6fceebed14fd1488bf7088_176_0.png)

Figure 3.50: The result of invoking a Factorial function to calculate 10!

## Summary

This chapter introduced different aspects of the Power Query M formula language and looked at how we can use Power Query Editor. We also looked at real-world scenarios and challenges that can directly affect our productivity and learned how to manage our data preparation step more efficiently.

The next chapter discusses getting data from various data sources, connection modes, and how they affect our data modeling.

## Join us on Discord!

Join The Big Data and Analytics Community on the Packt Discord Server!

Hang out with 558 other members and enjoy free voice and text chat.

https://packt.link/ips2H

## Getting Data from Various Sources

In previous chapters, we discussed some aspects of data modeling, including various layers in Power BI, how the data flows between different layers, virtual tables in Data Analysis eXpressions (DAX), and how they relate to data modeling. We also discussed leveraging query parameters and creating custom functions in the Power Query Editor with the Power Query formula language.

This chapter discusses getting data from various sources and describes some known challenges and common pitfalls in getting data from some sources. We also discuss data source certification and different connection modes to various data sources.

In this chapter, you will learn about the following topics:

- Getting data from common data sources
- Understanding data source certification
- Working with connection modes
- Working with storage modes
- Understanding dataset storage modes

### Getting data from common data sources

With Power BI, we can connect to many different data sources. In this section, we look at some common data sources that we can use in Power BI. We also look at common pitfalls when connecting to those data sources. But before we start, let us take a moment to discuss a common misunderstanding among many Power BI developers and users on what get

### Right-clicking Parameter1 and then clicking Transform Data

Dealing with data stored in tables is usually more straightforward than dealing with sheets.

+   13. Click the Sales_Data table.
+   14. Click OK.

The preceding steps are highlighted in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_183_0.png)

Figure 4.6: Navigating the Excel files to be combined

The preceding steps result in the creation of four new queries in the Power Query Editor, as shown in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_184_0.png)

Figure 4.7: Queries and folders automatically created to combine the files

Let us look at the new four queries automatically generated by the Power Query Editor:

- **Parameter1**: A **Binary** query parameter used in the **Transform File** custom function and the **Transform Sample File** query.
- **Sample File**: This is our original query with one more step in navigating to the first Excel workbook.
- **Transform File**: A custom function accepting **Parameter1** as an input parameter. Then, based on our choice in *step 13*, it navigates through the Excel file, reading the data from a sheet or a table (in our case, it is a table).
- **Transform Sample File**: A sample query to open the Excel file using **Parameter1**. The sample query is disabled to load into the data model.

In some cases, we need to keep some files’ metadata, such as **Date created** (which you can see in *Figure 4.8*). The **Attribute** column is a structured column of records keeping more metadata. We leave this to you to investigate further. But for this scenario, we would like to keep the **Date created** column for future reference. We now need to modify an automatically created step as follows:

+   1. Click the gear icon on the right side of the **Removed Other Columns1** step.
+   2. Tick the **Date created** column.

## CSV/Text/TSV

While there is a dedicated connector for CSV and TXT files in Power BI, no specific connectors support tab-separated values (TSV) files. This section looks at these three file types using an example scenario.

In the previous sections of this chapter, we managed to import sales data stored in a folder. Now, the business receives some data dumps stored in various formats. We have Product data in CSV format, Product Subcategory data in TSV format, and Product Category data in .txt format. We need to import the data from the files into Power BI. We use the Chapter 4, Get Data From Various Sources.pbix sample file:

https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/ea3530461d68ff6b08af69d1acdbc4c26204a63c/Samples/Chapter%2004/Chapter%204,%20Get%20Data%20From%20Various%20Sources.pbix

The following image shows the files to import to Power BI:

![](img/5d2bce74fd6fceebed14fd1488bf7088_186_0.png)

Figure 4.10: The CSV, TXT, and TSV files to be imported to Power BI

We start by getting data from the Product Category file, as follows:

+   1. In Power Query Editor, click the New Source drop-down button.

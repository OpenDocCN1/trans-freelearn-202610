
Now that we are sure that the RLS role works as expected, we publish it to the Power BI Service. If we have the correct RLS security settings in the service, the users must see only their sales data in the report. The following image shows a couple of examples of this:

![](img/5d2bce74fd6fceebed14fd1488bf7088_645_0.png)

Figure 11.22: Dynamic RLS automatically kicks in when users open the report

As shown in Figure 11.22, when Allan opens the report he sees his sales data, and Amy sees her sales data only.

Now that we know how dynamic RLS works, let’s take another step further and look at a more complex scenario. The next section explains dynamic RLS on a sample organizational chart.

## Managers can access their team members’ data in parent-child hierarchies

So far, we have implemented a straightforward scenario with dynamic RLS to enable salespersons to see the sales data that’s relevant to them. But there are many more complex scenarios. Suppose the business started using the report and everyone is happy apart from the sales managers. Their feedback is that they can only see their data, not any of their team members’. Moreover, the Chief Executive Officer (CEO) wants to see everyone’s sales. The business requires dynamic RLS implementation in which:

- Every salesperson can see their sales data.
- Sales managers can also see their team members’ sales.
- The CEO can see everyone’s sales.

To implement the preceding scenario, we need to create a parent-child hierarchy based on an existing organizational chart. The Employee table in the data model contains the required data to implement the preceding scenario. The EmployeeKey and ParentEmployeeKey columns contain the supporting data to create a parent-child hierarchy, and the EmailAddress column contains the users’ UPN supporting the dynamic RLS.

We discussed the parent-child hierarchy implementation in Chapter 10, Advanced Data Modeling Techniques.

We create a calculated column with the following DAX expression to identify the parent-child hierarchy path:

```
EmployeePath = PATH(Employee[EmployeeKey], Employee[ParentEmployeeKey])
```

The results of the preceding code snippet are shown in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_646_0.png)

Figure 11.23: Creating the EmployeePath calculated column in the Employee table

The next step is to identify the **EmployeeKey** value based on the **EmailAddress**.

The following DAX expression caters to that:

```
CALCULATETABLE(
    VALUES(Employee[EmployeeKey])
, FILTER(Employee, Employee[EmailAddress] =
USERPRINCIPALNAME())
, FILTER(Employee, Employee[Status] = "Current")
)
```

> **Note**
>
> In the **Employee** table, the **StartDate**, **EndDate**, and **Status** columns keep the employees’ employment history. Therefore, to get the current employees, we get the **EmployeeKey** values when the **Status** is **Current**.

To test the preceding code, create a **measure** using the same DAX expression, then validate the RLS role. The following image shows the result of using the same expression in a measure shown in a card visual:

![](img/5d2bce74fd6fceebed14fd1488bf7088_648_0.png)

Figure 11.24: Validating DAX expressions for RLS in a measure

We created the EmployeeKey measure to test the implementation, so we can remove it after testing. Now that we have retrieved the EmployeeKey value, we can use the results to find all rows where an EmployeeKey value appears in the EmployeePath calculated column. To understand the scenario, let’s look closely at the data. The following image shows all salespersons’ sales, their EmployeeKey values, names, email addresses, and EmployeePath values. Please note that the relevant values are marked with filled, dashed, and dotted lines:

![](img/5d2bce74fd6fceebed14fd1488bf7088_648_1.png)

Figure 11.25: Salespersons’ sales

As the preceding image shows, **Allan**, **Amy**, and **Syed** are sales managers. A sales manager is also a salesperson selling products. The **EmployeePath** column’s values reveal that the sales managers are in the third level of the organizational chart, as their employee keys appear in the third position within the **EmployeePath** values. Therefore, they must see all the sales data of their team members. The team members’ employee keys appear after the sales managers in the **EmployeePath**. Note that the person with an employee key of **277**, one level below the person with an employee key of **112**, must see all sales data. The employee key of 112 is the CEO’s. To implement this scenario, we must find each person’s employee key within the **EmployeePath** value for each row. To do so, we use a function in DAX to find values in a parent-child path: `PATHCONTAINS(<Path to lookup>, <value to be found within the Path>)`. The `PATHCONTAINS()` function returns `True` if the specified value appears within the path. We already have the path values within the **EmployeePath** column. We also wrote the DAX expression to retrieve the employee key of the current user, which is the value to be found within the path. So, the DAX expression looks like this:

```DAX
VAR _key = CALCULATETABLE (
    VALUES ( Employee[EmployeeKey] ),
    FILTER ( Employee, Employee[EmailAddress] = USERPRINCIPALNAME() ),
    FILTER ( Employee, Employee[Status] = "Current" )
)
RETURN
PATHCONTAINS (
    Employee[EmployeePath], _key
)
```

The only remaining part is to create an RLS role and use the preceding expression as a rule. The following image shows a new role, **Sales Team**, with the preceding DAX expression for its rule:

![](img/5d2bce74fd6fceebed14fd1488bf7088_650_0.png)

Figure 11.26: Creating a Sales Team role for dynamic RLS

Now, we validate the Sales Team role for one of the managers. The following image shows the validation results when Allan uses the report:

![](img/5d2bce74fd6fceebed14fd1488bf7088_650_1.png)

Figure 11.27: Validating dynamic RLS, enabling sales managers to see their team members’ sales

We can now publish the report to the service.

> **Note**
>
> If you are a Power BI administrator, assign the members to the new **Sales Team** role. Otherwise, ask your Power BI administrator to do so.

So far, we have implemented a dynamic RLS enabling the sales managers to see their team members’ sales. Now, let’s see what the CEO can see after opening the report. As a reminder, the CEO can see everyone’s sales data, as illustrated in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_651_0.png)

Figure 11.28: The CEO can see everyone’s data

While the preceding scenario was more complex than the first one, we may face more challenges in the real world. You must have noted that the **Employee** table contains the users’ login data to implement the previous scenarios, such as the **EmailAddress** value. In the next section, we learn how to implement scenarios when the email address data does not exist in the **Employee** table.

## Getting the user’s login data from another source

Imagine a scenario where the business has the same requirement as outlined previously; however, the source system does not contain the **EmailAddress** column in the **Employee** table. In those cases, we need to get the users’ login data from a different source. While the new data source provides users’ login data, it may not necessarily have an **EmployeeKey** column to relate employees to their login data. Depending on the source system, we may get different sets of data. In our scenario, we asked the system administrators to give us an extract of the organization’s AD users. They provided a **JavaScript Object Notation (JSON)** file containing a list of all users’ UPNs. You can find it here: https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/ca01add05cfd360fad97cbf304f66d9c80a8de4/Samples/Chapter%2011/Chapter%2011,%20Adventure%20Works,%20AAD%20UPNs.json.

> **Note**
>
> In the real world, we should think about automating the process of generating the JSON file and keeping it up to date.
>
> The generated JSON export from Azure AD contains sensitive data. Therefore, it must be stored in a secured location accessible by a restricted number of users.

For convenience, we use the same Power BI file used in the previous scenario. We just ignore the **EmailAddress** column in the **Employee** table.

The following steps explain how to overcome this challenge:

1. In Power BI Desktop, get the data from the provided **JSON** file, as illustrated in the following image:

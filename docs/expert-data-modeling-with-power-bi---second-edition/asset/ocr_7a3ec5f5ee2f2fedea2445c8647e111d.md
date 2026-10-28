# Figure 11.23: Creating the EmployeePath calculated column in the Employee table

The next step is to identify the **EmployeeKey** value based on the **EmailAddress**.

The following DAX expression retrieves caters to that:

```
CALCULATETABLE(
    VALUES(Employee[EmployeeKey])
, FILTER(Employee, Employee[EmailAddress] =
USERPRINCIPALNAME())
, FILTER(Employee, Employee[Status] = "Current")
)
```

> Note
> 
> In the **Employee** table, the **StartDate**, **EndDate**, and **Status** columns keep the employees’ employment history. Therefore, to get the current employees, we get the **EmployeeKey** values when the **Status** is **Current**.

To test the preceding code, create a **measure** using the same DAX expression, then validate the RLS role. The following image shows the result of using the same expression in a measure shown in a card visual:
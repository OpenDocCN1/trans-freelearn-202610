Figure 11.37: Validating the new RLS role

So far, we have learned how to implement various RLS scenarios. We also learned that sometimes we need to change the data model to meet some security requirements. While we covered some common scenarios, there are many more scenarios that we cannot cover in a single chapter, so we leave it to you to investigate more. The next sections explain OLS implementation in Power BI.

# Introduction to OLS

In the previous sections, we learned how to control the user’s access to data using RLS. In this section, we look at OLS in Power BI. With OLS, we can hide tables and columns that contain sensitive data from the model, such as hiding an entire table or columns for specific users. A more real-world example could be hiding people’s salaries, their bank accounts, or any other personal data from the Employees table in an HR data model. OLS also secures the metadata. Like RLS, OLS kicks in only in the Power BI Service for the users with the Workspace Viewer role and the users with read or build permissions on the dataset. So, sensitive objects are hidden from them, even though the users with a build permission on the dataset can create new reports or use the Analyse in Excel feature to connect to the dataset.

![](bbox=[0.178, 0.084, 0.822, 0.34])
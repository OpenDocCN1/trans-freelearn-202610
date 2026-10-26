# See also

In this recipe we saw how the SuggestText web control works and how it is used to prompt for a text value. To keep the recipe simple it did not include the necessary code to load and save the value to the database. For a quick review of adding fields to the database and routines to load and save values, see the recipe Connecting a module to the database in Chapter 4.

# Showing data in a Treeview

A Treeview control is used when you have information arranged in a hierarchy like the organizational chart of a company. The Treeview control will show each level in the hierarchy with an icon to drill-down or expand to the next level.

# Getting ready

To follow along with this recipe you must have completed the following recipes:

+   - Deploying a module as a standalone package (from Chapter 5)
+   - Displaying a Datagrid with filter controls
+   - Adding web controls to your Toolbox

The Treeview control requires that the employees in the Employee table have been assigned to managers. Here is the data that was used in this recipe:

| ItemID | EmpFirstName | EmpLastName | ManagerNo | HireDate | Salary | CreatedByUser | CreatedDate |
|---|---|---|---|---|---|---|---|
| 1 | John | Smith | 0 | 2/4/2010 12:00:... | 25000 | 1 | 2/4/2010 9:33:0... |
| 13 | Richard | Roe | 0 | 1/1/1995 12:00:... | 35000 | 1 | 2/11/2010 7:25:2... |
| 14 | Susan | Smith | 0 | 2/2/1997 12:00:... | 55000 | 1 | 2/11/2010 12:00:... |
| 23 | Kevin | Jones | 1 | 3/3/1998 12:00:... | 65000 | 1 | 2/11/2010 12:00:... |
| 24 | Amy | Roe | 1 | 4/4/1999 12:00:... | 90000 | 1 | 2/12/2010 8:27:... |
| 25 | Marianne | Brown | 13 | 5/5/1992 12:00:... | 30000 | 1 | 2/12/2010 8:28:... |
| 26 | Debby | Smith | 14 | 6/6/1996 12:00:... | 90000 | 1 | 2/12/2010 8:27:... |
| NULL | NULL | NULL | NULL | NULL | NULL | NULL | NULL |

In this example, the employees are arranged in a simple two-level hierarchy. If your ManagerNo is 0 it means you are a manager. Otherwise the manager number on your record is the ItemID of your manager. So John Smith is a manager (his ManagerNo=0) and he manages Kevin Jones and Amy Roe (as their ManagerNo is John Smith's ItemID).
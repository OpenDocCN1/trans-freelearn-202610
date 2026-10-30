
Consider these requirements for each value type:

| Value Type | Requirements | Predicate Expression Examples |
|---|---|---|
| string literal | Enclose in double quotes and escape the double quotes. | - `'Owner' == "Amber"`<br>- `'Stage Name' == "Closed Won"` |
| number literal | Can be a float or long datatype. Do not enclose in quotes. | - `'Expected Revenue' >= 2000.00`<br>- `'NetLoss' < -10000` |
| field value | When referencing a field from the User object, use the `$User[field]` syntax. Use the API name for the field.<br><br>You can specify standard or custom fields of type string, number, or multivalue picklist.<br><br>When you define a predicate for a dataset, you must have read access on all User object fields used to create the predicate expression. | - `'Owner.Role' == "$User.UserRoleId"`<br>- `'GroupID' == "$User.UserGroupId__c"`<br><br>Note: Supported User object field value types are string, number, and multivalue picklist. |

Figure 7.6 – Values in a predicate expression

If a user tries to query the dataset that a non-valid predicate is applied to, an error will appear for the user.

Now, go into Analytics Studio in your TCRM development organization and follow these steps:

1. Open **My First Analytics App** and view your list of datasets.
2. Edit the **Opportunities** dataset by clicking on the drop-down arrow on the right-hand side of the list.
3. Delete the existing security predicate (as seen in the code snippet following this bulleted list) by clicking on the pencil icon (as shown in the preceding table) and deleting the predicate code.
4. **Security predicate** should now be **None**.
5. Edit the predicate and add **New opportunity dataset predicate**.
6. Create a lens for the **Opportunities** dataset and check that the new predicate works as you expect. You may need to log out and then log back in for the predicates to take effect.
7. Undo your work by editing the dataset and replacing the new predicate with the previous one.

The pencil icon that you need to click to edit the predicate is highlighted with a bold arrow in the following screenshot:

![](img/06379e43783b9ddfee45e9da5192efe1_200_0.png)

Figure 7.7 – Opportunities dataset showing the existing security predicate

Here is the existing opportunity dataset predicate:

```
'Owner.Role.Roles' == "$User.UserRoleId" || 'OwnerId' == "$User.Id" || 'Account.OwnerId' == "$User.Id"
```

This predicate ensures that opportunity data is only available for viewing where it is permitted based upon the role hierarchy, opportunity ownership, and account ownership.

Here is the new opportunity dataset predicate:

```
'OwnerId' == "$User.Id" || 'Account.OwnerId' == "$User.Id"
```

This predicate has removed access based upon the role hierarchy.

Now, go back and replace the new predicate with the original one.

For a highly detailed reference on predicates, see the *Tableau CRM Security Implementation Guide* here:

```
https://resources.docs.salesforce.com/232/latest/en-us/sfdc/pdf/bi_admin_guide_security.pdf
```

## Summary

The following diagram provides a useful summary of how security works in Tableau CRM:

![](img/06379e43783b9ddfee45e9da5192efe1_201_0.png)

Figure 7.8 – TCRM security review

As you have seen, you have a variety of options for implementing security in Tableau CRM. In this chapter, you learned about these security tools and how to deploy them.

What have you learned? First, you grasped the basics of security in TCRM. Next, you learned how to govern TCRM access permissions. Third, you learned how to use app-level security to control access to TCRM analytics app assets. Next, you became competent in configuring and enabling Salesforce sharing inheritance. Lastly, you learned how to build and deploy security predicates for more complex, detailed data governance use cases.

In the next chapter, you will learn to build your very first TCRM dashboard.

## Questions

Here are some questions to test your knowledge of this chapter:

- What is the purpose of app-level security in TCRM?
- How can you control TCRM security from Salesforce objects and field access?
- When a dataflow job runs, TCRM uses the permissions of ____________ user to extract data from Salesforce objects and fields.
- What are three limitations of Salesforce sharing inheritance?
- What are three use case scenarios for security predicates?
- A security predicate expression must not exceed how many characters?
- Is this a valid predicate expression? Why, or why not?
  - `"Revenue">100`
- A security predicate expression is case-sensitive – true or false?

## Section 3: How to Build Awesome Analytics Dashboards in TCRM

In this section, you will learn how to design, build, test, and deploy powerful and useful dynamic analytics dashboards within TCRM.

This section comprises the following chapters:

- *Chapter 8, Building Your First TCRM Dashboard*
- *Chapter 9, Advanced Dashboard Design and Build*
- *Chapter 10, To Code, or Not to Code?*
- *Chapter 11, Best Practices in Dashboard Design Using TCRM*

## 8 Building Your First TCRM Dashboard

It's time to put your new understanding to use and build your first Tableau CRM (TCRM) dashboard. By the end of this chapter, you will be able to create a dashboard from scratch, navigate through the dashboard editor, add and edit widgets, add pages, slice and dice data, and share your findings with others.

We will cover the following main topics in this chapter:

- Getting to know your dashboard editor
- Creating a dashboard and placing widgets
- The devil is in the details
- Understanding the top-down design with pages
- A user's guide to a TCRM dashboard

### Technical requirements

You will need the following to successfully execute the instructions in this chapter:

- The latest version of the Google Chrome browser (Chrome is the preferred browser when working with TCRM)
- A working email address

Be sure to be logged in to your TCRM dev org.

Time to jump in and build your first TCRM dashboard!

### Getting to know your dashboard editor

In this section, you will get hands-on and learn how to use the dashboard editor in TCRM.

From your Analytics Studio home page, click on Create and select Dashboard, then click on Create Blank Dashboard. You will come to the dashboard editor canvas, which looks like this:

![](img/06379e43783b9ddfee45e9da5192efe1_206_0.png)

Figure 8.1 – The initial dashboard editor canvas

Before proceeding with the next steps, save your dashboard as My First Dashboard in My First Analytics App.

Now, configure the dashboard settings by clicking on the **Default** gear icon. I typically use these default settings:

- **Name:** `Default`
- **Columns:** 50
- **Row Height:** Fine
- **Cell Spacing:** 8 (horizontal) and 0 (vertical)
- **Maximum Dashboard Width:** 1500
- **Background Color:** Leave as-is for now

There are 14 tools available on the left-hand side of the canvas, as detailed next; these will be explained as you add them to your dashboard:

- Chart
- Table
- Filter
- Container
- Component
- Date
- Link
- Image
- List
- Number
- Range
- Text
- Toggle
- Navigation

When you click on the drop-down arrow at the top right of the editor, you have these options:

- **Clone in New Tab:** Creates a clone of the current dashboard in a new tab.
- **Dashboard Properties:** General options and widget default properties.
- **Pick Initial Values**: Set the default filter and facet values for the dashboard; these will apply every time the dashboard is opened.
- **Conversations**: Lets you query your data using conversational, everyday language. This needs to be enabled and the correct permissions assigned for the user to see and use this functionality.
- **Connect Data Sources**: Join data sources using a common field.
- **Delete**: Permanently delete the dashboard.

It's now time to flesh out your dashboard.

### Creating a dashboard and placing widgets

The best way to show you how to build a dashboard and help you become familiar with the dashboard editor is to walk you through a simple dashboard build.

### Creating a sales dashboard

We will create a sales dashboard based upon the **Opportunities** dataset in **My First Analytics App**. Follow these steps:

1. Create a fresh query by clicking on the **Create Query** button on the right-hand side of the canvas.
2. Select the **Opportunities** dataset in **My First Analytics App**. This brings you to a view that is familiar to you as being almost identical to a lens.
3. At the top left, where you see a pencil icon, rename the query **Pipeline by Close Date**.
4. Change the measure (**Bar Length**) to **Sum of Amount**.
5. Add a grouping (**Bar**) for **Close Date (Year-Month)**.
6. On the far right, click on the Charts icon and change the chart type to **Time Bar** in the **DATES & TIMES** category.
7. Add a filter for **Closed** equal to the value of **False** (as in, open opportunities).
8. Click **Done**, which will close the window.

   The query now appears in the query list on the right, under the **Opportunities** dataset.

9. Drag the query onto the canvas. This creates a chart widget.
10. Enlarge the widget by dragging the handles.
11. Click **Save** to save your dashboard.

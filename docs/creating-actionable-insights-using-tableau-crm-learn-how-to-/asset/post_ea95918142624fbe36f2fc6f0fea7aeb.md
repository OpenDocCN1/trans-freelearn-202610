
Figure 12.3 – Configured record action dialog box – part two

- 13. Close and reopen the My First Dashboard dashboard. Click on the drop-down arrow on the opportunity name for any record in the table. You should see these actions:

### Opportunity record table

| Opportunity Name | Opportunity Owner | Amount |
|---|---|---|
| Opportunity for Conner5 | Laura Palmer | $32,400 |
| Opportunity for McDonald13 | Dennis Howard | $240,747 |
| Opportunity for Jefferson17 | Irene McCoy | $21,640 |
| Opportunity for McLaughlin130 |  | $1,249,000 |
| Opportunity for Chandler133 |  | $754,640 |
| Opportunity for Rice134 |  | $243,450 |
| Opportunity for Barnes141 |  | $363,400 |
| Opportunity for Edwards146 |  | $4,212,140 |
| Opportunity for Williams149 |  | $2,305,550 |
| Opportunity for Wagner150 |  | $2,660,660 |
| Opportunity for Lewis258 | Laura Garza | $309,938 |
| Opportunity for Barrett259 | Doroth Gardner | $1,589,440 |
| Opportunity for Dennis260 | Bruce Kennedy | $2,868,400 |

Figure 12.4 – Configured record actions on the Opportunity Name field

Follow these steps to create and edit a follow-up task for a deal that requires attention:

- 1. Click on **New Task**.
- 2. Under **Subject**, select **Call**.
- 3. Enter a **Due Date** value.
- 4. Leave the **Name** field blank.
- 5. Note that the task is prefilled for the relevant opportunity under **Related To**.
- 6. Leave **Assigned To** and **Status** as they are.
- 7. Click on **Save**.
- 8. Click on **Open Record** for the opportunity and see how you are taken to the record in Salesforce.
- 9. Under the **Activity** tab for the opportunity in Salesforce, you will see the task that you just created. Click on it to be taken to the task record page.
- 10. Edit the task by changing the priority and adding comments.
- 11. Click on **Save**.

You did it! You created a task from a TCRM dashboard and edited that task in Salesforce with just a few clicks and no copying and pasting information!

Here is the task on the opportunity record:

![](img/06379e43783b9ddfee45e9da5192efe1_336_0.png)

Here is the end result:

![](img/06379e43783b9ddfee45e9da5192efe1_337_0.png)

*Figure 12.6 – The final task that was created and edited via TCRM*

Keep in mind that record actions in TCRM support these objects and global actions:

- Create and update object records.
- Log a call.
- Custom actions to trigger Lightning components, flows, and Visualforce pages.
- Lightning components that use Lightning events are supported in embedded dashboards but are not supported in Analytics Studio.
- Standard Chatter actions, excluding *File*.

Global quick actions and canvas apps are not supported in TCRM configured record actions. TCRM does not support actions assigned to a page layout for the user object.

As you saw briefly in *Chapter 10, To Code, or Not to Code*, TCRM also offers the advanced functionality of bulk actions, where a group of records can be actioned in Salesforce with just a few clicks. However, as this is an advanced developer feature, it will not be covered in this book.

Let's now understand how to trigger an automation in Salesforce from TCRM.

## Why would you embed your dashboards?

One of the great struggles in the world of data analytics and business intelligence is *user adoption*. It is often the case that organizations spend hundreds of hours and many thousands of dollars on capturing, analyzing, and visualizing, only for the project to fail miserably because business users simply don’t use and apply the insights.

One way to address poor user adoption is to embed the insights in front of the user in the CRM workflow, where they will inevitably view the dashboards as they work in their **Business As Usual (BAU)** environment. This is very different, and much more effective, than asking users to remember to review dashboards during their busy workdays, even if they are only a tab away in Salesforce. It really is a game changer for analytics adoption.

Here is an example that we created for one of our clients in the **Not-For-Profit (NFP)** space. In this use case, the NFP organization wanted insights about their donors added to the Salesforce home page and account record page. Once this was achieved, the usage of TCRM insights increased drastically, and users were able to action insights as a part of their daily BAU activity in Salesforce.

Here is the NFP home page with an embedded TCRM dashboard showcasing donor segmentation insights:

![](img/06379e43783b9ddfee45e9da5192efe1_339_0.png)

Figure 12.8 – The NFP home page with an embedded TCRM dashboard on the right

Here is the account record with rich insights added from TCRM:

![](img/06379e43783b9ddfee45e9da5192efe1_340_0.png)

Figure 12.9 – The NFP account record page with an embedded TCRM dashboard

Is it difficult to embed TCRM dashboards in Salesforce? Do you need to be a developer? The answer to both questions is no. Let me show you how to embed your dashboards.

## How do you embed your dashboards?

The best way to learn how to embed dashboards is to do it, so follow these steps to embed a simple dashboard on the account record page in Salesforce:

- 1. Create a new blank dashboard and save it as `Mini Sales Insights` in My First Analytics App.
- 2. Create and add a chart for sales by close date month.
- 3. Create and add a chart for the pipeline by close date month.
- 4. Save your dashboard.

The end result should look like this:

![](img/06379e43783b9ddfee45e9da5192efe1_341_0.png)

Figure 12.10 – The Mini Sales Insights dashboard

Now, let's go ahead and embed this dashboard on the account record page in Salesforce. Follow these steps:

- 1. Open any account record in Salesforce.
- 2. Click on the Setup gear icon at the top right of the page.
- 3. Click on Edit Page. You are now in the Lightning App Builder.
- 4. From the list of components on the left, select the Tableau CRM Dashboard component and drag it onto the canvas below the highlights panel.
- 5. Go into the component format panel on the right and make sure that Dashboard shows as Mini Sales Insights.
- 6. Adjust the Height value for the component so that the TCRM dashboard fits nicely on the page layout. For my example, this was a height of 400.
- 7. Leave the other options as they are.
- 8. The dashboard is ready, except that it will show information for all accounts, and we want it to show only information for the account record being displayed. To add a filter that will accomplish this, click on Add Dashboard Filter.
- 9. Select Opportunities under Dataset.
- 10. Select Account ID under Dataset Field.
- 11. Select Equals as Operator.
- 12. Select Account > Id under Object Field.
- 13. Click on OK.
- 14. Click on Save.
- 15. Activate the account record page and click on Assign as Org Default.
- 16. Go back to the account page in Salesforce (the left arrow in the top-left corner in the Lightning App Builder).

![](img/06379e43783b9ddfee45e9da5192efe1_342_0.png)

The format panel for the TCRM dashboard Lightning component should look like this:

Figure 12.11 – Completed format panel for the TCRM dashboard Lightning component

It is also worth noting that the visibility of the TCRM component can be controlled from this format panel, where filters can be added that control whether or not the embedded dashboard is visible to the logged-in user. These filters are based upon record fields or the device being used.

I trust that this book has been a help to you, wherever you may be on your TCRM journey. I encourage you to dive into the platform and use it in real-world use cases, because the best way to learn is to solve problems, build assets, and deliver solutions. Make sure that you get involved in the vibrant TCRM online community, and avail yourself of the many great online resources available for TCRM.

Please feel free to reach out to me via LinkedIn with any feedback or questions about my book or TCRM in general. I would love to hear from you!

## Questions

Answer the following questions to test your learning from this chapter:

- How do embedded dashboards aid user adoption of TCRM?
- Give three examples of configured record actions supported in TCRM.
- What is the purpose of the dashboard filter when embedding a TCRM dashboard via the Lightning App Builder?
- Give one example of how you can drive an automation in Salesforce from TCRM.

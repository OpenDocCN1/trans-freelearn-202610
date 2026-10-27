Compare this with the following screenshot. When **Grouping 1: Opportunity Owner** is selected, **Opportunity Owner** becomes disabled in the parent grouping level:

![](bbox=[0.1, 0.18, 0.9, 0.58])

+   5. Let's select **Grouping 2: Type** for this exercise and select **Opportunity Owner** as the parent grouping level from the radio button.
+   6. Click on the **< Insert** button, and you will get the formula added in our sample here: **PARENTGROUPVAL(summary_field, FULL_NAME)**.
+   7. Delete the **summary_field** text and change it with the correct field summary. To populate the **summary_field** parameter, place your cursor before the comma, click on **Summary Fields**, select the field name you would like to use (for example, **Amount**), and select the action (for example, **Sum**). Now your formula should be *PARENTGROUPVAL(AMOUNT:SUM, FULL_NAME)*.
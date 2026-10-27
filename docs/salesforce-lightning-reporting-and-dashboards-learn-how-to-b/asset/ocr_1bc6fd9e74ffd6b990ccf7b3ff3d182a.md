# Working with report filters

A report is one of the most powerful features of the Salesforce platform; depending on your permissions, you can build your own reports within minutes and manage them without help from Salesforce administrator or Salesforce developer.

We have discussed many things related to the report in previous chapters, including the permissions to create a report, permissions related to the report folder, and the report format that presents the report layout.

Another important item related to reporting is **report filters**. Report filters allow you to define which data conditions you want to use in order to include records in a report based on some criteria. The filter criteria in the report are almost similar to the filter criteria we found in other Salesforce features, such as List Views, Workflow Rules, and other areas in Salesforce, but the filter criteria in the report are even more powerful.

For each filter criterion, you need to set the field, operator, and value. You can have multiple filters for a report. The default relationship for each filter is the one using AND, but you can customize it using filter logic.

When you are in the report builder, if you click on the **Add** button for Filters, it will automatically add a new filter, but note that there is a small arrow button next to **Add**; if you click on this arrow, a few options available for the report filter are as follows:

+   - **Field Filter**: This is the same action as clicking on the **Add** button
+   - **Filter Logic**
+   - **Cross Filter**
+   - **Row Limit**: Only for the **Tabular** report format

![](bbox=[0.272, 0.613, 0.728, 0.825])
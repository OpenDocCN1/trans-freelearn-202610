![](bbox=[0.17, 0.09, 0.83, 0.25])

Figure 4.33: Getting data from Excel hosted in SharePoint Online with a direct link

The following list shows the benefits of this method over the first method:

+   - It requires fewer clicks to get the direct link
+   - The direct link does not have any query parts, so we do not need to modify the link
+   - The whole process is easier to remember

The next section explains connecting to Power BI datasets.

# Power BI datasets

Considering the increasing demand for team working and collaboration, using Power BI datasets is becoming more and more popular. When we create a data model in Power BI Desktop and publish it to a Power BI Service workspace, the underlying transformation pipelines in Power Query and the data model turn into a dataset. Then, we make the datasets available across the organization. This is an efficient method of collaboration in large Power BI projects. The data modelers create the models and publish them to the service, and the report writers make thin reports on top of the available shared datasets.

Note
Shared Power BI datasets are not available in the Power BI free licensing plan. The reason is that accessing Workspaces requires a Pro license;
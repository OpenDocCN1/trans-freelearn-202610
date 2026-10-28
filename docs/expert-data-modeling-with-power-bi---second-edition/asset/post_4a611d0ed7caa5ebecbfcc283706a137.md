
![](img/5d2bce74fd6fceebed14fd1488bf7088_710_0.png)

Figure 13.6: Power Query - Choose data source when creating a new Dataflow

If we select an on-premises data source, we need to select an up and running **on-premises Data Gateway**. Installing and configuring the on-premises Data Gateway is out of the scope of this book, but if you are keen to learn more about it, here is a good resource: [https://learn.microsoft.com/en-us/data-integration/gateway/service-gateway-onprem?WT.mc_id=DP-MVP-5003466](https://learn.microsoft.com/en-us/data-integration/gateway/service-gateway-onprem?WT.mc_id=DP-MVP-5003466).

We have now navigated to the **Power Query** form, similar to the **Power Query Editor** window in Power BI Desktop. One point to notice in the **Power Query** form is the **warnings** section. As the following image shows, if there are any issues in loading the data from some columns, a warning message shows up in the **warnings** section. We can click on the **warnings** section to see more details:

![](img/5d2bce74fd6fceebed14fd1488bf7088_711_0.png)

Figure 13.7: Warning messages after getting data from the source

As the preceding image shows, we can click the warning message to take action if necessary. After we have finished all the required transformation steps, we can save the Dataflow by clicking the **Save & close** button.

The next step is to give our Dataflow a name and save it, as shown in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_712_0.png)

Figure 13.8: Saving the Dataflow

Unlike the Power Query Editor in Power BI Desktop, which loads the data from the source immediately after we click the **Close & Apply** button, Dataflows will not automatically load data from the source. Instead, we have two options: to refresh the data or schedule a data refresh, as shown in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_712_1.png)

Figure 13.9: Data refresh options after creating a new Dataflow

We can populate the tables by clicking the **Refresh now** button or by setting a schedule to refresh the tables automatically.

## Create linked tables from other Dataflows

We can create linked tables (referencing tables from other Dataflows) in a new Dataflow or add a linked table to an existing Dataflow. The following steps show how to add linked tables as a new Dataflow:

1. After navigating to the desired workspace and starting to create your Dataflow, click the Add linked tables button, as illustrated in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_713_0.png)

Figure 13.10: Adding linked tables as a new Dataflow

2. Go through Connection settings.

3. Click Next.

The preceding steps are highlighted in the following image:

Figure 13.11: Connection settings to create linked tables

4. In the Power Query form, expand a workspace.
5. Select any desired Dataflows.
6. Click the desired tables.
7. Click the Transform data button.

The preceding steps are highlighted in the following image:

Figure 13.12: Selecting tables from another Dataflow to link

8. Click **Save & close**, as illustrated in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_715_0.png)

Figure 13.13: Save & close changes for selected linked tables

9. Type in a name and then click the **Save** button, as illustrated in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_715_1.png)

Figure 13.14: Saving linked tables as a new Dataflow

So far, we have learned how to create a new Dataflow with linked tables. In the next subsection, we learn how to create computed tables.

## Create computed entities

In this section, we learn how to create computed entities. The most common way to create a computed entity is within an existing Dataflow by referencing another entity. The following steps show how to create a computed entity after navigating to the desired workspace and opening a Dataflow:

1. Click the **Edit tables** button, as illustrated in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_716_0.png)

Figure 13.15: Editing entities in a Dataflow

2. Right-click the desired table from the **Queries** pane.

3. Click **Reference**.

The preceding steps are highlighted in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_717_0.png)

Figure 13.16: Creating a computed table

We created a computed table, as shown in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_717_1.png)

Figure 13.17: Computed table

We can also create a computed table from a linked table by referencing it or adding some transformation steps. The following image shows that the **Date** table is a linked table, and the **Date Comp** table is a computed table referencing it:

![](img/5d2bce74fd6fceebed14fd1488bf7088_718_0.png)

Figure 13.18: Creating a computed table from a linked table

Remember that the linked tables are read-only, and we are not allowed to modify them. If we do so, the Power Query Online engine forces us to roll back the changes.

## Configure incremental data refresh in Dataflows

As previously mentioned, one of the use cases for creating Dataflows is to prevent business analysts from accessing the source data. If the source table is large, then we can configure incremental data refresh. We already explained what an incremental refresh is and how to configure it in Power BI Desktop in Chapter 10, Advanced Data Modeling Techniques, in the Incremental refresh and hybrid tables section; therefore, we do not explain it again. But, there is a minor difference between configuring incremental refresh in the Power BI Desktop and Dataflows. When configuring the incremental refresh in Power BI Desktop, we must first create two query parameters: RangeStart and RangeEnd. We then use those parameters when defining the incremental refresh policies. But in Dataflows, we must not define the query parameters at all. The engine does it automatically for us.

Let us configure incremental data refresh on the **Orders** table from the **Northwind Sample Data** Dataflow we created earlier. Follow these steps:

1. Navigate to the desired Workspace.
2. Open the Northwind Sample Data Dataflow.
3. Click the **Incremental refresh** (🔄) button of the **Orders** table under the **ACTIONS** section.
4. Toggle the incremental refresh on.
5. Select the **OrderDate** column from the **Choose a DateTime column to filter by** dropdown.
6. Put **30 Years** in the **Store rows from the past** section.
7. Put **1 Month** in the **Refresh rows from the past** section.
8. Click **Save**.

The following image shows the preceding steps:

![](img/5d2bce74fd6fceebed14fd1488bf7088_719_0.png)

Figure 13.19: Incremental refresh on Dataflows

After saving the changes, edit the Dataflows to see the changes. The following image shows that the **RangeStart** and **RangeEnd** parameters are created, and the **Orders** table is filtered on the **OrderDate** column using the parameters automatically.

![](img/5d2bce74fd6fceebed14fd1488bf7088_720_0.png)

Figure 13.20: Configuring incremental refresh on Dataflows creates query parameters and adds an automated filter step on the corresponding table

Now that we implemented the Dataflow, let us export its source code for future use.

## Export/import Dataflows

It is always a good idea to have a copy of the source code while developing any software solutions and developing Dataflows is not an exception. We can export the Dataflows definitions into JSON format and import them later if required. Please note that exporting and importing are for the Dataflow definitions only, not the data. Let us see how they work.

### Export Dataflows

Navigate to the desired workspace, then proceed as follows:

1. Hover over a Dataflow and click the **More options** ellipsis button.

2. From the menu, click **Export.json**.

3. A message shows up when the file is ready.

4. Depending on your browser, the **JavaScript Object Notation (JSON)** file downloads automatically to your local machine.

The preceding steps are highlighted in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_721_0.png)

Figure 13.21: Exporting Dataflow definitions in JSON format

### Import Dataflows

The following steps show how to import a Dataflow after navigating to the desired workspace:

1. Click the **New** button.

2. Click **Dataflow**.

3. Click the **Import model** option.

4. Select an exported Dataflow definition file (**JSON**).

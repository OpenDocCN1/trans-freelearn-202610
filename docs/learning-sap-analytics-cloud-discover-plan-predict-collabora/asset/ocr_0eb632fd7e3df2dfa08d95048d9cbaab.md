# Join types

When you select a secondary model and add a measure from it to the chart, you see an additional section in the Builder panel. This section is labeled JOIN TYPE, as illustrated in the following screenshot. The options provided in this section allow you to select the type of data you want to display on the chart:

![](bbox=[0.31, 0.22, 0.7, 0.41])

All primary data: This is the default option. It displays all of the data in the primary model and the corresponding data in the secondary model. This means that all the events data from the Planned_Events model are displayed, whether or not they have associated records in the BestRun_Demo model.

All data: As you can see, this option is grayed out because it is available only for the linked models whose data sources are SAP HANA. When this condition is met, the option becomes accessible and fetches all data from the primary and secondary models.

Intersecting data only: This option will allow only the linked data to appear in the chart. Refer to the next section, where this join type is demonstrated using an example.

When you select any of these options from the list, the icon beside the secondary model (A) changes to indicate the type of join that is currently in effect.
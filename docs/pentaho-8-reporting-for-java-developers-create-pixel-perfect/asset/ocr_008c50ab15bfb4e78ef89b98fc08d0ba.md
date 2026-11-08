Looking at the Data tab in the report explorer, the default data source is called `design-time-data` and it's defined as the `Table` type. This specific data source is automatically defined for the index element (and the TOC element too, with few differences in the fields) to be used in the index definition. The following screenshot shows how the data source looks:

![](bbox=[0.378, 0.215, 0.622, 0.335])

Use the `item-data` and `item-pages` fields to generate the index data field name and page numbers.

To build your own index, drag the fields and drop them in the report bands, exactly in the same way as you learnt in the previous chapters. In our case, we would like to also add a title using a `label` element; the index layout is defined as shown in the following screenshot:

![](bbox=[0.148, 0.515, 0.852, 0.595])

Now that the master-report and the index are defined, let's preview it by pressing the eye icon (👁) in the report canvas. In the following screenshot, you can see the final result of the page number 97, where the index starts. As you can see, customer called AV Stores, Co. is quoted in pages 4, 60, 61, 69, and 70, the customer called Alpha Cognac is quoted in pages 10, 24, and 87, and so on.
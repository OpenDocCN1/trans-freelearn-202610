The final option, Get the latest data in real time with DirectQuery (Premium only) is, as the name implies, a feature only available for Premium (Fabric) capacities and Premium Per User (PPU) workspaces. Selecting this option adds a DirectQuery partition to the semantic model. In short, data that is changed after the last incremental refresh is retrieved by this partition. Including a DirectQuery partition can significantly reduce the number of refreshes per day and ensure that the latest changes to the data are included in reports.

# See also

+   - *Incremental refresh in Power BI:* https://bit.ly/3f3DSkM
+   - *Incremental refresh and real-time data for semantic models:* https://bit.ly/3w10oSv

# Summary

This chapter’s recipes contained semantic modeling, DAX measure, and M query patterns to enhance the performance, scalability, and reliability of Power BI semantic models. This included performance tuning examples of both semantic models and measures, error handling, and query folding examples of M queries, in addition to supporting details on the DAX and M query engines.

The next chapter is all about deploying and distributing Power BI content using the Power BI Service, Teams, and mobile devices.

# Join our community on Discord

Join our community’s Discord space for discussions with the author and other readers:

https://discord.gg/ebXZqyrxJb

![](bbox=[0.08, 0.64, 0.22, 0.75])
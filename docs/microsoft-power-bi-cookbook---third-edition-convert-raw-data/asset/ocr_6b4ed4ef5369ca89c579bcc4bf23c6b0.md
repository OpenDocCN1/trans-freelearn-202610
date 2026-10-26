| BUSINESS PROCESSES | Row Granularity | Measures | Date | Customer | Product | Promotion | Reseller | Sales Territory | Employee | Account | Organization | Department |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Internet Sales | Sales Order Line (SKU) | Internet Sales Dollars and Units, Count of Customers and Products | ✓ | ✓ | ✓ | ✓ |  | ✓ |  |  |  | |
| Reseller Sales | Sales Order Line (SKU) | Gross and Net Sales, Discounts, Margin Amount and % | ✓ |  | ✓ | ✓ | ✓ | ✓ |  |  |  | |
| General Ledger | GL Account Entity | Account Balance Amount | ✓ |  |  |  |  |  |  | ✓ | ✓ | ✓ |

*Figure 3.2: Implementation bus matrix: Internet Sales, Reseller Sales, and General Ledger*

Power BI queries (M) and analysis logic (DAX) should not be considered long-term substitutes for issues with data quality, master data management, and the data warehouse. If it is necessary to move forward, document the “technical debt” incurred and consider long-term solutions such as **Master Data Services (MDS)**.

3. **Choose the semantic model storage mode—import, DirectQuery, or composite:** With the logical design of a model in place, one of the top design questions is whether to implement this model with DirectQuery mode, the default imported In-Memory mode, or a combination of both import and DirectQuery—a composite model.

The default import mode is highly optimized for query performance and supports additional modeling and development flexibility with DAX functions. With compression, columnar storage, parallel query plans, and other techniques, an import mode model is able to support a large amount of data (in the range of 50 million rows) and still perform well with complex analysis expressions. Multiple data sources can be accessed and integrated into a single semantic model and all DAX functions are supported for measures, columns, and role security. The chief disadvantages of import mode are semantic model size limitations (1 GB for Pro, 10 GB for Premium) and the fact that data refreshes must be scheduled (eight times per day for Pro, 48 per day for Premium). An alternative to scheduled refreshes in the Power BI service is the use of REST APIs to trigger a data refresh of a published semantic model. For example, an HTTP request to a Power BI REST API calling for the refresh of a semantic model can be added to the end of a nightly update or ETL process script such that published Power BI content remains aligned with the source systems.

A DirectQuery mode model provides the same semantic layer interface for users and contains the same metadata that drives model behaviors as Import mode models do. The performance of DirectQuery models, however, is dependent on the source system and how this data is presented to the model. By eliminating the import or refresh process, DirectQuery provides a means to expose reports and dashboards to source data as the data changes.

This also avoids the file size limit of import mode models. However, there are several limitations and restrictions to be aware of with DirectQuery:

+   - Only a single source from a single, supported data source can be used in a DirectQuery model.
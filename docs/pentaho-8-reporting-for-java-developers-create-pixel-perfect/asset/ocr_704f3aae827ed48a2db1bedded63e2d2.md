# Data source reference

A single reference to the report's data source is specified in the `datadefinition.xml` file, as a `data-definition/data-source` XML element. If multiple data sources are used in a single report, you must use a compound data source that combines different data sources. The `data-source` XML element defines the following attributes:

| Name | Description |
|------|-------------|
| `report-query` | The named query used by the master report |
| `limit` | If specified, defines the limit of the number of rows returned by the primary query of the report |
| `timeout` | If specified, defines the query timeout for the primary query of the report |
| `ref` | A reference to the specified data source definition file |
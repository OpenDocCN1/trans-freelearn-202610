# Query properties

This group of properties is provided for Master Report only and contains the properties for query management:

| Name | Type | Description |
|------|------|-------------|
| query | String | Assigns a name to the selected query. If you are using a JDBC Custom data source, use the name field. |
| limit | Integer | Limit for the query execution and rows retrieve. The value -1 means no limit. |
| timeout | Integer | Timeout limit for the query. |
| design-timeout | Integer | Timeout limit when running from Report Designer. |
| auto-sort | Boolean | Flag indicating if the auto sort is requested. |
| shared-connection | Boolean | Flag indicating if the connection is shared to sub-reports. |
| data-cache | Boolean | Determines whether parameter result sets are cached, which would reduce the amount of reload time when switching parameters in a rendered report. This option can be further configured through the org.pentaho.reporting.engine.classic.core.cache.InMemoryCache.CachableRowLimit and org.pentaho.reporting.engine.classic.core.cache.InMemoryCache.MaxEntries engine settings. By |
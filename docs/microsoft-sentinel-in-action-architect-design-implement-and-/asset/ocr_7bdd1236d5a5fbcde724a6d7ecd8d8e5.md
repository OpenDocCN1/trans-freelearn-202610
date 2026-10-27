The available choices are as follows:

| Name | Description |
|---|---|
| Favorites | The star icon determines whether this is a favorite, in which case it will be filled in, or not, and only the outline will show. Favorites will also appear first in the listing of queries. |
| Query | This is the title of the query and not the KQL query. |
| Provider | This is who provided the query. This will typically be either Microsoft or Custom Queries. |
| Created By | This is who wrote the query. It will either contain the name (or email) of the person who created the custom threat hunting query or be blank if it was provided by Microsoft. |
| Created Time | The date and time the query was created. |
| Entities | This will show which entities the query will provide. |
| Data Source | This will list the data source that this query requires. If there is more than one data source required, the first one will be listed along with a number telling you how many more data sources there are. You can mouse over the information icon, which looks like an i inside a circle, to see the other data sources. |
| Results | If the query has not yet been run, this will show two dashes to indicate that. If it has been run, it will either show the number of results returned or N/A if it was unable to run due to missing data sources. |
| Results delta (preview) | This will show the delta between the results from the current run of the queries and the results from the last run of the queries. |
| Tactics | This will show the MITRE ATT&CK® tactics that this query uses. |
| Techniques (preview) | This will show the MITRE techniques that this query uses. Note that these will only show up for Microsoft-provided queries, as there is no way to select these when creating your own queries. |
# Some tips for indexing:

| Category | Tips |
|---|---|
| Table column selection | - A column that can be easily indexed should be chosen.
- Numeric values are easier to index than strings and hence indexes on integer values are much faster.
- If an index needs to be created on a string field it should be fixed width and not `varchar`, etc.
- Never create indexes based on dates.
- Dynamics AX has an automatically generated `RecID` for most of the tables. Indexing on that column is advisable if another column does not qualify. |
| Numbered columns (to make composite key) to be included in index | - Try minimizing the use of composite keys. If no other single column qualifies, RecId can be a good choice for indexing.
- The number of columns in an index should be minimized so that a search criterion is as simple as possible.
- A maximum of three columns are recommended for a composite key. |
| Number of indexes | - There is no limit to the maximum number of indexes on a table. However, it is recommended that indexes should be limited to only those columns that are frequently used in the `where` clause of select queries.
- Too many indexes will slow the overall operation as one insert or update operation will result in many insert or update operations due to an insert or update being required in each index files beside table. |
| Index column size | The column or combination of columns on which an index is based is known as the key. The key size should be as small as possible to unleash the benefits of indexing in performance optimization. The key size can be reduced by:
- Using the minimum number of columns in the index
- Using the minimum data size of the column used as index key
- Using numeric keys instead of other data types such as varchar, strings, etc. |
| Distinct index keys | Index keys should be chosen so that the duplicated values for the key are minimal. The best case is that the key is unique. |
```
, HIER.name_path || '-' || ENT.Entity_Name::string as name_path
FROM DATA_ENG_DBT.PUBLIC.Entity as ENT
JOIN ENTITY_HIERARCHY as HIER ON HIER.Entity_KEY = ENT.Parent_KEY
)
SELECT * FROM ENTITY_HIERARCHY;
```

The following table shows the results of the query:

| E_KEY | P_KEY | ENTITY_NAME | LEVEL | PARENT_NAME | KEY_PATH | NAME_PATH |
|---|---|---|---|---|---|---|
| 1 |  | President | 1 | - | 1 | President |
| 2 | 1 | VP Sales | 2 | President | 1-2 | President-VP Sales |
| 3 | 1 | VP Tech | 2 | President | 1-3 | President-VP Tech |
| 4 | 2 | Sales Dir | 3 | VP Sales | 1-2-4 | President-VP Sales-Sales Dir |
| 5 | 3 | Lead Architect | 3 | VP Tech | 1-3-5 | President-VP Tech-Lead Architect |
| 6 | 3 | Software Dev Manager | 3 | VP Tech | 1-3-6 | President-VP Tech-Software Dev Manager |
| 7 | 6 | Proj Mgr | 4 | Software Dev Manager | 1-3-6-7 | President-VP Tech-Software Dev Manager-Proj Mgr |
| 8 | 6 | Software Eng | 4 | Software Dev Manager | 1-3-6-8 | President-VP Tech-Software Dev Manager-Software Eng |

Table 3.1: Table with the results from the recursive query

By analyzing the code of the recursive query, you can see that the CTE is made up of two parts: the anchor part before UNION ALL and the recursive part after it.

The anchor part uses a query to fetch the initial set of rows and set the initial values of the calculated fields.

The recursive query uses a join between the base table and the content of the previous recursive iteration, accessed by the name of the CTE, to retrieve the next layer of entities and update the calculated fields.
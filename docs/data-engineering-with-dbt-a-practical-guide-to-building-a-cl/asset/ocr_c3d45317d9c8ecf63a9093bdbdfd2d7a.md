We can try our source by writing the following query, which is the simplest for testing a source:

```
```sql
SELECT *
FROM {{ source('abc_bank', 'ABC_BANK_POSITION') }}
```
```

Note that the names in dbt are case-sensitive, so use the same casing as in the YAML file. This calls for an agreed upon code convention for table and source names inside your project.

By clicking the **Preview** button in the dbt IDE, you ask dbt to compile and run the query on your database, and then take you to the **Query Results** tab, where you can see the results of the query:

![](bbox=[0.134, 0.289, 0.853, 0.514])

Figure 5.7: Results of a query on a source in dbt's Preview tab

Going to the **Compiled SQL** tab, you can see the SQL that was executed on the database, as shown in the following image. Note that clicking the </> **Compile** button just compiles the code and brings you to the **Compiled SQL** tab, without executing it:

![](bbox=[0.134, 0.65, 0.853, 0.864])

Figure 5.8: Compiled code for the preceding query in dbt Compile SQL tab
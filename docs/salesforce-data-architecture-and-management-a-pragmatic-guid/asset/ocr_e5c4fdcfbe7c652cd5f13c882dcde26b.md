+   - **Data type** (for example, string, number): Data type of the custom field
+   - **IsIndexed**: Boolean value to indicate whether the field is indexed
+   - **FieldNum**: Position of the field in the object

Next, we will look at the data table.

# Data table

As seen in Figure 2.18, this table contains data for all custom fields that are defined for custom objects:

| GUID | OrgID | ObjID | ...other columns... | Val0 |
| :--- | :--- | :--- | :--- | :--- |
| a01...1 | Org1 | a01 | ... | Up |
| a01...2 | Org1 | a01 | ... | Flat |
| a02...1 | Org1 | a02 | ... | 20201201 |
| a02...2 | Org1 | a02 | ... | 20210101 |
| a03...1 | Org1 | a03 | ... | 91.23012 |
| a03...2 | Org1 | a03 | ... | -10.35 |

Figure 2.18 – Data table where custom field data resides

Notice that this table has a limit of 500 columns, which is also the limit for custom fields on a custom object.

Key fields include the following:

+   - **GUID**: Global unique identifier for each field
+   - **ObjID**: Unique identifier of the object
+   - **OrgID**: Organization that owns the custom field
+   - **Field name**: Natural name of the field

Another 300 fields are allowed from managed packages. These columns, called flex columns, can store data from multiple data types since, in the end, it all gets stored as a string data type. All fields in an object are stored in a different column and no two fields are stored in the same column. This table also has the four audit fields, which we will discuss later on in the chapter, and an IsDeleted column (not shown) to indicate when data has been deleted.
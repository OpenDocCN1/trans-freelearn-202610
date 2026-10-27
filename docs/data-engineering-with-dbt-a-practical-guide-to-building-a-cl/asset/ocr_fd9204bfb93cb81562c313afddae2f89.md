The data model shows also, in the thin line with a by ETL description, the two relationships that are created by adding to the fact the FK of the department and main lecturer dimensions:

```
SELECT
  f.*,
  c.Department_CODE,
  c.Main_Lecturer_CODE
FROM FACT_XYZ as f
LEFT OUTER JOIN REF_Course as c on f.Course_Code = c. Course_Code
```

The preceding code, which would be part of the FACT_XYZ model, shows how such enrichment can be done by following the join from the fact to the course and picking the FK for these two tables, which are in the REF Course table.

# The application of master data

The other common task that you need to consider in the REF layer is the master data mapping needed to convert data from the coding and conventions of the original source to the ones in use in the master data system, if there is one, or at least into a common system to be used for the specified data mart.

This is so common that we have already added this step, in the diagram of the data model of the normal transformation chain to build a REF model at the beginning of this story’s description.

# A definition of the composite fact

In most cases, the fact at the center of the star schema for your report is a composite fact that needs to be created from base facts and supporting tables, including mappings and many-to-many relation tables

It is important that during stories S1 and S2, you cover well how to build the composite fact, with S1 more focused on what the desired content is, and S2 more focused on how to build it and apply the correct BRs to provide correct values.

We cannot cover all the possible situations that you will face, but we want to point out that you should carefully consider the nature of the relationships of the facts and intermediate tables that you need to compose, as cases where you have many-to-many relationships are very difficult to deal with.

The solution for such cases can be specific BRs that reduce the complexity of the relationships and/or aggregate the different parts to such a level that the relationship becomes easier to handle.

# S3.x – developing with dbt models the pipeline for the XYZ table

Once the S1 and S2 stories have been completed or are at least in the advanced stage of completion, you will have a list of tables that you need to build in the refined and delivery layers.
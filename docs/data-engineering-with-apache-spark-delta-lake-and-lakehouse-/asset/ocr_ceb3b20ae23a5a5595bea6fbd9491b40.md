# The process of curating raw data

By this point, hopefully, it is easy to envision that the need for curating data is very real. Now, let's focus on the actual process to make this happen.

![](bbox=[0.22, 0.23, 0.75, 0.38])

# Inspecting data

The process of data curation starts by inspecting sample data. Typically, this is a joint effort between the data engineers and the customer team members. You can start by visually inspecting data covering diverse data sources, although in many cases you may need to implement programming logic to discover data that is unstandardized, invalid, inconsistent, non-uniform, duplicate, or insecure.

Deliverable: A detailed report listing all the instances where data curation will be required, including a plan to fix each case. Within the report, feel free to include the pseudocode for the business logic that addresses the specific case, as follows:

```
IF raw_data.country IN ('USA', 'United States', 'United States of America') THEN
    curated_data.country = 'USA'
```

# Getting approval

The report created during the inspection phase should be formally submitted to the customer team for review and approval. This ensures that the customer is on board with the business logic that will be implemented to curate the data.

Deliverable: Formal approval by the customer team.
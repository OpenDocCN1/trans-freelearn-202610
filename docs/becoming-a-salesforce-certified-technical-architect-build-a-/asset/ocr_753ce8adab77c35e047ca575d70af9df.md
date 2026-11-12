# Understanding What You Should Be Able to Do as a Salesforce Data Architect

A data architect is an experienced technology professional responsible for designing and describing a solution's overall data modeling concepts and determining policies, procedures, and considerations for data collection, organization, and migration. The data architect is the principal for designing a scalable, performant, and optimized data model that considers the impact of large data volume (LDV) objects. It is worth mentioning that LDV challenges are not only specific to the Salesforce Platform, although a Salesforce data architect should probably be more conscious of them.

Note According to Salesforce's online documentation, the CTA candidate should meet a specific set of objectives, all of which can be found at the following link: https://pactt.link/3QOIV.

Have a closer look at each of these objectives.

# Describing Platform Considerations, Their Impact, and Optimization Methods while Working with Large Data Volume Objects

Having LDV objects in your Salesforce implementation can lead to several performance considerations, such as slower queries, slower record creations, and record locks. A CTA should be aware of such challenges and anticipate the potential risk of an identified LDV on the overall solution. The CTA should accordingly prepare a mitigation strategy to address these risks and challenges.

First, you need to be able to identify an LDV. A simple mathematical method can be used to identify LDVs, where an object must meet at least one of the following conditions:

+   - Has more than 5 million records
+   - Has a growth rate that would lead to creating 5 million new records per year

In addition, some other indicators can point to an LDV use case, such as the following:

+   - Having an org with thousands of active users who can access the system concurrently
+   - The object has lookup/parent objects with related/child objects that have more than 10,000 records
+   - Having an org that uses over 100 GB of storage space

The preceding criteria could be a bit conservative. Salesforce could perform well with double that number of records. However, it is recommended to follow a safer approach and use the previously mentioned figures to identify LDV objects.
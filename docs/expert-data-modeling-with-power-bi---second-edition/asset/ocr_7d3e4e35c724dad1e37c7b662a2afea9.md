# Row-Level and Object-Level Security

In the previous chapter, we learned advanced data modeling techniques, such as implementing various aggregation types, incremental refresh, parent-child hierarchies, role-playing dimensions, and calculation groups. This chapter discusses an essential aspect of data modeling, **row-level security** and **object-level security (RLS and OLS respectively)**. We will cover the following topics:

+   - What RLS and OLS mean in data modeling
+   - Terminology
+   - Implementation flows
+   - Common RLS and OLS implementation approaches

We try to cover the preceding topics with real-world scenarios, but remember that each Power BI project may have specific requirements, so it is virtually impossible to cover all possibilities and scenarios in one chapter.

When it comes to Power BI security, many people immediately think it relates to Power BI administration, which is correct to some extent. RLS filters the data within an entire data model to show relevant data to the relevant users. On the other hand, OLS guarantees that only authorized users can access the model objects, such as tables and columns. So, both RLS and OLS are access control mechanisms directly applied to the data model, and their implementation is directly relevant to data modelers. Still, at the end of the day, we publish the data model into the Power BI Service or Power BI Report Server, where the users, depending on their roles, interact with the data model. So, the security settings tasks vary from organization to organization. In some organizations, an administrator’s job is to take care of the dataset security-related settings within the Power BI
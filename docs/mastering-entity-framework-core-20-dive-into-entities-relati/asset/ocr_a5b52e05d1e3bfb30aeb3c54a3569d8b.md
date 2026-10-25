# 8

# Query Is All We Need – Query Object Pattern

We have covered raw SQL query execution in Entity Framework (EF), which might be required in a few valid scenarios to have control over queries or to improve performance. We have seen a simple SQL query, the security threat it could pose, and also a solution for this. Then we covered different ways of executing raw SQL queries in Entity Framework. Finally, we created an extension method to execute raw SQL queries without a DBSet and POCO model.

Let's discuss the repository pattern currently used in the Object Relational Mapping (ORM) layer, looking at its design issues, and we'll see how the query object pattern helps us in achieving this efficiently. The following design issues will be addressed in this chapter:

+   - The repository pattern started to abstract the ORM layer, but lately, the methods included in the repository were growing drastically, which defeats its purpose:
+     - The necessity to simplify the data access layer led to the query object pattern
+   - We have been creating multiple methods in repositories that differ only by the nature of queries:
+     - The query object pattern generalizes and improvises the repository methods
+   - Queries alone would not be sufficient in simplifying the data access layer:
+     - Composing queries with generic commands extends the pattern to all CRUD operations
# Use Joins in Forms

The use of `join` in forms can offer distinct advantages in comparison to use of display methods that contains `select` queries. In one call all records can be fetched for a form showing approximately twenty grid lines. An additional advantage of using `join` is that it is very easy to sort, filter, and find on the joined fields.

# AOS Performance Optimization

The key principle behind AOS optimization is to minimize the number of calls between the client and the server. The amount of data transported in each call is secondary to the number of calls. A few of best practices are:

+   - Achieve select discount.
+   - Use containers to reduce the client or server calls.

# Achieve Select Discount

Whenever a select statement needs to be executed from a data source located at another tier, the returned record has certain minimum size, which means multiple records can be fetched in a single call and hence less round trip calls to AOS are needed for a given number of records. This is phenomenon known as 'Select Discount'.

If only one record needs to be fetched, the number of records can be minimized to the first record only.

# Using Field Groups in Tables

The field groups are an important feature in Dynamics AX design, which is not only used to synchronise forms and reports with the concerned tables (while adding, deleting, or modifying any field of a table), but also for many other purposes such as caching and performance optimization. The following are a few advantages of using field groups in a table:

+   - IntelliMorph can automatically update the layout of all related forms and reports whenever any change is made to the field groups.
+   - It is easy to upgrade new forms and reports that have identical field groups to the standard table i.e. the custom objects in standard field groups will also be updated in the upgraded version.
+   - The sequence of fields can be used to determine the sequence of fields in forms and reports and hence the same order of fields can be assured in all concerned forms and reports related to a table.
+   - Identical field groups on tables and forms help in AOS optimization.
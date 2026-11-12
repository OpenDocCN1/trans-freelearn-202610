# Relationships pivot table

This table stores relationships that have been defined on an object. When a field is created with one of the relation types discussed in an earlier section, the platform maps the field to a value in the data table and uses this field to store the Obj ID of a related object. The relationships table is shown here:

![](bbox=[0.2, 0.21, 0.79, 0.45])

Figure 2.20 – Relationships table

In the preceding diagram, notice how relationship type fields are stored in this table that refer to the data table we looked at earlier.

# FallbackIndex table

This is used if the external search engine fails to return a result. A fallback search is then performed. This is done by implementing a direct database query referencing the names of the target application objects. To enable this type of search, all object names are defined in this table, so the database has access to the most up-to-date information.

# NameDenorm table

This table stores the Obj ID and name of each field in the data table. This is primarily used to provide hyperlinks to records for objects that are in a parent-child relationship.

# HistoryTracking table

This table stores changes to fields that have been identified by the tenant for auditing. It stores the old and new values of the field along with when and who made the change.
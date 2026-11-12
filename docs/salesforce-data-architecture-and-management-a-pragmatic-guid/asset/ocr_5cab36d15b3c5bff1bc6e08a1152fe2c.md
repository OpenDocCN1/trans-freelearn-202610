As can be seen in the preceding diagram, all custom objects and custom fields are defined in tables and there are no physical database tables created. We next take a look at the key specialized tables that form this architecture.

# Objects metadata table

This table stores the custom objects that have been defined in the org. The key attributes of this table are as follows:

+   - ObjID: Unique identifier of the object
+   - OrgID: Unique identifier of the organization
+   - ObjName: Name of the object

# Fields metadata table

This table contains data pertaining to the custom fields that are defined for the custom object:

![](bbox=[0.126, 0.425, 0.844, 0.623])

Figure 2.17 – Fields metadata table

All custom field metadata is stored in the preceding table. Also notice the presence of OrgID in all tables to ensure data partitioning and security for each tenant.

Key fields include the following:

+   - FieldID: Unique identifier of the custom field
+   - ObjID: Unique identifier of the object
+   - OrgID: Organization that owns the custom field
+   - FieldName: Name of the custom field
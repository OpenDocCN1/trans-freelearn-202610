# Verify your knowledge

+   - What is a role?
+   
+     A role can be a single user or a group of users that have access to the cluster and its databases. A role is the basic unit to grant access and define permissions. See the *Understanding roles* section for more details.
+   - What does the INHERITS clause do?
+   
+     The INHERITS clause makes a role inherit, that is get, instantly and dynamically all the permissions granted to the role from which it inherits. Without the INHERITS clause, the role still has the permissions of the role it belongs to, but an explicit SET ROLE is required in order to use such permissions. See the *Roles that inherit from other roles* section for more details.
+   - What is an Access Control List (ACL)?
+   
+     An ACL is the specification of a set of permissions attached to a database object, and is the way PostgreSQL implements and store the permissions. See the *ACLs* section for more details.
+   - What are the statements to add a permission to a role or remove a permission from a role?
+   
+     The GRANT statement adds a permission to a role, while the REVOKE statement removes a permission from a role. The special keyword ALL can be used to grant or revoke all the available permissions for the object, while multiple permissions can be specified at once in both the commands. See the *Granting and revoking permissions* section for more details.
+   - What is Row-Level Security (RLS)?
+   
+     RLS is a way to restrict the result set of a query depending on the role that is executing it. RLS can be applied to both read queries and write queries. See the *RLS* section for more details.

# References

+   - CREATE ROLE statement official documentation: https://www.postgresql.org/docs/current/sql-createrole.html
+   - ALTER ROLE statement official documentation: https://www.postgresql.org/docs/current/sql-alterrole.html
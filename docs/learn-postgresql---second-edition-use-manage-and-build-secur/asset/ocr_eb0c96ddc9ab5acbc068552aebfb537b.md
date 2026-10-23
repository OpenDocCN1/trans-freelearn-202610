Yes, it’s possible using pg_trgm. See the *Exploring the pg_trgm extension* section for more details.

+   - What is point-in-time recovery (PITR)?
+   
+   Given retention, point-in-time recovery is the ability to restore to any point in the past. See the *Disaster recovery with pgbackrest* section for more details.

+   - Does PostgreSQL have a tool that can help us to manage continuous backups and point-in-time recovery?
+   
+   Yes, it has several tools that help us to manage continuous backups and PITR, one of which is called pgbackrest. See the *Disaster recovery with pgbackrest* section for more details.

+   - Is it possible to connect directly to another PostgreSQL server?
+   
+   Yes, it is, using the PostgreSQL foreign data wrapper extension. See the *Using foreign data wrappers and the postgres_fdw extension* section for more details.

# References

+   - Pg_trgm official documentation: https://www.postgresql.org/docs/current/pgtrgm.html
+   - Foreign data wrappers wiki page: https://wiki.postgresql.org/wiki/Foreign_data_wrappers
+   - PostgreSQL foreign data wrappers official documentation: https://www.postgresql.org/docs/current/postgres-fdw.html
+   - PgBackrest official documentation: https://pgbackrest.org
+   - PgLoader official documentation: https://pgloader.readthedocs.io
# Configuring PgAudit

PgAudit ships with a rich set of configuration parameters that allow you to specify exactly what to log, when, what to exclude from auditing, and so on. All configuration parameters live within the pgaudit namespace so that they will not clash with other existing settings with the same name.

The most important setting is pgaudit.log, which defines which statements and actions you want to audit. The parameter can assume any of the following values:

+   - ALL to audit every statement
+   - NONE to audit nothing at all
+   - READ to audit only SELECT and COPY statements
+   - WRITE to audit every statement that modifies data (INSERT, UPDATE, and COPY)
+   - ROLE to audit role changes or creation
+   - DDL to audit all the data-definition statements, and therefore any change to the database structure
+   - FUNCTION to audit all code execution, including DO blocks
+   - MISC to audit all the values not explicitly categorized above
+   - MISC_SET to audit all SET-like commands

You are free to specify more than one setting at the same time by separating single names with a comma, for example:

```
pgaudit.log = 'WRITE,FUNCTION';
```

This function can be used to audit all data changes and code executions.

Another important configuration parameter is pgaudit.log_level, which specifies the log level that PgAudit will use to make the auditing messages appear in the logs. By default, this setting assumes the value log, but you can change it to any other log threshold except error ones (such as ERROR, FATAL, and PANIC).

In order to insert more details in the audit information, you will likely want to enable pgaudit.log_parameter to dump any query parameters (you will see an example later).

If you are going to configure PgAudit by object, you need to set the pgaudit.role parameter as you will see later in this chapter.
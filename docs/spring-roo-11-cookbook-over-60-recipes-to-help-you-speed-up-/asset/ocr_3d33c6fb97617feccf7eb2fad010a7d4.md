3. Executing the `database introspect` command for the first time suggests the database driver that we need to download to perform the introspection of the `myflightappdb` database in MySQL. So, download the suggested driver using the `addon install` command, as shown here:

```
roo> addon install id --searchResultId 01
Target resource(s):
------------------
Spring Roo - Wrapping - mysql-connector-java (5.1.13.0001)
Deploying...done.

Successfully installed add-on: Spring Roo - Wrapping - mysql-connector-java [version: 5.1.13.0001]
```

4. Re-execute the `database introspect` command as shown here:

```
roo> database introspect --schema no-schema-required --file mydb.xml --enableViews
Database metadata written to file C:\roo-cookbook\ch03-recipes\mydb.xml
```

5. Now, open the generated `mydb.xml` file to view the database metadata.

# How it works...

The `database introspect` command makes use of the connection properties defined in the `database.properties` file of the Roo project to create metadata information. This implies that you can't simultaneously introspect multiple databases using the `database introspect` command. The command accepts the following arguments:

+   `schema` **(mandatory)**: It is the database schema which you want to introspect. If you want Roo to connect to the database and provide the list of schemas available, then simply press `TAB` after entering the `--schema` argument on the command line. As some of the databases such as MySQL and Firebird don't support the concept of schemas, pressing `TAB` after entering the `--schema` argument on the command line will result in substituting `no-schema-required` as the value of the `schema` argument.

+   `file` **(optional)**: It is the file to which you want to save the database metadata. If you don't specify this argument, the metadata is displayed directly in the Roo console.

+   `enableViews` **(optional)**: It is the flag that indicates whether to include database views in the generated metadata. By default database views are not included in the generated metadata.
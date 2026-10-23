# # Managing extensions

Every extension is managed at a database level, meaning that every database that needs an extension must manage such an extension life cycle. In other words, there is no per-cluster way of managing an extension and applying it to every database within the cluster.

Extensions are mainly managed by three SQL statements: CREATE EXTENSION, DROP EXTENSION, and ALTER EXTENSION, to respectively install an extension in a database, remove the extension from the database, and modify extension attributes or upgrade them.

Every extension is specified by a mnemonic and a version; if a version is not specified, PostgreSQL assumes you want to deal with the latest available version or the one that is already installed.

In the following subsections, each of the three management statements will be explained.

# ## Creating an extension

The CREATE EXTENSION statement allows you to install an existing extension in the current database.

The synopsis of the statement is as follows:

```
CREATE EXTENSION [ IF NOT EXISTS ] extension_name
    [ WITH ] [ SCHEMA schema_name ]
                [ VERSION version ]
                [ CASCADE ]
```

The extension name is the mnemonic for the extension, and as you can see, you can specify the version number of the extension to install. If the extension depends on any other extension, the CASCADE option allows the system to automatically execute a recursive CREATE EXTENSION for the dependency. You can decide which schema the extension objects must be placed into, and of course, that makes sense only for such extensions that can be relocated.

As you can imagine, IF NOT EXISTS allows the command to gracefully fail if the extension has been already installed. More precisely, it does nothing if the extension has already been installed in the database.

In order to better see how CREATE EXTENSION works, assume we want to install the PL/Perl procedural language in the forumdb database; since the PL/Perl extension is available as the PostgreSQL contrib module, you should have the extension already available within the cluster.
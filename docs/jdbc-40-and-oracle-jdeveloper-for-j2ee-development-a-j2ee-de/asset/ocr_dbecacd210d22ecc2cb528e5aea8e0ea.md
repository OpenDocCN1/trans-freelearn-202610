In the **Create Ant Buildfile** specify a **File Name**, `build.xml`, and a **Directory Name** for the build file. A `build.xml` file is added to the Hibernate project in the Applications-Navigator.

![](bbox=[0.175, 0.205, 0.825, 0.54])

The example `build.xml` consists of targets for the following:

+   1. Generate Java classes from the mapping file.
+   2. Compile the Java classes.
+   3. Generate a database table from the mapping file and the `hibernate.properties` file.

We will discuss the structure of the `build.xml` file in detail. Add the `<project/>` tag, which specifies the project name, the default target and the `basedir` attribute. Add build file properties, which correspond to Hibernate directories. The directories are specified relative to the base directory, which is specified in the `basedir` attribute of the `project` element. The properties in the example `build.xml` are listed in the following table.
The current primefaces dependency looks like the following:

```
<dependency>
    <groupId>org.primefaces</groupId>
    <artifactId>primefaces</artifactId>
    <version>8.0</version>
</dependency>
```

With the upgrade, it should look like the following example. We changed the version to 10.0.0 and added the jakarta classifier:

```
<dependency>
    <groupId>org.primefaces</groupId>
    <artifactId>primefaces</artifactId>
    <version>10.0.0</version>
    <classifier>jakarta</classifier>
</dependency>
```

As mentioned earlier, we need to change the theme. To do this, go to the web.xml file and look for the following lines at the top part of the file:

```
<context-param>
    <param-name>primefaces.THEME</param-name>
    <param-value>omega</param-value>
</context-param>
```

The param-value tells PrimeFaces we want to use the omega theme for the project. This theme is not usable with version 10 of PrimeFaces. To switch to a different theme, change the previous code line into this:

```
<context-param>
    <param-name>primefaces.THEME</param-name>
    <param-value>saga</param-value>
</context-param>
```

After performing the last step, we concluded the migration from javax to jakarta. You can now run mvn clean and mvn package cargo:run to run the Cargo tracker application using Jakarta EE on Payara 6.
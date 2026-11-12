Execute the `ch04_web-app.roo` script that creates the `flight-app` Roo project, sets up Hibernate as the persistence provider, configures MySQL as the database for the application, creates the `Flight` and `FlightDescription` JPA entities and defines a many-to-one relationship between the `Flight` and `FlightDescription` entities. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

Start the Roo shell from the `C:\roo-cookbook\ch04-recipe` directory.

Execute the `controller all` command to create controllers and views corresponding to the JPA entities in the `flight-app` project, as shown here:

```
.. roo> controller all --package ~.web
```

Execute the `perform eclipse` command to update the project's classpath settings, as shown here:

```
.. roo> perform eclipse
```

Now, import the `flight-app` project into your Eclipse IDE.

# How to do it...

To add static views to a Roo-generated web application execute the `web mvc install view` command, as shown here:

```
.. roo> web mvc install view --path /static/views --viewName help --title Help
```

```
Created SRC_MAIN_WEBAPP\WEB-INF\views\static\views
Created SRC_MAIN_WEBAPP\WEB-INF\views\static\views\help.jspx
Managed SRC_MAIN_WEBAPP\WEB-INF\i18n\application.properties
Managed SRC_MAIN_WEBAPP\WEB-INF\views\menu.jspx
Created SRC_MAIN_WEBAPP\WEB-INF\views\static\views\views.xml
Managed SRC_MAIN_WEBAPP\WEB-INF\spring\webmvc-config.xml
```

# How it works...

The following table describes the arguments that the `web mvc install view` command accepts:

| Argument | Description |
| :--- | :--- |
| `path` | Specifies the sub-folder inside the `/WEB-INF/views/` folder in which the view is created. |
| `viewName` | The name of the view JSPX file. |
| `title` | Specifies the name of the menu option with which the static view is accessible. |
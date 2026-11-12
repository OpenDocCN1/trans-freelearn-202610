
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

## How to do it...

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

## How it works...

The following table describes the arguments that the `web mvc install view` command accepts:

| Argument | Description |
| :--- | :--- |
| `path` | Specifies the sub-folder inside the `/WEB-INF/views/` folder in which the view is created. |
| `viewName` | The name of the view JSPX file. |
| `title` | Specifies the name of the menu option with which the static view is accessible. |

As the output from the `web mvc install view` command suggests, the following actions are taken by Spring Roo in response to executing the command:

- Creates a /static/views directory inside the /WEB-INF/views folder. Roo uses the value of the path argument to determine the directory to create.
- Creates a help.jspx file inside the /WEB-INF/views/static/views directory. The value of the viewName argument is used as the name of the JSPX file.
- Adds a property with value Help to the application.properties, that is, the value of the title argument is used as the value of the newly added property. The property is used by menu.jspx to show a **Help** menu option. The **Help** menu option allows access to the newly created help.jspx view.
- Creates a /WEB-INF/views/static/views/views.xml tiles definitions XML file, containing a single tiles definition for showing the help.jspx view, as shown here:

```xml
<tiles-definitions>
    <definition extends="default" name="static/views/help">
        <put-attribute name="body"
            value="/WEB-INF/views/static/views/help.jspx"/>
    </definition>
</tiles-definitions>
```

- Adds a <view-controller> element to the webmvc-config.xml to allow access to the help.jspx view without requiring to write a controller, as shown here:

`<mvc:view-controller path="/static/view/help"/>`

## See also

- Refer to the *Manually creating a Spring MVC controller for a JPA entity* recipe for details on how to create a custom controller and view

## Internationalizing Roo-generated web applications

Roo supports internationalization of the complete UI by using resource bundles for labels and messages. In this recipe, we will look at the `web mvc install language` command of Roo and see how it simplifies internationalizing the Roo-generated web user interface.

## Getting ready

Delete the contents of ch04-recipe sub-directory inside the C:\roo-cookbook directory.

Copy the ch04_web-app.roo script into the ch04-recipe directory.

Execute the ch04_web-app.roo script that creates the flight-app Roo project, sets up Hibernate as the persistence provider, configures MySQL as the database for the application, creates the Flight and FlightDescription JPA entities and defines a many-to-one relationship between the Flight and FlightDescription entities. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

Start the Roo shell from the C:\roo-cookbook\ch04-recipe directory.

Execute the controller all command to create controllers and views corresponding to the JPA entities in the flight-app project, as shown here:

```
.. roo> controller all --package ~.web
```

Execute the perform eclipse command to update the project's classpath settings, as shown here:

```
.. roo> perform eclipse
```

Now, import the project into your Eclipse IDE.

## How to do it...

For internationalizing the Roo-generated web user interface execute web mvc install language, as shown here:

```
.. roo> web mvc install language --code es
```

```
Created SRC_MAIN_WEBAPP\WEB-INF\i18n\messages_es.properties
Created SRC_MAIN_WEBAPP\images\es.png
Managed SRC_MAIN_WEBAPP\WEB-INF\views\footer.jspx
```

## How it works...

The web mvc install language command accepts a single argument: code, which identifies the language code for which the support needs to be added to the web application. The code argument accepts a pre-defined language code, depending upon the languages supported by Spring Roo. Spring Roo contains translations for the standard messages and labels for the following language codes: de (German), en (English), es (Espanol), it (Italian), nl (Dutch), and sv (Swedish).

When the web mvc install language command is executed, Roo processes the command by taking the following actions:

- Creates the messages_es.properties in /WEB-INF/i18n/ folder
- Copies an image icon (es.png) for the language in the images directory
- Updates /WEB-INF/views/footer.jspx to show the image icon for the language

To check if the support for the Espanol language is correctly installed, deploy and run the flight-app application. The following screenshot shows the home page of the flight-app application, after the Espanol language support is added:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_211_0.png)

As the given screenshot shows, an additional image icon is displayed to allow users to change the language of the web application to Espanol. When a user clicks the image icon corresponding to a language, the lang request parameter is set in the request, which is used by LocaleChangeInterceptor (configured in the /WEB-INF/spring/webmvc-config.xml) for changing the current locale.

The CookieLocaleResolver configured in the /WEB-INF/spring/webmvc-config.xml stores a cookie named locale in the browser, so that users don't need to change their preferred language every time they access the web application.

## There's more...

In the Auto-generating Spring MVC controllers and the JSPX views from the JPA entities recipe, we discussed that the messages.properties contains messages and labels that are common to all Roo-generated web applications and the application.properties contains application-specific messages and labels.

As Roo can't provide translations for application-specific messages and labels, the web mvc install language command doesn't create an application_<language-code>.properties file. It is left up to the developer to create an application_<language-code>.properties file for specific language codes and provide translations.

## See also

- Refer to the Auto-generating Spring MVC controllers and JSPX views from JPA entities recipe for LocaleChangeInterceptor and CookieLocaleResolver configuration

## Adding or modifying themes generated by Roo

A theme is a collection of CSS and image files that define the overall look and feel of the web application. Spring Web MVC framework provides built-in support for defining and applying themes. In the Auto-generating Spring MVC controllers and JSPX views from JPA entities recipes, we touched upon themes support in Roo-generated Spring Web MVC applications. In this recipe, we'll see in detail how to add new themes to Roo-generated Spring Web MVC applications and to modify default themes installed by Roo.

In this recipe, we'll make the following modifications to the Roo-generated `flight-app` web application:

- Add a new custom theme
- Modify existing themes to show different background color of menu headings, and different header images
- Add a new `standard` theme, which is applied if the language is `es` (that is, Espanol)

### Getting ready

Delete the contents of `ch04-recipe` the sub-directory inside the `C:\roo-cookbook` directory.

Copy the `ch04_web-app.roo` script into the `ch04-recipe` directory.

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

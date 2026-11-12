You can now use the **Flights** and **FlightDescriptions** menu options to perform CRUD operations on Flight and FlightDescription JPA entities.

# How it works...

The gwt setup command is processed by the GWT add-on of Spring Roo.

You might be wondering, why the Roo-generated GWT user interface doesn't show a link corresponding to the findFlightDescriptionsByDestinationAndOrigin finder method in the FlightDescription JPA entity? As of Spring Roo 1.1.5, the GWT add-on doesn't add finder functionality to the scaffolded GWT application.

The gwt setup command does the heavy lifting of scaffolding GWT Activities, Places, Proxies, and Views for performing CRUD operations on JPA entities. Let's first take a look at the Roo-generated GWT module descriptor file, ApplicationScaffold.gwt.xml, which describes a GWT module.

# GWT module descriptor

Roo creates ApplicationScaffold.gwt.xml in the root package, sample.roo.flightapp, of the flightapp-gwt project. It defines module dependencies, source paths, properties, deferred binding configurations, and module entry points. Let's look at some of the important elements defined in ApplicationScaffold.gwt.xml.

By default, the name of the GWT module is derived from the location of the module descriptor. As Roo creates the ApplicationScaffold.gwt.xml file in the sample.roo.flightapp package, the name of the module is sample.roo.flightapp.ApplicationScaffold. The ApplicationScaffold.gwt.xml file renames the module to applicationScaffold using the rename-to attribute of the <module> element, as shown here:

```
<module rename-to="applicationScaffold">
```

The GWT compiler generates JavaScript code in the directory identified by the module name; therefore, the code for our applicationScaffold module is generated in the applicationScaffold sub-directory of the generated WAR file.

The <inherits> element of the module descriptor specifies modules on which the module is dependent upon. For instance, applicationScaffold is dependent on User, Logging, Activity, Places, and so on, built-in modules of GWT, as shown here:

```
<inherits name='com.google.gwt.activity.Activity' />
<inherits name='com.google.gwt.place.Place' />
<inherits name="com.google.gwt.user.User"/>
<inherits name='com.google.gwt.logging.Logging' />
```
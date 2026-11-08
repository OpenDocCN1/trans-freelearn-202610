![](bbox=[0.3, 0.09, 0.7, 0.38])

The Database Connection user interface contains a grouping selector on the left pointing to General and Options, a Connection Name field, a Connection Type field, Settings, Access, and few buttons for actions (Test, OK, and Cancel). Selecting Options in the grouping selector, a collection of empty couples of parameter/value can be specified for the connection (they really depend on the database).

The Connection Name field is mandatory (in the example, it points to the existing `SampleData (Hypersonic)` connection). The Connection Type field lists all the available types of databases supported by the Pentaho Reporting Engine, with more than 40 different types of relational DBMS, from the most famous to the more specific ones, open source or legacy ones, newest, and oldest versions, and so on.

> *All the drivers are provided for the supported databases. Even if you are going to use a very common type of relational DBMS, you may need to add the right driver. To check for the existing drivers or to add a new one, look at the `jar` libraries in the `<report-designer>/lib/jdbc` folder. Always remember to restart Pentaho Report Designer after every change to the drivers in the `jdbc` folder.*

Once selected a connection type, settings can change accordingly. To be more precise, the number (and types) of settings requested by the user depends on the
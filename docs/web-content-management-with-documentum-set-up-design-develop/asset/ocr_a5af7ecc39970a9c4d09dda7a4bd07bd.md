We have used the WDK com.documentum.web.form.control.databound.TableResultSet class to store the ID and actual values for the drop-down entities.

```
dfquery.setDQL("select col1,col2 from dm_dbo.table1");
```

A DFC query is fired to select the ID and descriptive labels from the registered table table1 in the Docbase. Please refer to our discussion of the IDfquery interface in the last chapter for further details.

Figure 24.9 shows the structure/data in the registered table table1 in the Docbase:

![](bbox=[0.17, 0.28, 0.83, 0.53])

Figure 24.9: Data in Docbase registered table table1.

```
tableresultset.add(new String[]{strId, strLabel});
```

Each row of result returned from the DQL query is added as an individual row to the TableResultSet.

```
txt_display_order.getDataProvider().setResultSet
(tableresultset, null);
```

The getDataProvider() method is called on the data dropdown list control to return a handle to the data provider for this control. This returns a reference to the com.documentum.web.form.control.databound.DataProvider class.

The method setResultSet() is invoked on the DataProvider class to bind data to the control.

```
ErrorMessageService.getService().setNonFatalError
(this, "MSG_ERROR_ORDER_DROPDOWN_POPULATION", dfe);
```

com.documentum.web.common.ErrorMessageService is a Singleton class whose getService() method looks at the <errormessageservice> XML element in the application's app.xml file to find out the error message service class to be instantiated.
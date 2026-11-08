# Subreports using Java

To understand the development of subreports using Java, let's describe the various components used for its definition. First is the `SubReport` class, used to declare and manage subreport instances in `MasterReport`. Differently from a regular element, a subreport is added to a report's band using the `addSubReport(SubReport subReport)` method.

As you saw in Chapter 10, *Subreports and Cross Tabs*, subreports always use a different data source for the content definition. Using Java, all the declared queries in `MasterReport` are inherited by default from the subreports, but it is required to specify the query for `MasterReport` and `SubReport`, by using the `setQuery` method.

As often happens, a subreport may use one or more parameters coming from `MasterReport` to guide the correct rendering. To specify the input parameters, the `SubReport` class provides the `addInputParameter(String outerName, String sourceColumn)` method.

Apart from the topics discussed previously, the use of `SubReport` is similar to a regular `MasterReport` where you can add elements, customize the layout, and so on.
# Report Type introduction

As mentioned in earlier chapters, each report needs to be built on a report type:

+   - What is a Report Type?
+   - How do we check the Report Type used in an existing report?
+   - How important is it to select the correct report type when we create a new report?

All of these questions will be covered in this chapter.

Salesforce reports, including all standard reports and custom reports, are built on a report type. A report type is the foundation of a report. Once the report is created, you cannot change the report type, but if you have many reports that need to change the report type, you can engage a developer or consultant to write a script for you, but we will not discuss these advanced topics in this book. For now, if you need to change the report type, you need to recreate it as a new report.

The Report Type defines the set of records and fields available to a report based on the relationships between a primary object and the related objects defined. Ideally, to maintain report type; including report type creation, edit, and delete should fall under the Salesforce admin job scope, not the standard user. As a system admin, you need to think about the number of report types deployable and available for the user to use. Too many report types available will cause your user to get confused as which report type to use and what the difference is between each of them, so make sure that you balance it as required.

When a custom object is enabled for a report, Salesforce will auto-create a report type with Report Type Name equal to the Plural Name of the object name, and this report type will be stored in the Other Reports category folder.

![](bbox=[0.12, 0.72, 0.21, 0.82])

To use a custom object for a report type, make sure that you select Enable Reports for the object.

In order to understand report types easily, let's have an example--we have a custom object called Expense with Plural Label Expenses, you can use other custom objects too for testing. The object has a Lookup relationship field to Opportunity, and it has also been activated for Enable Reports and Track Field History. Let's have a quick look at the details in the Custom Object setup:
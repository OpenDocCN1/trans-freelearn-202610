# Technical requirements

You will need the following to successfully execute the instructions in this chapter:

+   - The latest version of the Google Chrome browser (Chrome is the preferred browser when working with TCRM)
+   - A working email address

Ensure that you're logged into your TCRM development organization.

Let's begin by understanding where you may need to use code in your TCRM application.

# When do you need code?

The GUI of TCRM has come a very long way indeed since Wave was launched by Salesforce in 2014. Early users of the platform were forced to build everything in JSON, SAQL, and XMD as there was, essentially, no declarative option. Things are different now, with every new release of TCRM providing more point-and-click functionality.

However, there are applications of TCRM that do require code as you move into more advanced data transformation and dashboard building.

Here are six common applications in TCRM where you are required to deploy code:

+   - Security predicates
+   - Bindings in the JSON
+   - SAQL expressions
+   - Bulk actions (using Visualforce and the TCRM REST API)
+   - `timeseries` (modified)
+   - XMD – advanced formatting options

Let's begin with security predicates.

# Security predicates

As you saw in *Chapter 7, Security in Tableau CRM*, a predicate is a filter condition that defines row-level security for a dataset. Security predicates range from extremely simple to highly complex, depending on the requirements. All require at least a little code.
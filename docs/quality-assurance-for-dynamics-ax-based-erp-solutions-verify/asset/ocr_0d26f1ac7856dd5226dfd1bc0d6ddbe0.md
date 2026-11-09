# Use of IntelliMorph Maximally

Due to a user's locale or preferred format a form may be presented in a different language and/or a different date, time, or currency format. Dynamics AX best practices recommend **Auto** as the value for the display properties related to the following:

+   - Date
+   - Currency
+   - Time
+   - Language
+   - Number format (such as decimal operator, separator, etc.)
+   - Label size
+   - Form size

The rule of thumb is to keep the various properties as **Auto** or default value, which will help IntelliMorph to function maximally. For further details about best practices readers are recommended to go through the Developers Guide for Best Practices.

# Reports

The peculiar fact about the reports is that they are output media where the external environment such as paper size, user's configuration about the locale or language, font size, etc. matters.

Dynamics AX recommends using 'Auto Design' to develop the report as these kinds of reports can change the layout according to external environmental variables. Another way to develop a report in Dynamics AX is 'Generated Design'; this type of design is recommended only when strict report layout is required. A few such examples may be regulatory reports, accounts reports, etc.

# Summary

In this chapter we discussed various areas where quality could be improved by adopting best practices. We also discussed various best practices, theory behind best practices, and how to adopt these best practices, i.e. with practical tips.
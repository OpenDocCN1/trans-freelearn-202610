# Defect Management Tool

The defect management tool must support classification with proper levels for categorizations. Besides this there are some more additional requirements for a Defect Management Tool as prescribed by the IBI Specifications for the Dynamics AX.

+   - The defect management system must have the capability to register, track, and report the following:
+     - Bugs
+     - Issues
+     - Design change requests
+   - The defect management tool must accept or generate the following information for each bug:
+     - Unique number (auto generated)
+     - Description of the bug
+     - Bug priority (four levels as mentioned above)
+     - Bug severity (four levels as mentioned above)
+     - A record of comments to show the how defect is handled throughout its life
+     - Closing comments
+   - The defect management tool should generate reports as per the different classification criteria mentioned above as well as status.
+   - The defect management tool must generate reports for all open bugs (i.e. bugs with status such as work in progress, under investigation, more information required, waiting clarification from client, deferred, etc). It will help to generate a report for all know bugs at any point of time.
+   - The defect management tool must accept additional information for security related bugs.
+   - The defect management tool must take an action after finding security-related issues:
+     - Must fix: Issues such as write access violations, stack overflow exceptions
+     - Must investigate: Large memory allocations, integer overflows, custom exceptions, other system-level exceptions that could lead to an exploitable crash
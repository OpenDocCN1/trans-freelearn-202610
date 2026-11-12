Perform the following steps in the package org to try out this process:

+   1. Create a Race Management custom application (under Setup | Create) and add the Driver, Season, and Race tabs created earlier to it; make the Race tab the default.
+   
+   2. Create a Race Analytics custom application and add the Race and Reports tabs to it; make the Reports tab the default.
+   
+   3. Create the Permission Sets for the FormulaForce application as shown in the following table. Leave the User License field set to None.
+   
+   4. Finally, add the following Permission Sets to the FormulaForce package.

| Permission Set | Permissions |
|---|---|
| FormulaForce - Race Management | Race Management (Custom Application) Race Analytics (Custom Application) Season tab (Custom Tab) Race tab (Custom Tab) Driver tab (Custom Tab) Object (Custom Object, Full Access) Race object (Custom Object, Full Access) Driver object (Custom Object, Full Access) Contestant object (Custom Object, Full Access) |
| FormulaForce - Race Management - Update DNF Status | Contestant object (Custom Object, Read, Edit) DNF (Custom Field, Read, Edit) |
| FormulaForce - Race Analytics | Race Analytics (Custom Application) Race object (Custom Object, Read Only, Read DNF) |

Here, Full Access means it gives access to read, create, edit, and delete all custom fields. Also, at the time of writing this, the Custom Application and Custom Tab components within Permission Sets are not packable. Until they become packable, it is recommended that you still maintain this information in the Permission Sets.
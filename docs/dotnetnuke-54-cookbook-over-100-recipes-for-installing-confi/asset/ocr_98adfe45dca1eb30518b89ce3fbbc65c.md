18. The file should now look like this:

![](bbox=[0.168, 0.11, 0.832, 0.385])

19. With the new user control in place and the resource file created, all that is left is to add the new files to the manifest. Double-click to open the Employee.dnn file.

20. Scroll down and add the following text:

```
<control>
    <src>DesktopModules/ACME.Employee/ViewEmployee.ascx</src>
    <type>View</type>
    <helpurl></helpurl>
</control>
<control>
    <key>AcmeEmployeeDisplay</key>
    <title>Display Employee</title>
    <src>DesktopModules/ACME.Employee/DisplayEmployee.ascx</src>
    <type>View</type>
    <helpurl></helpurl>
</control>
```

21. Scroll down a little and add:

```
<file>
    <name>ViewEmployee.ascx</name>
</file>
<file>
    <name>DisplayEmployee.ascx</name>
</file>
<file>
    <name>EditEmployee.ascx</name>
</file>
```
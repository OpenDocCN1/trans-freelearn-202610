10. Click on **Save**.

![](bbox=[0.185, 0.115, 0.816, 0.322])

+   11. Now that we have a DNN list, we can use it inside our custom module. Launch the Development Tool.
+   12. Look for Recent Projects shown on the left side of the screen. Click on **Employee** to load the project.
+   13. Double-click to open the `ViewEmployee.ascx` file.
+   14. Look at the top of the file:

```
```aspx
<%@ Control language="vb"
    Inherits="ACME.Modules.Employee.ViewEmployee"
    AutoEventWireup="false" Explicit="True"
    Codebehind="ViewEmployee.ascx.vb" %>
<%@ Register TagPrefix="dnn" TagName="Audit"
    Src="~/controls/ModuleAuditControl.ascx" %>
<%@ Register TagPrefix="dnn" TagName="Label"
    Src="~/controls/LabelControl.ascx" %>

<asp:datalist id="lstContent" datakeyfield="ItemID"
    runat="server" cellpadding="4">
    <itemtemplate>
```
```

15. Add the following HTML table code just after the last register directive and before the Datalist:

```
```aspx
<%@ Register TagPrefix="dnn" TagName="Label" Src="~/controls/
LabelControl.ascx" %>

<table id="tblPanel" cellspacing="2" cellpadding="2" border="0"
       runat="server">
    <tr>
        <td width="20%" class="SubHead">
            [Put a label here]
```
```
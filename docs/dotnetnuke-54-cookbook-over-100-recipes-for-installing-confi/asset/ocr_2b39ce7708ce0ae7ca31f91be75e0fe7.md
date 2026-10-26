4. When the module is displayed it will show the Employee records in the Employee table. The Starter Kit has given us a datalist control which is used to display records from the database. We need to put our Employee fields into this control. We do this by replacing the lblContent label control with label controls for each of our five fields.

```
<%@ Control language="vb"
    Inherits="YourCompany.Modules.Employee.ViewEmployee"
    AutoEventWireup="false" Explicit="True"
    Codebehind="ViewEmployee.ascx.vb" %>
<%@ Register TagPrefix="dnn" TagName="Audit"
    Src="~/controls/ModuleAuditControl.ascx" %>
<asp:datalist id="lstContent" datakeyfield="ItemID" runat="server" cellpadding="4">
<itemtemplate>
<table cellpadding="4" width="100%">
<tr>
<td valign="top" width="100%" align="left">
<asp:HyperLink ID="HyperLink1" NavigateUrl='<%# EditURL("ItemID",DataBinder.Eval(Container.DataItem,"ItemID")) %>' Visible="<%# IsEditable %>" runat="server"><asp:Image ID="Image1" Runat=server ImageUrl="~/images/edit.gif" AlternateText="Edit" Visible="<%#IsEditable%>" resourcekey="Edit"/></asp:hyperlink>
<asp:Label ID="lblEmpFirstName" runat="server" CssClass="Normal"/>
<asp:Label ID="lblEmpLastName" runat="server" CssClass="Normal"/>
<asp:Label ID="lblManagerNo" runat="server" CssClass="Normal"/>
<asp:Label ID="lblHireDate" runat="server" CssClass="Normal"/>
<asp:Label ID="lblSalary" runat="server" CssClass="Normal"/>
</td>
</tr>
</table>
</itemtemplate>
</asp:datalist>
```

5. Next we must add code to populate the datalist control. Double-click to open the ViewEmployee.ascx.vb file (if you don't see it listed in the Solution Explorer, click on the Show All Files icon on the Solution Explorer toolbar — the second one from the left).

> The datalist control is part of the DNN web controls library. It will display records from the database, but we must tell it what fields from the table to display.
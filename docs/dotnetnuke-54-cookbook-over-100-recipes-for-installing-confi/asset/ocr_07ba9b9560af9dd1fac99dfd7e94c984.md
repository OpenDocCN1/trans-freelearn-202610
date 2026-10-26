# Getting ready

To follow along with this recipe you must have completed the following recipe from Chapter 4:

+   - Creating a new module with the Starter Kit

# How to do it...

+   1. Start by launching your preferred development tool (Visual Studio or Visual Web Developer Express).
+   2. Look for Recent Projects shown on the left side of the screen. Click on Employee to load the project.
+   3. Double-click to open the EditEmployee.ascx file. The part we need to fix is an HTML table that holds the Employee fields. The Starter Kit created a single row in the HTML table and once again we must substitute the place holder Content with the names of our fields. This form offers a text label, input control, and a validation control, so we need to replace it with a label, an input control, and a validation control for our five fields.

```
<%@ Control language="vb"
    Inherits="YourCompany.Modules.Employee.EditEmployee"
    AutoEventWireup="false" Explicit="True"
    Codebehind="EditEmployee.ascx.vb" %>
<%@ Register TagPrefix="dnn" TagName="Label"
    Src="~/controls/LabelControl.ascx" %>
<%@ Register TagPrefix="dnn" TagName="TextEditor"
    Src="~/controls/TextEditor/TextEditor.ascx"%>
<%@ Register TagPrefix="dnn" TagName="Audit"
    Src="~/controls/ModuleAuditControl.ascx" %>
<table width="650" cellspacing="0" cellpadding="0" border="0"
    summary="Edit Table">
    <tr valign="top">
        <td class="SubHead" width="125"><dnn:label id="lblContent"
            runat="server" controlname="lblContent" suffix=":">
            </dnn:label>
        </td>
        <td>
            <dnn:texteditor id="txtContent" runat="server"
                height="200" width="500" />
            <asp:RequiredFieldValidator ID="valContent"
                resourcekey="valContent.ErrorMessage"
                ControlToValidate="txtContent"
                CssClass="NormalRed" Display="Dynamic"
                ErrorMessage="<br>EmpFirstName is required"
                Runat="server" />
```
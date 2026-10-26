```
<table width="650" cellspacing="0" cellpadding="0" border="0" summary="Edit Table">
<tr valign="top">
<td class="SubHead" width="125"><dnn:label id="lblEmpFirstName" runat="server" controlname="lblEmpFirstName" suffix=":"></dnn:label></td>
<td>
<asp:TextBox ID="txtEmpFirstName" runat="server"></asp:TextBox>
<asp:RequiredFieldValidator ID="valEmpFirstName" resourcekey="valEmpFirstName.ErrorMessage" ControlToValidate="txtEmpFirstName" CssClass="NormalRed" Display="Dynamic" ErrorMessage="<br>EmpFirstName is required" Runat="server" />
</td>
</tr>
<tr valign="top">
<td class="SubHead" width="125"><dnn:label id="lblEmpLastName" runat="server" controlname="lblEmpLastName" suffix=":"></dnn:label></td>
<td>
<asp:TextBox ID="txtEmpLastName" runat="server"></asp:TextBox>
<asp:RequiredFieldValidator ID="valEmpLastName" resourcekey="valEmpLastName.ErrorMessage" ControlToValidate="txtEmpLastName" CssClass="NormalRed" Display="Dynamic" ErrorMessage="<br>EmpLastName is required" Runat="server" />
</td>
</tr>
</table>
</dnn:DNNTabs>
<dnn:DNNTabs Label-Text="Information" ID="tab2">
<table width="650" cellspacing="0" cellpadding="0" border="0" summary="Edit Table">
<tr valign="top">
```
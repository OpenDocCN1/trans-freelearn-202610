6. That gives five rows in the HTML table holding our fields. The last thing we need to do is edit the code to show our employee fields. The entire HTML table should now look like this:

```
```html
<table width="650" cellspacing="0" cellpadding="0" border="0" summary="Edit Table">
    <tr valign="top">
        <td class="SubHead" width="125"><dnn:label
                                      id="lblEmpFirstName"
             runat="server" controlname="lblEmpFirstName" suffix=":">
                                  </dnn:label>
        </td>
        <td>
        <asp:TextBox ID="txtEmpFirstName" runat="server"></asp:TextBox>
        <asp:RequiredFieldValidator ID="valEmpFirstName"
                         resourcekey="valEmpFirstName.ErrorMessage"
                         ControlToValidate="txtEmpFirstName"
        CssClass="NormalRed" Display="Dynamic"
          ErrorMessage="<br>EmpFirstName is required" Runat="server" />
        </td>
    </tr>
```
```

7. The second row of the table has the employee's last name:

```
```html
<tr valign="top">
    <td class="SubHead" width="125"><dnn:label id="lblEmpLastName"
             runat="server" controlname="lblEmpLastName" suffix=":">
                                  </dnn:label>
    </td>
    <td>
    <asp:TextBox ID="txtEmpLastName" runat="server"></asp:TextBox>
    <asp:RequiredFieldValidator ID="valEmpLastName"
                         resourcekey="valEmpLastName.ErrorMessage"
                         ControlToValidate="txtEmpLastName"
    CssClass="NormalRed" Display="Dynamic" ErrorMessage="<br>
                 EmpLastName is required" Runat="server" />
    </td>
</tr>
```
```

8. The third row of the table has the employee's manager number:

```
```html
<tr valign="top">
    <td class="SubHead" width="125"><dnn:label id="lblManagerNo"
             runat="server" controlname="lblManagerNo" suffix=":">
                                  </dnn:label></td>
    <td>
    <asp:TextBox ID="txtManagerNo" runat="server"></asp:TextBox>
    <asp:RequiredFieldValidator ID="valManagerNo"
                         resourcekey="valManagerNo.ErrorMessage"
                         ControlToValidate="txtManagerNo"
```
```
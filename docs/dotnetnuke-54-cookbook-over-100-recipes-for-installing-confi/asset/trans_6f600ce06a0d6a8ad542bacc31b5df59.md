

## 准备工作

要继续本示例，您必须先完成第4章中的以下示例：

- 使用 Starter Kit 创建新模块

## 操作步骤...

1. 首先启动您偏好的开发工具（Visual Studio 或 Visual Web Developer Express）。
2. 在屏幕左侧查找“最近项目”（Recent Projects）。点击“Employee”以加载项目。
3. 双击打开 `EditEmployee.ascx` 文件。我们需要修复的部分是一个包含 Employee 字段的 HTML 表格。Starter Kit 在 HTML 表格中创建了一行，我们再次需要用字段名替换占位符 Content。该表单包含一个文本标签、一个输入控件和一个验证控件，因此我们需要将其替换为我们五个字段的标签、输入控件和验证控件。

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

```
</td>
</tr>
</table>
<p>
<asp:linkbutton cssclass="CommandButton" id="cmdUpdate" resourcekey="cmdUpdate" runat="server" borderstyle="none" text="Update">
</asp:linkbutton>&nbsp;
<asp:linkbutton cssclass="CommandButton" id="cmdCancel" resourcekey="cmdCancel" runat="server" borderstyle="none" text="Cancel" causesvalidation="False">
</asp:linkbutton>&nbsp;
<asp:linkbutton cssclass="CommandButton" id="cmdDelete" resourcekey="cmdDelete" runat="server" borderstyle="none" text="Delete" causesvalidation="False">
</asp:linkbutton>&nbsp;
</p>
<dnn:audit id="ctlAudit" runat="server" />
```

4. 修改上面突出显示的代码，将控件重命名，使用 EmpFirstName 代替 Content。我们还将使用 textbox 控件而不是 texteditor 控件。修改后的代码如下：

```
<tr valign="top">
<td class="SubHead" width="125"><dnn:label id="lblEmpFirstName" runat="server" controlname="lblEmpFirstName" suffix=":">
</dnn:label>
</td>
<td>
<asp:TextBox ID="txtEmpFirstName" runat="server"></asp:TextBox>
<asp:RequiredFieldValidator ID="valEmpFirstName" resourcekey="valEmpFirstName.ErrorMessage" ControlToValidate="txtEmpFirstName" CssClass="NormalRed" Display="Dynamic" ErrorMessage="<br>EmpFirstName is required" Runat="server" />
</td>
</tr>
```

5. 这样完成了一个字段（员工名字）。接下来是最难的部分——我们需要再重复这个过程四次，以创建其他四个字段。因此，将您刚刚为名字编辑的代码复制并粘贴四次。

6. 这样 HTML 表格中就有五行来存放我们的字段。最后需要做的是编辑代码以显示员工字段。整个 HTML 表格现在应该如下所示：

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

7. 表格的第二行是员工的姓氏：

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

8. 表格的第三行是员工的主管编号：

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

9. 表格的第四行是员工的入职日期：

```
<tr valign="top">
<td class="SubHead" width="125"><dnn:label id="lblHireDate" runat="server" controlname="lblHireDate" suffix=":">
</dnn:label></td>
<td>
<asp:TextBox ID="txtHireDate" runat="server"></asp:TextBox>
<asp:RequiredFieldValidator ID="valHireDate" resourcekey="valHireDate.ErrorMessage" ControlToValidate="txtHireDate" CssClass="NormalRed" Display="Dynamic" ErrorMessage="<br>HireDate is required" Runat="server" />
</td>
</tr>
```

10. 表格的第五行是员工的薪水：

```
<tr valign="top">
<td class="SubHead" width="125"><dnn:label id="lblSalary" runat="server" controlname="lblSalary" suffix=":">
</dnn:label>
</td>
<td>
<asp:TextBox ID="txtSalary" runat="server"></asp:TextBox>
<asp:RequiredFieldValidator ID="valSalary" resourcekey="valSalary.ErrorMessage" ControlToValidate="txtSalary" CssClass="NormalRed" Display="Dynamic" ErrorMessage="<br>Salary is required" Runat="server" />
</td>
</tr>
</table>
```

11. 要检查结果，请将编辑器模式切换到“源代码”（Source）。页面现在应该如下图所示（如果您看到缺少控件的错误，请不要担心。这些控件属于 DNN 站点的一部分，编辑器无法识别它们。要正确查看这些字段，您需要将模块部署到测试 DNN 站点）：

```
CssClass="NormalRed" Display="Dynamic" ErrorMessage=
"<br>ManagerNo is required" Runat="server"/>
</td>
</tr>
```

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_143_0.png)

12. 现在编辑表单已完成，我们需要添加代码，以便在表单打开时填充字段。双击打开 `EditEmployee.ascx.vb` 文件。
13. 向下滚动到 `Page_Load` 过程。
14. 在该过程中间查找注释行“get content”下方。

```
Dim objEmployees As New EmployeeController
Dim objEmployee As EmployeeInfo = _
                         objEmployees.GetEmployee(ModuleId, ItemId)
If Not objEmployee Is Nothing Then
    txtContent.Text = objEmployee.EmpFirstName
    ctlAudit.CreatedByUser = objEmployee.CreatedByUserName
    ctlAudit.CreatedDate = objEmployee.CreatedDate.ToString
Else ' security violation attempt to access item not related to
this Module
    Response.Redirect(NavigateURL(), True)
End If
```


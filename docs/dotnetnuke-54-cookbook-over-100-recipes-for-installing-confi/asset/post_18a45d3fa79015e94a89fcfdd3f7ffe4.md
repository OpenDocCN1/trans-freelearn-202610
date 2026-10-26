
## Getting ready

To follow along with this recipe you must have completed the following recipes:

- Adding web controls to your Toolbox
- Deploying a module as a standalone package (from Chapter 5)

## How to do it...

1. Launch the Development Tool and load the **Employee** project.
2. In the Solution Explorer, look under the references and make sure your project includes a reference to `DotNetNuke.WebUtility.dll`.
3. Double-click to open the `EditEmployee.ascx` file.
4. Make sure the following register directive is at the top of the file:

```aspx
<%@ Register tagprefix="DNN" assembly="DotNetNuke.WebControls"
             namespace="DotNetNuke.UI.WebControls" %>
```

5. Scroll down to the bottom of the file and note where the table holding the edit controls ends and the links to save, cancel, or delete are shown:

```aspx
</table>
<p>
    <asp:linkbutton cssclass="CommandButton" id="cmdUpdate"
                    resourcekey="cmdUpdate" runat="server"
                    borderstyle="none" text="Update">
    </asp:linkbutton>&nbsp;
```

6. Put the following code just after the HTML table and before the links:

```aspx
</table>

<p>
<dnn:CaptchaControl ID="btnCaptcha" CaptchaHeight="40"
                    CaptchaWidth="150"
ErrorStyle-CssClass="NormalRed" cssclass="Normal" runat="server"
                    resourcekey="Captcha"
ErrorMessage="The typed code is case sensitive, must match the
              image and cannot be left blank. Please try again."/>
</p>

<p>
    <asp:linkbutton cssclass="CommandButton" id="cmdUpdate"
                    resourcekey="cmdUpdate" runat="server"
                    borderstyle="none" text="Update">
    </asp:linkbutton>&nbsp;
```

7. Next, open the `EditEmployee.ascx.vb` file.

8. As the CAPTCHA control validates before allowing changes, we need to modify the update process to check if the CAPTCHA value has been given before performing the update. Scroll down to the `cmdUpdate_Click` procedure.

9. At the top of the procedure add the first part of an IF statement; and place the rest at the bottom of the procedure. The entire procedure should now look like this:

```vb
Private Sub cmdUpdate_Click(ByVal sender As Object,
                             ByVal e As EventArgs) Handles cmdUpdate.Click
    Try

        If btnCaptcha.IsValid Then

            Dim objEmployees As New EmployeeController

            Dim objEmployee As EmployeeInfo =
                                      New EmployeeInfo

            objEmployee.ModuleId = ModuleId
            objEmployee.ItemId = ItemId
            objEmployee.EmpFirstName =
                                      txtEmpFirstName.Text
            objEmployee.EmpLastName = txtEmpLastName.Text
            objEmployee.ManagerNo = txtManagerNo.Text
            objEmployee.HireDate = txtHireDate.Text
            objEmployee.Salary = txtSalary.Text

            objEmployee.CreatedByUser = Me.UserId

            If Common.Utilities.Null.IsNull(ItemId) Then
                ' add the content within the Employee
                                                  table
                objEmployees.AddEmployee(objEmployee)
            Else
                ' update the content within the Employee
                                                  table
                objEmployees.UpdateEmployee(objEmployee)
            End If

            ' Redirect back to the portal home page
            Response.Redirect(NavigateURL(), True)

        End If

    Catch exc As Exception     'Module failed to load
        ProcessModuleLoadException(Me, exc)
    End Try
End Sub
```

10. Select **Save All** from the **File** menu.

11. To check the results, build and deploy the module to a development portal for testing (for instructions see the recipe *Deploying a module as a standalone package* in *Chapter 5*).

12. Go to the **ACME Employee** page and click on the edit icon (the little pencil) next to an employee to display the **Edit Employee** page. At the bottom of the page the CAPTCHA control will appear.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_234_0.png)

13. When you click on the **Update** link to save any changes to the record it will check that you have filled in the correct CAPTCHA value. If the value doesn't match the image an error message is displayed and the record is not saved.

## How it works...

In this recipe we saw the tasks to use a CAPTCHA control:

- We placed the control in the `.ascx` file and set the properties
- We modified the normal update procedure to check the CAPTCHA control and only save if the correct value was entered

## See also

Other control properties you can set:

| Property | Meaning |
|---|---|
| `CaptchaChars` | The set of characters that could be used to create the CAPTCHA. |
| `CaptchaHeight` | The height of the CAPTCHA image. |
| `CaptchaLength` | Total maximum number of characters to use in the CAPTCHA. |
| `CaptchaWidth` | The width of the CAPTCHA image. |
| `CssClass` | The CSS class for the control. |
| `Text` | The prompt Text that is used. |

## Creating a multi-state checkbox

A checkbox control normally has two possible values (or states): checked and unchecked. The DNN Multi-State checkbox control is used in situations when you want to show a visual indicator for the state of the control but you need more then checked and unchecked.

You can see an example of this with the DNN page security where you grant, deny, or ignore access to a DNN page:

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_235_0.png)

In this recipe we will add a four-state checkbox to the Edit Employee page representing the Bonus Level of the employee (None, Small, Medium, and Large) represented by color-coded star icons.

## Getting ready

To follow along with this recipe you must have completed the following recipes:

- Adding web controls to your Toolbox
- Deploying a module as a standalone package (from Chapter 5)

## How to do it...

1. Launch the Development Tool and load the Employee project.
2. In the Solution Explorer, look under the references and make sure your project includes a reference to DotNetNuke.WebUtility.dll.
3. Double-click to open the EditEmployee.ascx file.
4. Make sure the following register directive is at the top of the file:

```
<%@ Register tagprefix="DNN" assembly="DotNetNuke.WebControls" namespace="DotNetNuke.UI.WebControls" %>
```

5. Scroll down to the bottom of the file and locate the table holding the edit controls. Add a new table row by inserting the following code:

```
<tr valign="top">
    <td class="SubHead" width="125"><dnn:label id="lblBonus" runat="server" controlname="msbBonus" suffix=":">
    </dnn:label>
    </td>
    <td>
        <dnn:DNNMultiStateBox ID="msbBonus" runat="server" ImagePath="~/images/">
            <States>
                <dnn:DNNMultiState Key="1" ImageUrl="unchecked.gif" DisabledImageUrl="lock.gif" />
                <dnn:DNNMultiState Key="2" ImageUrl="ratingminus.gif" DisabledImageUrl="lock.gif" />
                <dnn:DNNMultiState Key="3" ImageUrl="ratingzero.gif" DisabledImageUrl="lock.gif" />
                <dnn:DNNMultiState Key="4" ImageUrl="ratingplus.gif" DisabledImageUrl="lock.gif" />
            </States>
        </dnn:DNNMultiStateBox>
    </td>
</tr>

</table>
```

6. Next, open the EditEmployee.ascx.vb file.

7. Check the top of the file and make sure it has the following imports:

```
Imports DotNetNuke.UI.WebControls
```

8. To set the checkbox when the employee information is displayed, scroll down to the Page_Load procedure and add the following code after the existing set values code:

```
txtHireDate.Text = objEmployee.HireDate
txtSalary.Text = objEmployee.Salary

msbBonus.SelectedStateKey = objEmployee.BonusLevel
```

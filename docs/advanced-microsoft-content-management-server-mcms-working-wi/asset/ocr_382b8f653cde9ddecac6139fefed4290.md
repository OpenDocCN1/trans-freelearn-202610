```
<h1>Move Template</h1>
(Add Literal for displaying the path here)
</td>
</tr>
<tr>
<td>Destination Gallery:</td>
<td>
(Add the text box for the destination gallery here)
</td>
</tr>
<tr>
<td colspan="2">(Add the Label for displaying error messages here)</td>
</tr>
<tr>
<td colspan="2" align="right">
(Add the Move Template button here)
<INPUT type="button" value="Close" onclick="javascript:window.close();">
</td>
</tr>
</table>
```

3. Toggle to Design view. Drag and drop the following controls from the Web Forms section of the Toolbox and set their properties as shown below:

| Control | Property | Value |
|---|---|---|
| Literal | ID | litOriginalTemplate |
| TextBox | ID | txtDestination |
| Label | ID | lblErrorMessage |
|  | Text | (empty string) |
| Button | ID | btnMove |
|  | Text | Move Template |

When completed, the web form appears in Design view as shown below:

![](bbox=[0.29, 0.61, 0.73, 0.74])

4. Double-click on the form to get to its code-behind file. Add the Microsoft.ContentManagement.Publishing namespace:

```
. . . code continues . . .
// MCMS PAPI
using Microsoft.ContentManagement.Publishing;

namespace CmsExplorer
{
. . . code continues . . .
}
```
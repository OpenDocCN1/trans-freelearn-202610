

```csharp
private CmsHttpContext cmsContext;
private Template originalTemplate;

private void Page_Load(object sender, System.EventArgs e)
{
    cmsContext = CmsHttpContext.Current;

    // Clear all error messages, if any
    lblErrorMessage.Text = String.Empty;

    // Get an instance of the original template
    string cmsObjectGuid = String.Empty;
    if (Request.QueryString["CMSObjectGuid"] != null)
    {
        cmsObjectGuid = Request.QueryString["CMSObjectGuid"];
        originalTemplate = cmsContext.Searches.GetByGuid(cmsObjectGuid) as Template;

        if (originalTemplate != null)
        {
            // Display the template's path
            litOriginalTemplate.Text = originalTemplate.Path;
        }
        else
        {
            // Uh-oh we can't get an instance of the template.
            lblErrorMessage.Text = "Original Template not found!";
        }
    }
}
```

7. 现在，让我们获取目标模板库的实例。用户应该已在提供的文本框中输入了目标模板库的路径。因此，要获取目标模板库的实例，我们只需使用 Searches.GetByPath() 方法。如果用户输入了无效路径或一个其无权创建模板的库的路径，则会在屏幕上显示错误消息。切换到设计视图并双击 btnCopy 按钮以进入 btnCopy_Click() 事件处理程序。添加以下代码：

```csharp
private void btnCopy_Click(object sender, System.EventArgs e)
{
    try
    {
        // creating a copy of a template

        // get the destination template gallery
        TemplateGallery destinationGallery;
        string destinationGalleryPath = txtDestination.Text;
        destinationGallery = cmsContext.Searches.GetByPath(
            destinationGalleryPath) as TemplateGallery;

        if (destinationGallery != null)
        {
            if (destinationGallery.CanCreateTemplates)
            {
                // TODO:Add code to copy template here
            }
            else
            {
                // We do not have rights to create templates in the
                // specified destination template gallery
                lblErrorMessage.Text =
                    "You do not have rights to create templates "
            }
        }
    }
}
```

复制的模板将继承原始模板的所有已提交属性值以及自定义属性和占位符定义集合。

与连接的模板不同，复制的模板与原始模板没有任何关联。对原始模板的更新不会更改复制模板的属性、定义集合或状态。

让我们尝试复制 Plant 模板：

1. 在 CMS Explorer 中，单击 Templates。
2. 导航到 TropicalGreen | PlantCatalog。
3. 单击 Plant 模板的 Edit 链接。
4. 单击 Copy。
5. 在 Destination Gallery 字段中，输入 /Templates/MyTemplateGallery/。
6. 单击 Copy Template。
7. 单击 Close。
8. 返回 CMS Explorer，导航到 MyTemplateGallery。Plant 模板的副本现在位于其中。

> 复制模板库的唯一方法是使用 Site Manager。PAPI 中没有 TemplateGallery.CopyTo() 方法。
> 
> 然而，使用 Site Manager 中的“Copy”功能复制模板库时要小心。如果源库中包含连接的模板，则该关系不会保留在目标库中。

## 移动模板库和模板

模板库和模板是通过模板资源管理器或站点管理器将对象从一个容器拖放到另一个容器来移动的。PAPI 提供了 TemplateGalleryItem.MoveTo() 方法以编程方式移动这些对象。

该技术与复制模板类似。我们需要做的是：

- 首先，获取我们希望移动的模板库项的引用。
- 接下来，获取模板库项将被移动到的目标模板库的引用。
- 最后，调用 TemplateGalleryItem.MoveTo() 方法。

让我们为 CMS Explorer 构建一个移动模板对话框。

向项目添加一个名为 MoveTemplate.aspx 的 Web 表单。

1. 将 styles.css 拖放到 Web 表单上。
2. 在 HTML 视图中，输入以下代码：

```html
<table>
<tr>
<td colspan="2">
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

3. 切换到设计视图。从工具箱的 Web Forms 部分拖放以下控件并按如下所示设置其属性：

| Control | Property | Value |
|---|---|---|
| Literal | ID | litOriginalTemplate |
| TextBox | ID | txtDestination |
| Label | ID | lblErrorMessage |
|  | Text | (empty string) |
| Button | ID | btnMove |
|  | Text | Move Template |

完成后，Web 表单在设计视图中如下所示：

![](img/442e26bf28f2df81fbf8dff3478f88e3_103_0.png)

4. 双击表单以进入其代码隐藏文件。添加 Microsoft.ContentManagement.Publishing 命名空间：

```csharp
. . . code continues . . .
// MCMS PAPI
using Microsoft.ContentManagement.Publishing;

namespace CmsExplorer
{
. . . code continues . . .
}
```

5. 在 Page_Load() 事件处理程序中，我们获取要移动的模板的实例。在之前的示例中，我们在用户单击按钮后才检查用户是否有权操作项目。这次我们尝试不同的方法。不是向没有必要权限的用户返回错误消息，我们将相应地启用或禁用按钮。
我们通过检查 Template.CanMove 属性值来检查用户是否有权移动模板。如果 Template.CanMove 返回 true，则表示用户是管理员或被分配到当前模板库的频道管理员或模板设计师角色的权限组。如果用户有权移动模板，我们保持 btnMove 按钮启用；否则，禁用它。

```csharp
private CmsHttpContext cmsContext;
private Template originalTemplate;

private void Page_Load(object sender, System.EventArgs e)
{
    cmsContext = CmsHttpContext.Current;

    // Clear all error messages, if any.
    lblErrorMessage.Text = String.Empty;

    // Get an instance of the original template
    string cmsObjectGuid = String.Empty;
    if (Request.QueryString["CMSObjectGuid"] != null)
    {
        cmsObjectGuid = Request.QueryString["CMSObjectGuid"];
        originalTemplate = cmsContext.Searches.GetByGuid(cmsObjectGuid)
            as Template;

        if (originalTemplate != null)
        {
            // Display the template's path
            litOriginalTemplate.Text = originalTemplate.Path;

            if (originalTemplate.CanMove)
            {
                btnMove.Enabled = true;
            }
            else
            {
                // we can't move this template.
                lblErrorMessage.Text = "You do not have rights to " +
                    "move this template.";
                btnMove.Enabled = false;
            }
        }
        else
        {
            // Uh-oh we can't get an instance of the template.
            lblErrorMessage.Text = "Original Template not found!";
        }
    }
}
```

6. 接下来，我们获取目标模板库的实例。我们仅在用户有权在目标模板库中创建模板时才执行移动操作。一旦我们确定目标模板库有效，我们将调用 Template.MoveTo() 方法，并将目标模板库的实例作为输入参数传递。移动模板后，其状态为 Saved。




> 你也可以通过在 Web Author 控制台中附加此对话框的方式提供“创建频道”选项，使用我们在《使用 Microsoft Content Management Server 构建网站》（Packt Publishing，2005 年 1 月，ISBN 1-904811-16-7）一书的第 19 章《自定义 Web Author 控制台》中讨论的技术。

## 创建帖子

与频道一样，帖子也可以使用 PAPI 以编程方式创建。为了演示如何实现，我们将在 CMS Explorer 中创建一个用于创建帖子的对话框：

- 1. 在 CMSEXplorer 解决方案中添加一个新的 Web 表单，并将其命名为 CreatePosting.aspx。
- 2. 在设计视图中，从解决方案资源管理器将 Styles.css 文件拖放到表单上。这会将样式表应用到页面。
- 3. 切换到 HTML 视图并插入以下用于创建包含六行的表格的代码：

```
<table>
<tr>
<td colspan="2">
<h1>Create Posting</h1>
<h2>Parent Channel: (Add Literal for displaying the path here)</h2>
</td>
</tr>
<tr>
<td>Name:</td>
<td>(Add the text box for the Name here)</td>
</tr>
<tr>
<td>Description:</td>
<td>(Add the text box for the Description here)</td>
</tr>
<tr>
<td>Template Path:</td>
<td>(Add the text box for the Template Path here)</td>
</tr>
<tr>
<td colspan="2">(Add the Label for displaying error messages here)</td>
</tr>
<tr>
<td colspan="2" align="right">
(Add the Create Posting button here)
<INPUT type="button" value="Close" onclick="javascript:window.close();">
</td>
</tr>
</table>
```

- 4. 在设计视图中，添加控件（来自工具箱的 Web Forms 部分）到 Web 表单并按以下图所示进行排列。请注意，我们通过使用文本框来指定模板路径从而简化了界面。你可以将其增强为模板选择器，为用户提供友好的界面以从他们有权访问的模板列表中选择模板，类似于 Web Author 中使用的模板选择对话框。

## 使用 PAPI 管理频道和帖子

- 7. 切换回设计视图并双击 btnCreate 按钮以进入 btnCreate_OnClick() 事件处理程序。输入以下所示的代码：

```
private void btnCreate_Click(object sender, System.EventArgs e)
{
    // IMPORTANT: You must be in update mode for the code to work
    CmsHttpContext cmsContext = CmsHttpContext.Current;
    Channel parentChannel = cmsContext.Channel;

    try
    {
        // get the template the new posting would be based on
        Template template;
        template = cmsContext.Searches.GetByPath(txtTemplate.Text)
            as Template;
        if (template != null)
        {
            // check if we can create a posting
            if (!parentChannel.CanCreatePostings)
            {
                throw new Exception("Current user does not have "
                    + "permissions to create postings in channel: "
                    + "parentChannel.Path);
            }

            // get the name of the posting
            string Name = txtName.Text.Trim();

            if (!Utility.ValidateCMSObjectName(Name))
            {
                throw new Exception("Specified posting name is not "
                    + "valid. May contain only alphanumeric "
                    + "characters, open or close parens, "
                    + "hyphens, underscores, periods, or "
                    + "spaces. No period at the end and no "
                    + "consecutive periods are allowed. ");
            }

            // create the new posting
            Posting newPosting;
            newPosting = parentChannel.CreatePosting(template);

            // set the properties of the new posting
            newPosting.Name = txtName.Text;
            newPosting.Description = txtDescription.Text;

            // commit the changes
            cmsContext.CommitAll();

            // display the success message
            lblErrorMessage.Text = "Posting created successfully!";
        }
        else
        {
            // could not access the specified template
            lblErrorMessage.Text = "Invalid Template path or you "
                + "don't have the appropriate permissions to "
                + "use the specified Template";
        }
    }
    catch(Exception ex)
    {
        // rollback the changes
        cmsContext.RollbackAll();

        // after a rollback it is required to dispose of the CMS context
        cmsContext.Dispose();
    }
}
```

```
// display the error message
lblErrorMessage.Text = ex.Message;
}
```

此代码首先访问父频道，即新帖子将在其中创建的频道。接下来，基于输入的模板路径，我们使用 Searches.GetByPath() 方法获取新帖子应基于的模板对象的实例。如果可以访问模板对象，我们将其作为输入参数传递给 Channel.CreatePosting() 方法，该方法创建新的帖子。

创建帖子后，我们设置其属性值。在此示例中，我们将 Name 和 Description 属性设置为文本框中输入的值。你可能希望增强此对话框以包含用于设置帖子的所有可写属性和占位符的输入控件。

直到调用 cmsContext.CommitAll() 时，新帖子才会被保存到存储库。如果发生错误，消息将显示在 lblErrorMessage 标签中。

保存并构建解决方案。要查看其运行效果：

- 1. 打开 http://localhost/CMSEXplorer。
- 2. 点击 MyNewChannel 查看其内容。频道目前为空，因为我们尚未在其中创建任何帖子或频道。
- 3. 从工具栏中选择 New | New Posting。这将打开一个包含创建帖子对话框的新浏览器窗口。
- 4. 在创建帖子对话框中输入：

| 属性 | 值 |
|---|---|
| Name | My New Posting |
| Description | This posting was created using the PAPI! |
| Template Path | /Templates/TropicalGreen/PlantCatalog/Plant |

- 5. 点击 Create Posting，然后点击 Close。
- 6. 在主窗口中点击 Refresh。新的帖子已在频道中创建！

## 我收到以下异常：“Unable to retrieve PostingEvents. The module Microsoft.ContentManagement.Publishing.Events.PostingEventsModule must be included in the section of the web.config and must be assigned the name 'CmsPosting'.”。我该怎么办？

很可能你在第 1 章中没有创建 MCMS Web 应用程序项目，而是创建了一个常规的 ASP.NET Web 应用程序。当你创建 MCMS Web 应用程序时，它会向应用程序的 web.config 中添加 HttpModules。

要将 ASP.NET Web 应用程序转换为 MCMS Web 应用程序，请在 Visual Studio .NET 的项目菜单中选择 Enable as MCMS project。

> 如果你已安装在《使用 Microsoft Content Management Server 构建网站》一书中创建的 Tropical Green 项目（第 1 章《构建 CMS Explorer》中提到），请注意，由于 CMS Explorer 与 Tropical Green 处于不同的应用程序范围，因此添加到 Tropical Green 项目的 `global.asxc.cs` 文件中的工作流事件处理程序将不会被触发。

## 连接的帖子

连接的页面是两个不同的帖子对象的副本，它们共享相同的占位符值。由于连接的帖子共享占位符定义和值，因此对一个帖子的属性所做的任何更改都将反映在其所有连接的帖子中。

让我们创建一个创建连接帖子的对话框：

- 1. 在 CMSExplorer 项目中添加一个名为 `CreateConnectedPosting.aspx` 的 Web 表单。
- 2. 将 `Styles.css` 拖放到 Web 表单上。
- 3. 切换到 HTML 视图并在 `<form>` 标签之间添加以下由六行组成的表格。
- 4. 切回设计视图。从工具箱的 Web Forms 部分拖放以下控件。

```
```html
<table>
<tr>
    <td colspan="2">
        <h1>Create Connected Posting</h1>
        <h2>Original Posting: (Add Literal for displaying the path here)</h2>
    </td>
</tr>
<tr>
    <td>Display Name:</td>
    <td>(Add the text box for the Name here)</td>
</tr>
<tr>
    <td>Template Path:</td>
    <td>(Add the text box for the Template File here)</td>
</tr>
<tr>
    <td>Destination Channel:</td>
    <td>(Add the text box for the Destination Channel here)</td>
</tr>
<tr>
    <td colspan="2">(Add the Label for displaying error messages here)</td>
</tr>
<tr>
    <td colspan="2" align="right">
        (Add the Connected Posting button here)
        <INPUT type="button" value="Close"
               onClick="javascript:window.close();">
    </td>
</tr>
</table>
```
```

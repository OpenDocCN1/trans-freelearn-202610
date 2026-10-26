

| Control | Property | Property Value |
|---|---|---|
| Literal | ID | litOriginalPosting |
| TextBox | ID | txtDisplayName |
| TextBox | ID | txtTemplate |
| TextBox | ID | txtDestination |
| Button | ID | btnCreate |
|  | Text | Create Connected Posting |
| Label | ID | lblErrorMessage |
|  | Text | (empty string) |

![](img/442e26bf28f2df81fbf8dff3478f88e3_71_0.png)

- 5. 在设计视图中，双击表单以打开代码隐藏文件。添加命名空间 Microsoft.ContentManagement.Publishing：

```
// ... code continues ...
// MCMS API
using Microsoft.ContentManagement.Publishing;

namespace CmsExplorer
{
... code continues ...
}
```

- 6. 在 Page_Load() 事件处理程序中，我们获取将作为连接发布的基础的发布实例（或原始发布），并在 litOriginalPosting 文本中显示其路径。我们将首先从 CmsObjectGuid 查询字符串参数中检索发布的 GUID，然后使用 Searches.GetByGuid() 方法获取其实例。

```
private void Page_Load(object sender, System.EventArgs e)
{
    // Display the path of the original posting
    Posting posting = CmsHttpContext.Current.Searches.GetByGuid(
        Request.QueryString["CMSObjectGuid"]) as Posting;
    if (posting != null)
        litOriginalPosting.Text = posting.Path;
    else
        lblErrorMessage.Text = "Requested posting does not exist.";
}
```

## 使用 PAPI 管理频道和发布

连接发布的创建方式与创建新发布非常相似。要创建连接发布，请调用 `Channel.CreateConnectedPosting()` 方法。该方法接受两个参数：

- - 发布应基于的模板（模板需要与原始发布的模板相同，或为原始模板的连接模板）。
- - 原始发布。

7. 切回设计视图。双击 btnCreate 按钮以进入 btnCreate_Click() 事件处理程序。添加如下所示的代码：

```
private void btnCreate_Click(object sender, System.EventArgs e)
{
    CmsHttpContext cmsContext = CmsHttpContext.Current;
    Posting originalPosting;
    originalPosting = cmsContext.Searches.GetByGuid(
        Request.QueryString["CMSObjectGuid"]) as Posting;

    if ( originalPosting != null )
    {
        try
        {
            // get the destination channel
            Channel destination;
            destination = cmsContext.Searches.GetByPath(txtDestination.Text)
                as Channel;

            if ( destination != null )
            {
                // check if the current user has permissions to
                // create postings in the target channel
                if (!destination.CanCreatePostings)
                {
                    throw new Exception("Current user does not have permissions "
                        + "to create postings in channel: "
                        + destination.Path);
                }

                // get the connected template
                Template connectedT;
                connectedT = cmsContext.Searches.GetByPath(txtTemplate.Text)
                    as Template;
                if ( connectedT != null )
                {
                    if ( connectedT.CanUseForAuthoring )
                    {
                        // create the connected posting
                        Posting newConnectedPosting;
                        newConnectedPosting = destination.CreateConnectedPosting(
                            connectedT, originalPosting);

                        // set the properties of the connected posting
                        newConnectedPosting.DisplayName = txtDisplayName.Text;

                        // commit the changes
                        cmsContext.CommitAll();

                        // display the success message
                        lblErrorMessage.Text = "Connected Posting created successfully!";
                    }
                }
            }
        }
    }
}
```

```
```csharp
else
{
    throw new Exception("Current user does not have permissions "
        + "to create postings using template: "
        + connectedT.Path);
}
}
}
else
{
    lblErrorMessage.Text = "Destination Channel not found";
}
}
catch(Exception ex)
{
    // rollback the changes
    cmsContext.RollbackAll();

    // after a rollback it is required to dispose of the CMS context
    cmsContext.Dispose();

    // display the error message
    lblErrorMessage.Text = ex.Message;
}
else
{
    //display the error message
    lblErrorMessage.Text = "Requested posting does not exist.";
}
```
```

保存并构建解决方案。

代码首先获取在 `txtDestination` 文本框中指定的目标频道的实例。然后通过查看 `Channel.CanCreatePostings` 属性返回的值来检查用户是否有权在目标频道中创建发布。

一旦我们确定目标频道存在并且用户有权在该频道内创建发布，我们就会获取连接模板的实例。连接模板的路径从 `txtTemplate` 文本框中检索。我们仅在以下情况下继续：

- - 连接模板存在。
- - 当前用户有权使用该模板创建发布。这是通过查看 `Template.CanUseForAuthoring` 属性返回的值来检查的。

要创建连接发布，请调用 `Channel.CreateConnectedPosting()`。我们将模板和原始发布作为输入参数传递。创建连接发布后，我们设置其属性。在这里，我们根据 `txtDisplayName` 文本框中输入的值设置其显示名称。当然，您可以设置发布的任何可写属性（请记住这是允许的，并且对某些属性定义和值的更改可能会影响其他连接发布）。

如果一切顺利，成功消息将显示在屏幕上。否则，将显示错误消息，并且整个操作将被回滚。

尝试使用我们刚刚创建的对话框添加一个连接发布。

- 1. 打开 `http://localhost/CMSExplorer`。
- 2. 点击 MyNewChannel。
- 3. 点击我们之前创建的 My New Posting 发布旁边的 Edit 链接。
- 4. 点击 Create Connected Posting。
- 5. 在 Create Connected Posting 对话框中，输入：

| Property | Value |
|---|---|
| Display Name | MyConnectedPosting |
| Template Path | /Templates/TropicalGreen/PlantCatalog/Plant |
| Destination Channel | /Channels/TropicalGreen/ |

- 6. 点击 Create Connected Posting，然后点击 Close。
- 7. 返回 CMS Explorer，导航到 TropicalGreen 频道。您应该会看到新的连接发布。

## 复制发布

发布可以从一个频道复制到另一个频道，甚至复制到同一频道。与连接发布不同，通过复制操作创建的发布作为独立实体存在。它们不共享占位符值，因此对原始发布属性的更改不会以任何方式影响副本。

复制可以使用 `Posting.CopyTo()` 方法完成。让我们为 CMS Explorer 创建一个 Copy Posting 对话框：

- 1. 向 CMSExplorer 项目添加一个新的 Web 表单。将新 Web 表单命名为 `CopyPosting.aspx`。
- 2. 在设计视图中，从解决方案资源管理器将 `Styles.css` 文件拖放到表单上。这会将样式表应用到页面。
- 3. 切换到 HTML 视图并在 `<form>` 标签之间添加如下所示的代码，以创建一个包含四行的表格：

```
```html
<table>
<tr>
    <td colspan="2">
        <h1>Copy Posting</h1>
        <h2>Original Path: (Add Literal for displaying the path here)</h2>
    </td>
</tr>
<tr>
    <td>Destination Channel:</td>
    <td>(Add the text box for the Destination Channel here)</td>
<tr>
    <td colspan="2">(Add the Label for displaying error messages here)</td>
</tr>
<tr>
    <td colspan="2" align="right">
        (Add the Copy Posting button here)
        <INPUT type="button" value="Close"
```


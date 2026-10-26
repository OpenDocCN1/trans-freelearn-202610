

| 对象类型 | 操作 |
|---|---|
| 通道、模板库、模板、资源库 | 属性<br>删除 |
| 发布 | 复制<br>移动<br>创建关联发布<br>属性<br>删除 |
| 模板 | 复制<br>移动<br>创建关联模板<br>属性<br>删除 |
| 资源 | 替换<br>属性<br>删除 |

完成后，DataGrid 的显示效果如下，并且我们点击了 Egg Plant 发布项的编辑按钮：

![](img/442e26bf28f2df81fbf8dff3478f88e3_42_0.png)

我们向 DataGrid 添加了两列新列，一列用于显示编辑按钮，另一列用于包含可能的动作列表。

```xml
<Columns>
<asp:TemplateColumn>
<ItemTemplate></ItemTemplate>
</asp:TemplateColumn>
<asp:TemplateColumn HeaderText="DisplayName">
<ItemTemplate>
<a id="aName" runat="server">
<%= DataBinder.Eval(Container.DataItem, "DisplayName") %>
</a>
</ItemTemplate>
</asp:TemplateColumn>
<asp:BoundColumn DataField="LastModifiedDate" ReadOnly="True"
HeaderText="Last Modified Date"
DataFormatString="{0:dd MMM yyyy hh:mm tt}"></asp:BoundColumn>

<asp:EditCommandColumn ButtonType="LinkButton" EditText="Edit">
<asp:EditCommandColumn>
<asp:TemplateColumn></asp:TemplateColumn>
</Columns>
```

点击编辑按钮时，我们将 DataGrid1 的 EditItemIndex 设置为所选行的索引。在 DataGrid1 的事件属性窗口中，双击 EditCommand 以注册事件处理程序并添加以下代码：

```csharp
private void DataGrid1_EditCommand(object source,
System.Web.UI.WebControls.DataGridCommandEventArgs e)
{
    DataGrid1.EditItemIndex = e.Item.ItemIndex;
    BindData();
}
```

同时，我们希望显示可在所选对象上执行的操作列表。这由 AddActionItems() 方法完成。该方法为上表中定义的每个操作项创建超链接。AddActionItems() 方法接受两个输入参数：

- 一个名为 actionCell 的 TableCell。这是要添加操作按钮的单元格。
- 一个名为 hlItem 的 HierarchyItem。这是我们为其创建操作按钮的项目。

确定传递给 AddActionItems() 方法的对象类型后，我们为当前对象添加类型特定的操作按钮。例如，如果传递了一个发布项，则会添加复制和移动按钮。

除了类型特定的选项外，还会向菜单中添加一个适用于所有对象的属性按钮。最后，我们将检查用户是否有权限删除当前对象，如果有，则添加删除按钮。

请注意，我们使用 PrepareUrl() 方法生成的 URL 将其分配给每个操作按钮的 NavigateUrl 属性。在 DataGrid1_EditCommand() 事件处理程序下方添加 AddActionItems() 方法：

```csharp
private void AddActionItems(TableCell actionCell, HierarchyItem hlItem)
{
    if (hlItem is Posting)
    {
        Posting currentPosting = hlItem as Posting;
        // actions for postings include:
        // Copy, Move, Create Connected Posting.

        // the copy option
        HyperLink hCopy = new HyperLink();
        hCopy.Text = "Copy<br>";
        hCopy.Target = "_blank";
        hCopy.NavigateUrl = PrepareUrl(hlItem, PublishingMode.Update,
            "CopyPosting.aspx");
        actionCell.Controls.Add(hCopy);

        // the move option
        if (currentPosting.CanMove)
        {
        }

        // add shared options include:
        // Properties and Delete.

        // the properties option
        HyperLink hProperties = new HyperLink();
        hProperties.Text = "Properties<br>";
        hProperties.Target = "_blank";
        hProperties.NavigateUrl = PrepareUrl(hItem, PublishingMode.Update,
                                    "Properties.aspx");

        actionCell.Controls.Add(hProperties);

        // the delete option
        if (hItem.CanDelete)
        {
            HyperLink hDelete = new HyperLink();
            hDelete.Text = "Delete<br>";
            hDelete.Target = "_blank";
            hDelete.NavigateUrl = PrepareUrl(hItem, PublishingMode.Update,
                                     "Delete.aspx");

            actionCell.Controls.Add(hDelete);
        }
    }
}
```

> 如果在测试上述代码时收到 JavaScript 错误消息，您可能需要将打开的表单标签中的 ID 更改为其他名称，例如 "CMSExplorerDefault"，因为浏览器可能不喜欢 Visual Studio .NET 分配给表单的 ID "default"。

保存并构建解决方案，然后导航到 **http://localhost/CmsExplorer**。此时，您可以通过点击频道名称浏览频道，并查看我们的操作菜单。

接下来我们需要的是一个工具栏，用于从当前选择的父容器中向上移动一级层级、刷新页面，以及选择除频道之外的其他根路径，例如模板或资源。

## 构建工具栏

网格顶部的灰色条是工具栏。它将由六个控件组成：

- 将用户带回频道层级上一级的向上按钮
- 更新显示的刷新按钮
- 包含创建新频道、发布、模板、模板库或资源的选项的 DropDownList
- 用于浏览可用频道和发布的 Channels 按钮
- 用于浏览可用模板库和模板的 Templates 按钮
- 用于浏览可用资源库和资源的 Resources 按钮

![](img/442e26bf28f2df81fbf8dff3478f88e3_45_0.png)

| StartItem 类型 | 添加的选项 |
|---|---|
| Channel | 新频道<br>新发布 |
| TemplateGallery | 新模板库<br>新模板 |
| ResourceGallery | 新资源 |

例如，如果 startItem 是频道并且用户有权在父频道中创建频道和发布，则会添加“新频道”和“新发布”选项。

在 ddlNewItem_SelectedIndexChanged() 下方添加以下 PrepareToolbar() 方法，该方法会在下拉列表中插入选项：

```csharp
private void PrepareToolbar()
{
    // remove any pre-existing options from the DropDownList
    ddlNewItem.Items.Clear();
    ddlNewItem.Items.Add(new ListItem("New",""));

    ListItem li = null;

    if (startItem is Channel)
    {
        Channel currentChannel = startItem as Channel;

        // if the user has rights to create channels, add option to create
        // a new channel
        if (currentChannel.CanCreateChannels)
        {
            li = new ListItem("New Channel","NewChannel");
            ddlNewItem.Items.Add(li);
        }

        // if the user has rights to create postings, add option to create
        // a new posting
        if (currentChannel.CanCreatePostings)
        {
            li = new ListItem("New Posting","NewPosting");
            ddlNewItem.Items.Add(li);
        }

        imgTitle.ImageUrl = "images/channelopen_big.gif";
    }
    else if (startItem is TemplateGallery)
    {
        TemplateGallery templateGallery = startItem as TemplateGallery;

        // if the user has rights to create template galleries, add option
        // to create a new template gallery
        if (templateGallery.CanCreateTemplateGalleries)
        {
            li = new ListItem("New Template Gallery",
                "NewTemplateGallery");
            ddlNewItem.Items.Add(li);
        }

        // if the user has rights to create templates, add option to create
        // a new template
        if (templateGallery.CanCreateTemplates)
```

```csharp
MyObject.GetType().GetMethods();
```

要获取其属性列表，可以调用：

```csharp
MyObject.GetType().GetProperties();
```

正如您所见，反射是一个强大的功能。在此示例中，而不是编写 40 多行代码来列出 ChannelItem 对象的属性，我们将简单地使用反射遍历每个属性并显示其值。

让我们在 DataGrid 中显示属性列表。首先，向 CMSExplorer 项目添加一个新的 Web 表单。将新 Web 表单命名为 Properties.aspx。切换到 HTML 视图并向表单添加几个标题以描述其功能：

```html
<form>
<h1>Properties of <asp:Literal Runat="server" ID="litCurrentItem"/></h1>
<h2>List of all Properties and their values</h2>
</form>
```

在 Web 表单的代码隐藏文件中，导入 Microsoft.ContentManagement.Publishing 和 System.Reflection 命名空间，并在 Page_Load() 事件处理程序中添加以下代码：

```csharp
. . . code continues . . .
// MCMS PAPI
using Microsoft.ContentManagement.Publishing;

// for reflection
using System.Reflection;

namespace CMSExplorer
{
    /// <summary>
    /// Summary description for Properties.
    /// </summary>
    public class Properties : System.Web.UI.Page
    {
        HierarchyItem hlItem; // the current item

        private void Page_Load(object sender, System.EventArgs e)
        {
            CmsHttpContext cmsContext = CmsHttpContext.Current;
            hlItem = null;

            string cmsObjectGuid = "";
            if (Request.QueryString["CMSObjectGuid"]!=null)
            {
                // template gallery items and resource gallery items
                cmsObjectGuid = Request.QueryString["CMSObjectGuid"];
                hlItem = cmsContext.Searches.GetByGuid(cmsObjectGuid);
            }
            else
            {
                // channels and postings
                hlItem = cmsContext.ChannelItem;
            }

            // list all properties and their values in the grid
            if (hlItem!=null)
            {
                litCurrentItem.Text = hlItem.Path;
                ListProperties();
            }
        }
    }
```


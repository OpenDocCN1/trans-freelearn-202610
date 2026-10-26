

## The SavePosting() Web Method

让我们再创建一个 Web 方法 SavePosting()，它将接受一个 LightweightPosting 对象作为参数并将其内容保存到 MCMS。将以下代码添加到 authoring.asmx.cs 文件中并重新生成解决方案：

```
/// <summary>
/// Save a lightweight posting object to MCMS
/// </summary>
/// <param name="_Posting">LightweightPosting object to save</param>
[WebMethod]
public void SavePosting(LightweightPosting _Posting)
{
    // Save the posting
    _Posting.Save();
}
```

SavePosting() Web 方法不能像我们对 GetPosting() 所做的那样从浏览器进行测试，因为它需要一个复杂数据类型（LightweightPosting）作为参数。我们将在稍后创建的 InfoPath 表单中通过 InfoPath 表单来测试 SavePosting()。

## Preparing the InfoPath Environment

此时，您需要在计算机上安装 Microsoft InfoPath 2003 和 Office 2003 Service Pack 1（SP1）。如果您的 Office 2003 安装尚未应用 SP1，您可以从以下位置下载：

http://www.microsoft.com/downloads/details.aspx?FamilyID=9c51d3a6-7cb1-4f61-837e-5f938254fc47

您还应从以下位置下载 InfoPath 2003 SDK 以供进一步参考：

http://www.microsoft.com/downloads/details.aspx?FamilyId=351F0616-93AA-4FE8-9238-D702F1BFBAB4

要验证是否已安装 Service Pack 1，请启动 InfoPath 并单击 Help | About Microsoft Office InfoPath。最新服务包的名称会打印在产品版本号旁边。

## Creating the InfoPath Document

InfoPath 文档在创建 InfoPath 表单模板并将其连接到 Web 服务以进行数据检索和提交之前，需要一个在 InfoPath 中设计的 InfoPath 模板（.xsn）。按照以下步骤创建一个将连接到我们刚刚创建的 Web 服务的 InfoPath 模板：

- 1. 启动 Microsoft InfoPath 2003。
- 2. 在 Fill Out a Form 窗口左侧，选择 Design a Form。如果 Fill Out a Form 窗口未打开，请选择 File | Design a Form。
- 3. 从右侧的任务窗格中，选择 New from Data Connection。
- 4. 在 Data Connection Wizard 中，选择 Web Service 并单击 Next。
- 5. 选择 Receive and submit data 并单击 Next。

现在让我们为此表单添加一个标题。单击 Click to add title 标签并输入 MCMS Authoring InfoPath Form。选择文本并使其加粗。

最后，我们将在标题下方输入一些简要说明。单击 Click to add form content 标签上的任意位置。在提供的空间中输入 This InfoPath form can retrieve and update existing MCMS postings。

## Creating the Controls in InfoPath

通过从 XML Web 服务生成文档，InfoPath 允许我们在几分钟内而不是在其他环境中花费数小时或数天创建一个功能齐全的用户界面。为了允许用户输入帖子的路径并查询 Web 服务，请按照以下步骤操作：

- 1. 在任务窗格中，选择 Data Source 类别。
- 2. 展开 `myFields`、`queryFields` 和 `s0:GetPosting`。
- 3. 将 `strPath` 元素拖到表单上标记为 "Drag query fields here." 的区域。
- 4. 将标签 `strPath` 更改为 "Posting To Load"（例如 `/Channels/www.TropicalGreen.net/PlantCatalog/AloeVera`）。
- 5. 将 Run Query 按钮拖到 `strPath` 文本框下方，并删除 Run Query 按钮之前多余的行中断。
- 6. 选择布局表的底部边框并将其向上拖动以减小表的高度。

![](img/442e26bf28f2df81fbf8dff3478f88e3_397_0.png)

我们的表单现在允许用户输入帖子的路径，该路径传递给 ASP.NET Web 服务，后者返回一个序列化的 LightweightPosting 对象。

让我们创建显示帖子属性并允许编辑的用户界面。

- 1. 在 Data Source 窗格中，展开 `dataFields` 和 `s0:GetPostingResponse`。
- 2. 将 `GetPostingResult` 节点拖到 "Drag data fields here" 标签上。
- 3. 将出现一个上下文菜单，列出可以执行的操作。对于此节点，我们将创建带控件的部分，因此单击 Section with Controls。
- 4. 现在已生成以下表单。现在不要担心布局，我们将在下一节中使其更易于使用。

![](img/442e26bf28f2df81fbf8dff3478f88e3_398_0.png)

![](img/442e26bf28f2df81fbf8dff3478f88e3_398_1.png)

## Cleaning Up the Generated Form

当我们选择 Section with Controls 时，InfoPath 生成的界面完全可用，但美观性不佳，也不包含所有正确的控件配置。我们可以通过简单地将控件拖放到不同区域来重新排列表单上的控件。

首先，让我们更好地排列所有常见的帖子和模板属性。

- 1. 将光标放在最上面的 Name: 标签左侧（标题正下方的标签），从 Table 菜单中选择 Insert | Layout Table。
- 2. 将列数设置为 2，行数设置为 7，然后单击 OK。
- 3. 现在已生成布局表。将以下文本添加到第一列的单独单元格中：
  - Name
  - Display Name
  - Posting Path
  - Posting ID
  - Template Name
  - Template Path
  - Template ID
- 4. 选择单元格并将文本设置为粗体。
- 5. 我们现在必须将生成的文本字段移动到其对应标签旁边。将 Name、Display Name、Posting Path 和 ID 标签下方的四个文本字段拖到表格右侧列的前四行中。然后删除多余的标签。
- 6. 与模板属性相关的文本框已在表单底部生成，因为它们由另一个 XML 元素表示。将这些文本框拖到其对应标签旁边的列中。您可以通过选择它们并查看右侧的 Data Source 窗格来轻松验证属性与哪个对象相关。顶部表格现在应如下所示：

![](img/442e26bf28f2df81fbf8dff3478f88e3_399_0.png)

从屏幕顶部的工具栏更新一些占位符值和自定义属性，然后按 Submit。如果您浏览到 http://www.tropicalgreen.net/PlantCatalog/AloeVera.htm，更新现在应该可见——您可能需要切换到 Unpublished Mode，因为更改需要在上线前获得编辑批准。完成后，单击屏幕顶部的 Close Preview，表单将返回设计模式。

## Saving the InfoPath Form

InfoPath 模板开发完成后，可以通过将完成的表单发布到 SharePoint、Web 服务器或网络共享等共享位置来分发。或者模板也可以保存到文件系统以便稍后修改。

要将模板保存到本地文件系统，请选择 File | Save，然后在对话框中选择 Save。然后只需输入或浏览到合适的[content]

## The SavePosting() Web Method

让我们再创建一个 Web 方法 SavePosting()，它将接受一个 LightweightPosting 对象作为参数并将其内容保存到 MCMS。将以下代码添加到 authoring.asmx.cs 文件中并重新生成解决方案：

```
/// <summary>
/// Save a lightweight posting object to MCMS
/// </summary>
/// <param name="_Posting">LightweightPosting object to save</param>
[WebMethod]
public void SavePosting(LightweightPosting _Posting)
{
    // Save the posting
    _Posting.Save();
}
```

SavePosting() Web 方法不能像我们对 GetPosting() 所做的那样从浏览器进行测试，因为它需要一个复杂数据类型（LightweightPosting）作为参数。我们将在稍后创建的 InfoPath 表单中通过 InfoPath 表单来测试 SavePosting()。

## Preparing the InfoPath Environment

此时，您需要在计算机上安装 Microsoft InfoPath 2003 和 Office 2003 Service Pack 1（SP1）。如果您的 Office 2003 安装尚未应用 SP1，您可以从以下位置下载：

http://www.microsoft.com/downloads/details.aspx?FamilyID=9c51d3a6-7cb1-4f61-837e-5f938254fc47

您还应从以下位置下载 InfoPath 2003 SDK 以供进一步参考：

http://www.microsoft.com/downloads/details.aspx?FamilyId=351F0616-93AA-4FE8-9238-D702F1BFBAB4

要验证是否已安装 Service Pack 1，请启动 InfoPath 并单击 Help | About Microsoft Office InfoPath。最新服务包的名称会打印在产品版本号旁边。

## Creating the InfoPath Document

InfoPath 文档在创建 InfoPath 表单模板并将其连接到 Web 服务以进行数据检索和提交之前，需要一个在 InfoPath 中设计的 InfoPath 模板（.xsn）。按照以下步骤创建一个将连接到我们刚刚创建的 Web 服务的 InfoPath 模板：

- 1. 启动 Microsoft InfoPath 2003。
- 2. 在 Fill Out a Form 窗口左侧，选择 Design a Form。如果 Fill Out a Form 窗口未打开，请选择 File | Design a Form。
- 3. 从右侧的任务窗格中，选择 New from Data Connection。
- 4. 在 Data Connection Wizard 中，选择 Web Service 并单击 Next。
- 5. 选择 Receive and submit data 并单击 Next。

现在让我们为此表单添加一个标题。单击 Click to add title 标签并输入 MCMS Authoring InfoPath Form。选择文本并使其加粗。

最后，我们将在标题下方输入一些简要说明。单击 Click to add form content 标签上的任意位置。在提供的空间中输入 This InfoPath form can retrieve and update existing MCMS postings。

## Creating the Controls in InfoPath

通过从 XML Web 服务生成文档，InfoPath 允许我们在几分钟内而不是在其他环境中花费数小时或数天创建一个功能齐全的用户界面。为了允许用户输入帖子的路径并查询 Web 服务，请按照以下步骤操作：

- 1. 在任务窗格中，选择 Data Source 类别。
- 2. 展开 `myFields`、`queryFields` 和 `s0:GetPosting`。
- 3. 将 `strPath` 元素拖到表单上标记为 "Drag query fields here." 的区域。
- 4. 将标签 `strPath` 更改为 "Posting To Load"（例如 `/Channels/www.TropicalGreen.net/PlantCatalog/AloeVera`）。
- 5. 将 Run Query 按钮拖到 `strPath` 文本框下方，并删除 Run Query 按钮之前多余的行中断。
- 6. 选择布局表的底部边框并将其向上拖动以减小表的高度。

![](img/442e26bf28f2df81fbf8dff3478f88e3_397_0.png)

我们的表单现在允许用户输入帖子的路径，该路径传递给 ASP.NET Web 服务，后者返回一个序列化的 LightweightPosting 对象。

让我们创建显示帖子属性并允许编辑的用户界面。

- 1. 在 Data Source 窗格中，展开 `dataFields` 和 `s0:GetPostingResponse`。
- 2. 将 `GetPostingResult` 节点拖到 "Drag data fields here" 标签上。
- 3. 将出现一个上下文菜单，列出可以执行的操作。对于此节点，我们将创建带控件的部分，因此单击 Section with Controls。
- 4. 现在已生成以下表单。现在不要担心布局，我们将在下一节中使其更易于使用。

![](img/442e26bf28f2df81fbf8dff3478f88e3_398_0.png)

![](img/442e26bf28f2df81fbf8dff3478f88e3_398_1.png)

## Cleaning Up the Generated Form

当我们选择 Section with Controls 时，InfoPath 生成的界面完全可用，但美观性不佳，也不包含所有正确的控件配置。我们可以通过简单地将控件拖放到不同区域来重新排列表单上的控件。

首先，让我们更好地排列所有常见的帖子和模板属性。

- 1. 将光标放在最上面的 Name: 标签左侧（标题正下方的标签），从 Table 菜单中选择 Insert | Layout Table。
- 2. 将列数设置为 2，行数设置为 7，然后单击 OK。
- 3. 现在已生成布局表。将以下文本添加到第一列的单独单元格中：
  - Name
  - Display Name
  - Posting Path
  - Posting ID
  - Template Name
  - Template Path
  - Template ID
- 4. 选择单元格并将文本设置为粗体。
- 5. 我们现在必须将生成的文本字段移动到其对应标签旁边。将 Name、Display Name、Posting Path 和 ID 标签下方的四个文本字段拖到表格右侧列的前四行中。然后删除多余的标签。
- 6. 与模板属性相关的文本框已在表单底部生成，因为它们由另一个 XML 元素表示。将这些文本框拖到其对应标签旁边的列中。您可以通过选择它们并查看右侧的 Data Source 窗格来轻松验证属性与哪个对象相关。顶部表格现在应如下所示：

![](img/442e26bf28f2df81fbf8dff3478f88e3_399_0.png)

从屏幕顶部的工具栏更新一些占位符值和自定义属性，然后按 Submit。如果您浏览到 http://www.tropicalgreen.net/PlantCatalog/AloeVera.htm，更新现在应该可见——您可能需要切换到 Unpublished Mode，因为更改需要在上线前获得编辑批准。完成后，单击屏幕顶部的 Close Preview，表单将返回设计模式。

## Saving the InfoPath Form

InfoPath 模板开发完成后，可以通过将完成的表单发布到 SharePoint、Web 服务器或网络共享等共享位置来分发。或者模板也可以保存到文件系统以便稍后修改。

要将模板保存到本地文件系统，请选择 File | Save，然后在对话框中选择 Save。然后只需输入或浏览到合适的

## Possible Enhancements to this Solution

以下是一些可以轻松纳入此解决方案的可能增强功能：

- 通过选择模板创建新帖子的能力。
- 对 Selection Type 自定义属性的支持。
- 对 HtmlPlaceholder 以外的占位符对象支持。
- 支持将图像粘贴到富文本框中，该图像必须解码并上传到资源库。
- 保存以便稍后修改的能力。
- InfoPath 表单中的另一个视图，允许用户列出和选择他们想要修改的帖子。

## Summary

在本章的整个过程中，我们见证了 Microsoft Office InfoPath 2003 的强大功能，我们迅速创建了一个灵活的内容创作解决方案，利用其丰富的 UI。与此同时，我们能够在解决方案的各层之间保持标准的数据表示。

在这一过程中，我们探索了 ASP.NET Web 服务，特别是如何创建一个可序列化为 XML 的 .NET 对象模型，以便通过 ASP.NET Web 服务发送和接收。

我们使用 InfoPath 生成了一个表单，并将此表单绑定到我们的 ASP.NET Web 服务。InfoPath 表单的布局是从 Web 服务定义自动生成的，重新排列生成的界面非常简单。测试表单也易如反掌，正如我们在检索示例帖子、进行一些修改并提交更改时所看到的那样。


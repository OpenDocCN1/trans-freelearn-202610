

+   4. 将页面布局更改为 **FlowLayout**。
+   5. 从解决方案资源管理器将 /Styles/styles.css、/UserControls/TopMenu.ascx 和 /UserControls/RightMenu.ascx 文件拖放到设计器上。
+   6. 切换到 HTML 模式并按如下方式修改 body 标签：
+   `<body topmargin="0" leftmargin="0" rightmargin="0">`
+   7. 在 `<form>` 和 `</form>` 标签之间添加以下代码，替换刚刚添加的两个用户控件：
+   ```
+   <form id="Form1" method="post" runat="server">
+   <table width="100%" border="0" cellspacing="0" cellpadding="0">
+   <tr>
+   <td width="100%" colspan="2" valign="top" bgcolor="#ffc00">
+   <img src="/tropicalgreen/images/Logo.gif">
+   </td>
+   <td vAlign="top" rowSpan="10">&nbsp;</td>
+   </tr>
+   <tr bgColor="#66cc33">
+   <td colspan="2"><uc1:TopMenu id="TopMenu1" runat="server"></uc1:TopMenu></td>
+   </tr>
+   <tr>
+   <td vAlign="top" style="PADDING-RIGHT:30px; PADDING-LEFT:30px; PADDING-BOTTOM:30px;">
+   <p>&nbsp;</p>
+   <table cellspacing="0" cellpadding="10" border="1" bordercolor="#669900">
+   <tr vAlign="top">
+   <td>Tropical Green Search:</td>
+   </tr>
+   <tr>
+   <td vAlign="top">
+   </td>
+   </tr>
+   </table>
+   </td>
+   <td class="RightMenuBar" width="20%" valign="top" height="100%" align="center" rowspan="2" bgcolor="#669900">
+   <uc1:RightMenu id="RightMenu1" runat="server"></uc1:RightMenu>
+   </td>
+   </tr>
+   </table>
+   </form>
+   ```

## 为什么我们要将用户控件拖到页面上然后再替换生成的 HTML？

将用户控件拖到页面上会为我们自动添加 ASPX 中的 `<%@ Register %>` 行，以及 HTML 中的用户控件 ASP.NET 标签。然后我们只需要修改 HTML 使其更美观即可。

## 使用 SharePoint 搜索 MCMS

现在你在设计模式下查看的页面应该如下所示：

![](img/442e26bf28f2df81fbf8dff3478f88e3_152_0.png)

让我们保存新的搜索页面，构建 Tropical Green 项目，并在浏览器中导航到它，以确保一切正常，然后再添加搜索输入和结果控件。

+   1. 保存 search.aspx 页面的所有更改。
+   2. 右键单击 TropicalGreen 项目并选择 Build。
+   3. 如果构建没有错误，打开浏览器并导航到：http://www.tropicalgreen.net/TropicalGreen/Search.aspx。
+   4. 如果有任何问题，重新检查我们到目前为止采取的步骤，解决错误，然后重试该 URL。

现在我们已经有一个可用的搜索页面，我们需要为其添加一些功能。我们将添加两个 MCMS Connector 服务器控件，进行一些配置更改，构建解决方案并测试我们的搜索页面。

+   1. 如果尚未打开，请在 Visual Studio .NET 中打开 search.aspx 页面，并切换到设计视图。
+   2. 打开 Visual Studio .NET 工具箱并将 SearchInputControl 和 SearchResultsControl 拖入 Tropical Green Search 单元格下方的表格单元格中。参考以下图片了解放置位置：

![](img/442e26bf28f2df81fbf8dff3478f88e3_153_0.png)

3. 选择我们添加到 search.aspx 的 SearchInputControl 并在 Visual Studio .NET 属性窗口中设置以下属性：

| 属性 | 值 |
|---|---|
| SearchMode | Simple |
| SearchResultPage | /TropicalGreen/Search.aspx |

4. 选择我们添加到 search.aspx 页面的 SearchResultControl 并使用 Visual Studio .NET 属性窗口设置以下属性：

| 属性 | 值 |
|---|---|
| PortalUrl | http://portal.tropicalgreen.net/ |
| SearchResultPageSize | 10 |

> 我们使用的是附录 A 中创建的门户的 URL。请用你在本章前面步骤中为内容源和搜索组配置的门户 URL 替换此 URL。

## 使用 SharePoint 搜索 MCMS

让我们看看我们的搜索是否正常工作。保存 search.aspx 的所有更改，构建 Tropical Green 项目，并在浏览器中访问 http://www.tropicalgreen.net/TropicalGreen/Search.aspx。你应该会看到如下类似的页面：

![](img/442e26bf28f2df81fbf8dff3478f88e3_154_0.png)

输入一个你知道会在网站上找到的词，例如 ficus。你会看到与在包含内容索引的门户中搜索同一字符串时返回的搜索结果列表相同。

![](img/442e26bf28f2df81fbf8dff3478f88e3_154_1.png)

如果收到错误消息“加载输入控件时出现问题。系统返回的错误是：找不到路径的一部分 c:\inetpub\wwwroot\tropicalgreen\cms\wssintegration\searchpropertycollection.xml”，请仔细检查你是否在 TropicalGreen Web 应用程序中添加了 CMS 虚拟目录。

此时，我们的网站已通过 MCMS Connector 控件和 SPS 的搜索功能获得了搜索能力。但此解决方案非常有限，例如无法更改这些控件的外观和感觉，也无法配置结果页面中显示的属性，例如为返回的文档显示简短描述。

为了解决这个问题，我们现在将构建自己的搜索控件。

## 构建自定义搜索实现

如前所述，MCMS Connector 搜索控件有优点也有缺点。最明显的是 SearchResultControl 不允许我们配置 SPS 搜索返回的结果。我们现在将构建自己的搜索实现，该实现将利用 SPS 搜索 Web 服务，为用户提供高级和专门的搜索功能，并以可自定义的方式呈现结果。

### 关于 SharePoint Portal Server 查询服务

我们即将构建的一切都依赖于 SPS 中包含的查询服务 Web 服务，该服务向远程客户端（如我们的网站）公开搜索功能。此 Web 服务接受 Microsoft.Search.Query XML 格式的请求，并以 Microsoft.Search.Response XML 格式返回响应。为了构建健壮的解决方案，我们提交的请求将使用 Microsoft SQL 语法进行全文搜索。查询服务提供的一种方法是 QueryEx，我们将使用它，因为它以 DataSet 的形式返回结果。

> 有关 Microsoft SharePoint Portal Server 查询服务 Web 服务的更多信息和文档，请参阅 MSDN 文档：
> http://msdn.microsoft.com/library/default.asp?url=/library/en-us/spptsdk/html/cPSQueryService_SV01004360.asp。

### 构建搜索输入控件

我们首先要做的是构建一个搜索输入控件，该控件将向页面提交搜索查询以进行处理。此实现将使我们能够快速地将小型搜索组件添加到所有模板中。在提交搜索查询后，我们的用户控件将把查询参数添加到查询字符串并将请求重定向到结果页面。

让我们先开始创建一个新的用户控件。

+   1. 在 Visual Studio .NET 中，右键单击 Tropical Green 项目中的 User Controls 文件夹，并选择 Add | Add Web User Control。
+   2. 将新控件命名为 SearchInput.ascx。
+   3. 在设计视图中，从工具箱拖放控件到 Web 表单上并按如下所示排列：

| 控件 | 属性 |
|---|---|
| TextBox | ID = txtSearchInput |
| Button | ID = btnExecuteSearch<br>Text = Go |
| LinkButton | ID = lnkAdvancedSearch<br>Text = advanced search options |

![](img/442e26bf28f2df81fbf8dff3478f88e3_156_0.png)

4. 我们创建的 LinkButton 将把用户带到搜索结果页面，我们稍后将为其添加一些高级搜索功能。双击我们的 LinkButton。Visual Studio .NET 将为 Click() 事件创建一个空事件处理程序。向此空事件处理程序添加一行代码以将用户重定向到搜索结果页面：

```
private void lnkAdvancedSearch_Click(object sender, System.EventArgs e)
{
    Response.Redirect(Request.ApplicationPath + "/SearchResults.aspx");
}
```

5. 接下来，我们需要为用户单击 Go 按钮时创建一个事件处理程序。我们将获取 TextBox 中输入的关键字并将搜索请求发送到搜索结果页面。双击 Go 按钮并将以下代码添加到事件处理程序中：

```
private void btnExecuteSearch_Click(object sender, System.EventArgs e)
{
    string keywords = this.txtSearchInput.Text;
    keywords = HttpUtility.UrlEncode(keywords);
    Response.Redirect(Request.ApplicationPath
        + "/SearchResults.aspx?keywords="
        + keywords);
}
```

让我们看看我们新的搜索输入控件是否一切正常。保存更改并构建项目。如果收到任何错误消息，请重新检查步骤并确保没有拼写错误。

在使用此控件之前，我们需要将其添加到现有模板中。虽然我们理想情况下希望在网站的所有页面上提供搜索功能（通常通过将其添加到全局页眉控件），但我们现在只将其添加到主页。

+   1. 打开 \Templates\HomePage.aspx 模板并将我们新的 SearchInput.ascx 拖到顶部单元格中，位于 logo 的右侧。
+   2. 切换到 HTML 视图并找到我们刚刚添加的控件。它很可能具有 uc1:SearchInput 的开始标签。用 HTML DIV 包裹此控件并将其对齐方式设置为右对齐，如以下代码所示：

```
<td width="100%" colspan="2" valign="top" bgcolor="#ffcc00">
    <img src="/tropicalgreen/images/Logo.gif">&nbsp;
    <div align="right">
        <uc1:SearchInput id="SearchInput1" runat="server"></uc1:SearchInput>
    </div>
</td>
```


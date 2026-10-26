

+   1. 右键单击项目并选择 Add | Add Web Form。
+   2. 将新页面的名称设为 SearchResults.aspx。
+   3. 在 Design 视图中，从 Solution Explorer 将 Styles.css 文件拖放到表单上以将样式表应用到页面。
+   4. 将页面布局更改为 FlowLayout。
+   5. 将以下用户控件拖到设计器中：
+      - \UserControls\TopMenu.ascx
+      - \UserControls\RightMenu.ascx
+   6. 切换到 HTML 视图并按如下方式修改 body 标签：
+      <body topmargin="0" leftmargin="0">
+   7. 在 <form> 标签之间向页面添加以下 HTML 代码，替换我们刚刚添加的两个控件：

```
<form id="Form1" method="post" runat="server">
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tr>
<td width="100%" colspan="2" valign="top" bgcolor="#ffcc00">
<img src="/tropicalgreen/images/Logo.gif">
</td>
<td valign="top" rowspan="10">
</td>
</tr>
<tr bgcolor="#66cc33">
<td colspan="2">
<uc1:TopMenu id="TopMenu1" runat="server">
</uc1:TopMenu>
</td>
</tr>
<tr>
<td valign="top" style="PADDING-RIGHT:30px; PADDING-LEFT:30px; PADDING-BOTTOM:30px; PADDING-TOP:10px">
<p>&nbsp;</p>
<table cellspacing="0" cellpadding="10" border="1" bordercolor="#669900">
<tr valign="top">
<td>
<b>Tropical Green Search:</b>
</td>
</tr>
<tr>
<td valign="top">
<b>Advanced Search</b>
<p>
<b>Search Results</b>
</td>
</tr>
</table>
</td>
<td class="RightMenuBar" width="20%" valign="top" height="100%" align="center" rowspan="2" bgcolor="#669900">
<uc1:RightMenu id="RightMenu1" runat="server">
</uc1:RightMenu>
</td>
</tr>
</table>
</form>
```

我们现在有了高级搜索和搜索结果页面的基本布局，其外观与我们网站中的其他模板类似。让我们为高级搜索添加一些控件。

+   1. 在 Design 视图中，从 Toolbox 拖动一个 TextBox 并将其放置在 Advanced Search 文本下方。
+   2. 在 Design 视图中，从 Toolbox 拖动一个 Button 并将其放置在 TextBox 的右侧。
+   3. 我们需要添加的下一件事是包含搜索结果的 DataGrid。在 Design 视图中，从 Toolbox 拖动一个 DataGrid 控件到 Add Search Results Here 文本正下方。我们稍后再担心格式化此控件，现在我们只需要一些东西来显示我们的数据。
+   4. 根据以下表格设置我们刚刚添加的控件的属性：

| Property | Value |
|---|---|
| TextBox | ID = txtAdvancedSearch |
| Button | ID = btnAdvancedSearch<br>Text = Go |
| DataGrid | ID = dgrSearchResults |

我们的高级搜索页面现在应该如下所示：

![](img/442e26bf28f2df81fbf8dff3478f88e3_159_0.png)

现在是开始编写搜索逻辑的时候了。首先，我们需要为高级搜索按钮添加一个事件处理程序。

+   1. 在 Design 视图中，双击 btnAdvancedSearch 按钮以创建单击事件处理程序。Visual Studio .NET 将在代码隐藏文件中添加一个事件处理程序方法。
+   2. 将以下代码添加到 btnAdvancedSearch_Click() 事件处理程序中：

```
private void btnAdvancedSearch_Click(object sender, System.EventArgs e)
{
    string keywords = this.txtAdvancedSearch.Text;
    keywords = HttpUtility.UrlEncode(keywords);
    Response.Redirect(Request.ApplicationPath
        + "/SearchResults.aspx?keywords="
        + keywords);
}
```

接下来，我们需要在 Page_Load() 事件处理程序中检查 querystring，以查看是否从 SearchInput.ascx 控件或 txtAdvancedSearch TextBox 传递了任何关键字。

添加以下代码以检查是否提供了任何关键字并在提供时执行搜索：

```
private void Page_Load(object sender, System.EventArgs e)
{
    if (Request.QueryString["keywords"] != null
        && Request.QueryString["keywords"] != String.Empty)
    {
        string keywords = Request.QueryString["keywords"];
        DataSet ds = ExecuteSearch(keywords);
        this.dgrSearchResults.Visible = true;
        this.dgrSearchResults.DataSource = ds;
        this.dgrSearchResults.DataBind();

        // autofill the keyword input box with the search keywords
        this.txtAdvancedSearch.Text = keywords;
    }
    else
    {
        this.dgrSearchResults.Visible = false;
    }
}
```

现在我们需要创建将对 SPS 内容索引执行搜索的方法。此方法将：

+   - 创建我们刚刚添加到项目的 Query Service Web Service 的实例。
+   - 调用将构建要提交给 Query Service 的 MSQuery 的方法。
+   - 执行搜索。
+   - 将搜索结果绑定到 DataGrid。

在 SearchResults.aspx.cs 文件中导入以下命名空间：

```
using System.Security.Principal;
using System.Runtime.InteropServices;
using System.Net;
using System.Text;
using System.Text.RegularExpressions;
using Microsoft.ContentManagement.Publishing;
```

将以下方法添加到 SearchResults.aspx.cs 文件中 Page_Load() 事件处理程序之后。此方法将确保当前线程在任何可能之前调用的模拟下仍在原始安全上下文下运行：

```
```csharp
// Get reference to the RevertToSelf method
[DllImport("ADVAPI32.DLL")]
public static extern int RevertToSelf();

/// <summary>
/// Builds the appropriate MSQuery,
/// submits the query to the SPS Query Service,
/// and returns the results as a DataGrid.
/// </summary>
/// <param name="keywords">String of keywords to search for.</param>
/// <returns>DataSet of search results.</returns>
private DataSet ExecuteSearch(string keywords)
{
    // decode the list of keywords
    keywords = HttpUtility.UrlDecode(keywords);

    // create reference to the Query Service Web service
    net.tropicalgreen.portal.QueryService spsQueryService =
        new net.tropicalgreen.portal.QueryService();

    // use the current application pool identity to login
    // to the SharePoint Query Service Web service
    WindowsIdentity CurrentUser = WindowsIdentity.GetCurrent();

    try
    {
        // use the Application Pool account to do access the
        // SharePoint Search Services
        RevertToSelf();
        spsQueryService.Credentials = CredentialCache.DefaultCredentials;
    }
    catch(System.Exception exception)
    {
        throw new System.Exception(exception.Message);
    }
    finally
    {
        // ensure that the original user is being impersonated again
        CurrentUser.Impersonate();
        CurrentUser = null;
    }

    // build MSQuery XML string to send to the Query Service
    // - change the content source to "CMSChannels" if you used SearchSetup.exe
    string msQuery = BuildMSQuery(keywords, "Tropical Green website");

    // execute the query and return the dataset
    return spsQueryService.QueryEx(msQuery);
}
```
```

> 如果您使用 SearchSetup.exe 程序创建了内容源，则应将代码中“ Tropical Green website”改为内容源组“CMSChannels”。

我们的 ExecuteSearch() 方法调用另一个名为 BuildMSQuery() 的方法，该方法构造要发送到 QueryEx() Web 方法的 MSQuery。MSQuery 由 XML 标签组成，这些标签为 Query Service 提供指令，例如请求中返回的结果数

和一个 Microsoft SQL 全文 (MSSQLFT) 查询。构建 MSSQLFT 查询和 MSQuery 可能是实现 SharePoint 搜索中最复杂的部分。我们将其分为两个任务：构建实际的 MSSQLFT 查询和构建 MSQuery XML 字符串。我们将首先构建 MSQuery 将在构造发送到 Query Service 的 XML 字符串时使用的全文查询。

## 构建 Microsoft SQL 全文查询

SPS 的搜索过程使用全文索引和查询来快速查找关键字，以便及时响应最终用户。全文查询与 Microsoft SQL Server 中的常规 T-SQL 查询非常相似，但您可以使用额外的函数或谓词来增强查询的功能。其中一个在搜索 SharePoint 索引时有用的谓词是 FREETEXT。FREETEXT 接受由空格分隔的单词列表，确定哪些单词和短语是重要的，并使用该信息构建内部查询以高效地搜索目标数据。

首先，您需要了解查询中可用的各种字段或属性。SharePoint 在门户的 Site Settings 管理页面中提供了此信息列表。

+   1. 打开 Internet Explorer 的新实例并导航到我们的门户：http://portal.tropicalgreen.net。
+   2. 单击右上角的 Site Settings 链接。
+   3. 在 Search Settings and Indexed Content 部分下，单击 Manage properties from crawled documents 链接。

对于每个被抓取的文档，Manage Properties of Crawled Content 页面列出了 SharePoint 可能包含索引数据的所有属性。对于我们的目的，我们只会查看以下两个字段，因为它们包含我们搜索结果页面的信息：

+   - DAV:href
+   - DAV:getlastmodified

![](img/442e26bf28f2df81fbf8dff3478f88e3_162_0.png)


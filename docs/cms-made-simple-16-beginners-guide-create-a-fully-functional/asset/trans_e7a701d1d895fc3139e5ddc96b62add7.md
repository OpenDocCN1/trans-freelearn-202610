

## Time for action – adding custom fields

+   1. 在管理字段中，打开 **Content | News** 并选择 **Field Definitions** 选项卡。
+   2. 点击 **Add Field Definition** 并输入字段，如以下截图所示：

![](img/b21e3dc5c94405bc7b3f102758ea770c_133_0.png)

+   3. 点击 **Submit**。
+   4. 打开你最近添加的文章条目，例如 **Marketing Manager (Content | News)**，你会在页面底部找到新的字段 **Location**。
+   5. 在 **Location** 字段中输入 **Washington**，然后点击 **Submit**。
+   6. 打开你的页面，你可以在新闻文章下方找到添加的位置。 （注意：这仅适用于 *Sample* 模板或基于默认模板的模板！）。它应与以下截图类似：

![](img/b21e3dc5c94405bc7b3f102758ea770c_133_1.png)

## What just happened?

你已为新闻文章添加了一个新的自定义字段。由于该字段标记为公开，它将在网站上显示。该字段会自动添加到新闻编辑窗口。如果你想更改字段的位置，应编辑模板。在默认模板 Sample 中，所有附加字段都添加到新闻文章底部。在你自己的模板中，可以使用以下代码行获取任何自定义字段值：

```
Location: {$entry->fieldsbyname.location->value}
```

上一行将显示保存在 Location 字段中的值。你可以在摘要和详细模板中使用此行。

还有更多参数可用于优化 Smarty 插件 {news} 的输出。对于每个模块，参数列表可以在模块的帮助部分找到。在管理控制台中，打开 Content | News，然后点击模块名称右侧的 Module Help 链接。

新闻模块有一些有用的参数，如下表所示：

| News parameter | Description |
|---|---|
| browsecat="1" | 显示可浏览的分类列表。 |
| showall="1" | 显示所有文章，不受结束日期限制。 |
| showarchive="0" | 仅显示已过期的新闻文章。 |
| sortasc="true" | 按升序日期顺序而非降序排序新闻项目。 |
| sortby="news_title" | 按字段排序。选项为：news_date, summary, news_data, news_category, news_title, news_extra, end_time, start_time, 和 random。默认值为 news_date。 |
| moretext="more..." | 如果 Summary 字段不为空，则在新闻项目末尾显示的文本。默认设置为 "more..." |
| start="0" | 从给定项目号开始列出文章。 |

![](img/b21e3dc5c94405bc7b3f102758ea770c_134_0.png)

如果你想在除主模板以外的其他页面模板中显示新闻文章的详细视图，请创建一个新的页面模板（Layout | Templates），创建一个新页面，并将新模板分配给该页面。使用 {news page="alias"} 调用 News 模块，其中 alias 是应显示每篇新闻项目详细视图的页面的名称。

你甚至可以允许网站访问者向你的模块添加新闻文章，如果你安装 **FrontEndUsers** 模块，该模块将在第 7 章 *Using Third-party Modules* 中描述。网站访问者提交的新闻文章将保存为草稿，你可以在管理内容页面中批准后发布它们。

## Using the news title as the page title

注意在显示完整文章的页面上浏览器标题栏的标题。你会发现标题标签对每个项目都相同，因为只有一个真实页面用于显示每篇新闻文章。然而，搜索引擎优化要求每个页面都有唯一的标题。你可以用一个小技巧自定义生成的页面的标题。打开管理控制台中的 **Content | News**，并选择 **Detail Templates** 选项卡。在模板的最顶部，添加以下行：

```
{assign var="pagetitle" value=$entry->title|escape}
```

此行将生成 Smarty 变量 {$pagetitle}，其中包含新闻文章的标题。要将变量添加到主模板，请打开它，并在模板的最顶部添加标签 {process_pagedata} 并用 IF 结构替换标题标签，如以下代码片段所示：

```
{process_pagedata}
<!DOCTYPE html>
<html>
<head>
{if isset($pagetitle) && !empty($pagetitle)}
<title>{title} - {sitename}</title>
{else}
<title>{sitename} - {title}</title>
{/if}
...
```

这段代码检查变量 {$pagetitle} 是否已定义且不为空。如果变量存在，则新闻标题用于标题标签并显示在浏览器标题栏中。如果不存在，则采用普通的页面标题。

## Using the search function with the module Search

接下来应解释的标准功能是 **Search** 模块。它默认安装，允许网站访问者搜索网站上的内容页面和新闻文章。通常，你将搜索字段添加到模板中而不是每个页面。

让我们将搜索功能添加到网站顶部的模板中。

## Time for action – adding a search form

+   1. 打开模板 **Business World (Layout | Templates)**
+   2. 将 Smarty 标签 `{search}` 添加到侧边栏，如下所示：

```
<!DOCTYPE html>
<html>
<head>
<title>{title} - {sitename}</title>
{stylesheet}
{metadata}
<meta name="description" content="" />
</head>
<body>
<div id="container">
<div id="header">
 businessWorld
</div>
<div id="top-navi">
 {menu number_of_levels="1" template="minimal_menu.tpl"}
</div>
<div id="content">
<h1>{title}</h1>
{content}
{if $page_alias neq "contact-us"}
<p>Last modified by {last_modified_by format="fullname"}</p>
{/if}
</div>
<div id="sidebar">
{search}
{menu start_level="2" template="minimal_menu.tpl"}
<hr />
{news summarytemplate="NewsList"}
</div>
<div id="footer">
<p>{menu template="footer_menu"}</p>
2009 businessWorld
</div>
</div>
</body>
</html>
```

3. 点击 Submit 并在浏览器中查看你的页面，它应如下图所示：

![](img/b21e3dc5c94405bc7b3f102758ea770c_137_0.png)

## What just happened?

你已将搜索功能添加到你的网站。如果你尝试在页面内容或新闻文章中搜索，它开箱即用。

你可以在管理控制台中定义停用词列表（Extensions | Search | Options）。这些词从搜索索引中排除，并且还为你提供了常用停用词的预定义列表。通常，你会添加公司名称或可以在每个页面或模板中找到的任何词。为了避免这种情况，搜索功能会显示搜索结果中的所有页面。你可以在列表末尾用逗号分隔添加自定义词或定义自己的列表。保存停用词列表后，不要忘记点击 Reindex All Content 按钮。它将为你的网站创建新的搜索索引。

如果你的网站是英文的，你可以使用词干搜索。这意味着搜索索引中的词被简化为其词干、词根或基本形式。例如，你有一个内容页面，其中找到并索引了词 steps。然而，网站访问者搜索 step。在未激活词干功能的情况下，找不到任何内容，因为字面上的 step 与 steps 不同。使用词干搜索，所有搜索词的形式都会被考虑，如 "step"、"steps" 甚至 "stepped"。

排除搜索中的页面
如果你想让某些页面，如 Policy 或 Disclaimer 从搜索中排除，请在管理控制台中打开你想排除的页面（Content | Pages）并选择 Options 选项卡。取消选择字段 This page is searchable，然后保存页面。现在即使搜索词出现在该页面上，该页面也不会被找到。

你可以在管理控制台中自定义搜索模板（搜索字段本身）和搜索结果模板（Extensions | Search）。与往常一样，你使用 HTML 和 Smarty 标签的混合来更改元素的顺序。使用 stylesheet 使其符合你的布局。

如果你想在与主模板不同的模板中输出搜索结果，你必须自己创建所需的模板（你已在上一章学习了如何创建模板）并将模板分配给应显示搜索结果的页面。此页面的别名应作为参数传递给 resultpage，如示例所示。修改 Smarty 标签 {search}，添加 resultpage 参数如下：

```
{search resultpage="search-result"}
```

你必须将之前创建的页面的别名分配给 resultpage 参数，以便搜索结果始终显示在此页面上。

搜索功能还会跟踪你网站上的搜索活动，并让你了解网站访问者需要或缺少什么。你可以跟踪单个搜索词或短语，以查看它们被访问者使用的频率。在 Search 模块的 Options 选项卡（Extensions | Search）中，你可以选择 Track Search Phrases, not Individual Words 以启用短语统计。

CMS Made Simple 的 Search 模块不如大型搜索引擎那么复杂。如果你想在网站上显示搜索词的文本并对英语以外的语言具有词干功能，可以使用自定义 Google 搜索。但如果你的网站未在 Google 上索引，那么你别无选择，只能使用 CMS Made Simple 的标准搜索功能。

## Browsing files with File Manager

你可以使用 File Manager 在 CMS Made Simple 中上传任何文件。File Manager 就像一个内置的 FTP 浏览器，你可以在其中上传、移动、删除和复制文件或目录。在管理控制台中，打开 Content | File Manager，找到保存在你网络托管的 uploads 文件夹中的所有文件列表。

在 Upload files 选项卡上，你可以上传多个文件，甚至是上传后在此文件夹中解压的归档文件。

在 Settings 选项卡上，你可以启用高级模式，这将让你访问 CMS Made Simple 网站的整个文件结构，以便 File Manager 不再限制在 uploads 文件夹。谨慎使用此设置，因为你可以轻松移动和删除 CMS Made Simple 的核心文件，从而损坏整个网站。

如果你想上传和编辑图像，则应使用 Image Manager。


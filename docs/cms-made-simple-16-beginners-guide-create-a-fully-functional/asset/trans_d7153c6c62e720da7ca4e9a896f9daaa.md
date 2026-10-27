

# 设计与布局

在 Smarty 中还可以使用更多限定符来构建逻辑条件。其中一些列在下表中：

| 限定符 | 含义 | 示例 |
|---|---|---|
| `eq` | 等于 | `$page_alias eq "contact-us"` |
| `neq` | 不等于 | `$page_alias neq "contact-us"` |
| `gt` | 大于 | `$friendly_position gt 2` |
| `lt` | 小于 | `$friendly_position lt 3` |
| `gte` | 大于或等于 | `$friendly_position gte $count` |
| `lte` | 小于或等于 | `$friendly_position lte 5` |
| `not` | 否定 | `not $lang`<br><br>（否定可用于检查变量是否包含任何值或是否为空。如果未给出值，则条件的结果为 true，否则条件的结果为 false。） |
| `mod` | 取模 | `$count mod 2`<br><br>取模运算求一个数除以另一个数的余数。你可以使用上面的例子来判断第一层级页面的数量是偶数还是奇数。 |

IF 结构是处理模板中根据页面名称或层级位置的细微变化的有用工具。在前面的示例中，你已经看到可以使用模板中的每个变量来构建逻辑条件。

## 使用 Smarty 循环创建导航模板

你还可以更改导航的 HTML 标记。在本章中，解释了“模板中模板”的伟大原则，它赋予你完全控制设计的权力。在学习这一原则之前，你需要了解一些 Smarty 基础知识。

当我们将顶部导航添加到网站时，我们使用了导航的标准模板。它将导航显示为无序 HTML 列表。想象一下，你需要一种页脚导航，其中显示顶部导航中的所有链接。在这种情况下，你不需要无序 HTML 列表。你只想在一行中显示所有链接，用竖线 (|) 分隔，如下所示：

![](img/b21e3dc5c94405bc7b3f102758ea770c_109_0.png)

这意味着你需要为这种导航使用完全不同的 HTML 标记。CMS Made Simple 的一大优势是可以将模板显示在模板中。虽然你可以使用主模板定义页面的整体布局，但导航的 HTML 标记保存在其自己的模板中。该导航模板只是添加到主模板中的 HTML 代码片段，放置在标签 `{menu}` 的位置。

菜单模板包含导航的一些 HTML 标记。让我们看看如何为菜单创建一个全新的模板并添加你自己的 HTML 标记。

### 动手时间 – 创建菜单模板

我们需要一个简单的页脚导航，其中列出网站的所有页面，用竖线 (|) 符号分隔，如前所示。

- 1. 在页面的管理控制台中，点击 **Layout | Menu Manager**。
- 2. 点击 **Add Template**。
- 3. 填写 **New Template Name** 和 **Template Content** 的字段，如以下屏幕截图所示：

![](img/b21e3dc5c94405bc7b3f102758ea770c_110_0.png)

- 4. 点击 **Submit** 保存新的菜单模板。
- 5. 打开 **Business World** 模板进行编辑（**Layout | Templates**）。
- 6. 使用 `{menu}` 标签添加页脚导航，并提供菜单模板的名称，如以下代码片段所示：

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
```

```
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
{menu start_level="2" template="minimal_menu.tpl"}
</div>
<div id="footer">
<p>{menu template="footer_menu"}</p>
2009 businessWorld
</div>
</div>
</body>
</html>
```

7. 点击 Apply 并查看网站上的页脚导航。它应如下图所示：

![](img/b21e3dc5c94405bc7b3f102758ea770c_111_0.png)

## 发生了什么？

你刚刚创建了一个新的菜单模板。在菜单模板中，你使用了需要解释的一些 Smarty 代码。代码中最重要的部分是 Smarty 变量 $nodelist。它不是我们已经学过的普通 Smarty 变量。这是一个数组。这意味着其中保存了多个值。该变量包含我们网站所有活动页面的所有信息，例如菜单文本或 URL。通过内置的 Smarty 函数 foreach，我们可以遍历该变量的所有值，并为每个页面显示相同的 HTML 代码片段。让我们具体看看循环是如何工作的。

foreach 遍历变量 {$nodelist} 中保存的每个页面，并将放置在 foreach 标签之间的 HTML 代码打印与变量中找到的页面数量一样多次。每次运行，单个页面的值都保存在变量 $node 中。我们通过 foreach 标签中的参数 item=node 来命名并创建此变量。循环从第一个菜单项——页面 Our Company 开始运行。此时，菜单文本和 URL 保存在变量 $node 中。我们可以通过 {$node->menutext} 和 {$node->url} 访问所需的信息并在 HTML 代码中使用标签。在下一次（第二次）运行中，下一个页面保存在变量 $node 中。因此，从第二次运行开始，另一个菜单文本和 URL 会粘贴到相同的 HTML 代码中。foreach 循环遍历每个页面，并在找不到更多页面时自动停止。

页脚导航显示所有页面，而不考虑其在层级中的位置。你也可以将页脚导航限制为仅第一层级。方法与顶部导航相同。在标签 {menu} 中使用参数 number_of_levels，如下所示：

```
{menu number_of_levels="1" template="footer_menu"}
```

foreach 循环是构建你能想象的任何形式导航的强大方法。这样，你就可以在导航中使用图像而不是文本。

另一个如何创建图形导航的例子如下所示：

```
{foreach from=$nodelist item=node}
<a href="{$node->url}">
    <img src="uploads/design/{$node->alias}.jpg"
         alt="{$node->menutext}" border="0" />
</a>
{/foreach}
```

要理解前面的 Smarty 代码会生成什么 HTML，请在脑海中将粗体 Smarty 标签替换为页面的实际值，例如 Contact Us：

```
<a href="contact-us">
    <img src="uploads/design/contact-us.jpg" alt="Contact Us" />
</a>
```

由于前面的代码放置在 `foreach` 循环中，标记将为每个页面重复。你可以看到，要使图形示例正常工作，你需要为每个页面创建一个 JPG 文件。文件名称必须与页面别名对应。在我们的示例中，对于顶部导航，你需要五张图片：

- - `our-company.jpg`
- - `products.jpg`
- - `client-center.jpg`
- - `contact-us.jpg`
- - `sitemap.jpg`

在你选择的图形编辑程序中创建图像（按钮）并将它们上传到 Web 服务器上的 `uploads/design` 目录。创建一个名为 `graphical_menu` 的新菜单模板，并将 Smarty `foreach` 代码添加到其中，如前所示。然后用以下行替换主模板（**Layout | Templates**）中的顶部导航：

```
```smarty
{menu number_of_levels="1" template="graphical_menu"}
```
```

现在，导航由图形而不是文本组成。

菜单模板中关于每个页面的信息还有更多，列在下表中：

| 变量 | 说明 |
|---|---|
| `$node->pagetitle` | 页面的标题 |
| `$node->depth` | 页面层级中的层级数 |
| `$node->prevdepth` | 页面层级中前一个页面的层级数 |
| `$node->haschildren` | 如果页面有子页面则打印 1 |
| `$node->target` | 页面的目标可由编辑器从下拉菜单中设置。在模板中，你按如下方式使用它，如以下示例所示：<br><br>`<a href="{$node->url}" target="{$node->target}">` |
| `$node->alias` | 打印页面的别名 |
| `$node->current` | 如果页面当前正在显示则打印 1 |
| `$node->type` | 页面的类型。可能的值为：<br><br>`sectionheader`（节标题）<br>`separator`（分隔符）<br>`content`（内容）<br>`pagelink`（内部页面链接）<br>`link`（外部链接） |



## 电子商务研讨会

现在我们已经学会了如何实现一个包含电子商务功能的完整网站，我们可以继续学习高级主题，这些主题需要基础的PHP知识，这将是下一章的内容。届时，你的网站已经完全可用并可以发布，因此你不需要下一章的扩展主题就能拥有一个完整的网站。

## CMS Made Simple 的高级使用

> 本章为使用CMS Made Simple创建网站的高级站长提供一些提示。你无需阅读整章，只需挑选对你最重要的部分。

在本章中，我们将：

- 学习如何使用CMS Made Simple开发多语言网站
- 为网站编辑器创建额外的布局控制选项
- 进行一些搜索引擎优化
- 使用用户定义标签和事件管理器来实现我们自己的功能或定义自定义工作流

你需要具备基础的PHP知识才能按照本章的说明操作。那么让我们开始吧。

### 本地化与翻译

CMS Made Simple 的管理控制台已被翻译成超过20种语言。正如你在第2章中已经了解到的，CMS Made Simple 分为两个版本发布。基础版本只包含英语语言文件，而完整版本包含所有语言文件。

如果你除了英语之外只需要一种语言（英语无法卸载），你可以下载基础包，然后下载所需的语言文件。语言文件可以在CMS Made Simple 官方网站（http://www.cmsmadesimple.org/）页面底部的 **Downloads | CMSMS Release** 部分找到（你只需要与你版本对应的文件）。要在基础安装中包含任何语言，请先下载并解压基础安装文件，然后下载与基础版本号对应的所需语言文件并同样解压。通过将语言文件夹复制到基础文件上来合并基础文件夹与语言文件文件夹的内容。然后像往常一样安装CMS Made Simple。

有权访问网站管理控制台的用户可以在其个人账户中选择首选语言（**My Preferences | User Preferences**）。对于基础安装，该字段中只显示 **English**。如果你安装了任何其他语言，它也会显示在 **Language** 下拉字段中。此设置不影响网站访问者所使用的网站语言。

要设置网站语言，请在管理控制台中点击 **Site Admin | Global Settings**，并更改 **Default language for the frontend**。如果此处未设置语言，则CMS Made Simple默认使用英语（**en_US**）。此设置将自动更改网站上某些模块的默认文本，例如 **Search** 模块中搜索按钮的标签，或 **News** 模块中的某些标签。

![](img/b21e3dc5c94405bc7b3f102758ea770c_279_0.png)

在上面的截图中，可以看到 **Category**（德语 *Kategorie*）、**Posted by**（德语 *Erstellt von*）等标签之间的区别。这将有助于定义大部分由模块显示的、已翻译成你所使用语言的标签。然而，仅当你的网站不是多语言且只使用一种语言时，这才有帮助。

### 配置日期

CMS Made Simple 时常需要输出日期和时间。尤其是对于新闻文章，正确的日期格式非常重要。

可以在管理控制台（**Site Admin | Global Settings**）的 **Date Format String** 字段中更改通用日期格式。日期格式应根据PHP格式函数 `strftime` (http://www.php.net/manual/en/function.strftime.php) 输入。下表显示了一些示例：

| 日期格式字符串 | 输出 |
|---|---|
| `%Y-%m-%d` | 2009-05-20 |
| `%a %d %b %Y` | Wed 20 May 2009 |
| `%a %d %b %H:%M:%S %Y` | Wed 20 May 15:53:40 2009 |

第一列根据允许的参数构建（请参阅最后一个链接以了解说明）。第二列显示页面上的结果。在 **Date Format String** 字段中输入第一列的参数，即可获得第二列所示的结果。

月份和星期名称取决于服务器的 `locale` 设置。在某些服务器上，你可以更改用于输出日期的语言。设置区域设置也会影响货币、小数分隔符以及其他依赖于语言的输出。在 CMS Made Simple 中，打开 `config.php` 文件，找到 `$config['locale']` 这一行。你在此处输入什么值取决于你的服务器。对于 Linux 服务器上的德语月份名称，请尝试更改该行，如下所示：

```php
$config['locale'] = 'de_DE.UTF8';
```

### 创建多语言网站

多语言网站使用不止一种语言创建。每种语言都有自己翻译后的导航、内容，可能还有翻译后的新闻文章。我们这里谈的不是由Google或任何其他工具进行的网站自动翻译。自动翻译很糟糕。它们往往让人无法阅读，而且毫无意义。在本节中，你将学习如何创建一个真正的多语言网站，其中所有内容都由人工翻译人员翻译。

你可以使用 **Babel** 模块来管理网站和导航的多种语言。安装该模块的方式与为网站安装其他模块相同（**Extensions | Module Manager**）。

安装模块后，为网站设置页面结构以匹配你需要的语言，例如：

- 1. 英语
  - 1.1. 我们的公司
  - 1.2. 公告
  - 1.3. 联系我们
- 2. 德语
  - 2.1. Unternehmen
  - 2.2. Aktuelles
  - 2.3. Kontakt

根语言页面（1. 和 2.）应定义为内容类型为“部分标题（Section Headers）”的页面。你可以为它们设置任何想要的标题。对于页面别名，我建议使用两个字母的ISO 639-1代码系统，例如en（英语）、de（德语）、fr（法语）。页面别名将用于网站URL，如 http://www.yourdomain/en/our-company 或 http://www.yourdomain/de/kontakt。

为了正确显示菜单（即不显示语言标题页面和属于其他语言的页面），你必须在主模板（Layout | Templates）的顶部菜单标签中添加参数 start_level="2"。打开模板，调整你的顶部导航，如以下代码片段所示：

```
{menu number_of_levels="1" start_level="2"}
```

这将确保在顶部导航中只显示属于所选语言的第二级页面。如果你使用侧边栏导航，也应通过减少起始级别数来更改这里的级别数：

```
{menu start_level="3"}
```

页面结构创建完成后，在管理控制台中点击 Extensions | Babel: Multilingual Site。对于你在页面结构中集成的每种语言，点击“添加语言”链接，并输入相应的信息，如以下截图所示：

![](img/b21e3dc5c94405bc7b3f102758ea770c_281_0.png)

**与该语言关联的根页面** 应为你在每种语言中创建的部分标题之一。**用于重定向到该语言的默认页面** 应为每种语言的起始页（而不是前一个字段中的部分标题）。对于 **语言代码**，你可以使用任意五个字符的代码，但为了与模块提供良好的兼容性，你应该使用与CMS Made Simple相同的语言代码。使用管理区域右上角的下拉列表，用默认值预填表单。

点击 **提交**，并确保你已在 **Active** 列中激活了该语言。以相同方式添加所有需要的语言。

要显示语言菜单，请在主模板中应显示不同语言链接的位置使用以下标签：

```
{babel action="menu"}
```

### 编辑语言条目

在 **Babel** 模块的管理区域，你会找到一个用于编辑每种语言的语言条目的链接。语言条目是每种语言都存在但包含不同文本（或HTML代码）的字符串。对于 **Search** 模块中的 *搜索* 按钮标签，你肯定希望有不同的描述，例如：

- 英语为 *Search*
- 德语为 *Suche*
- 法语为 *Recherche*，等等。

打开管理控制台中的 **Babel: Multilingual Site** 模块，点击 **编辑语言条目** 链接。点击模块管理区域右上角的 **添加新语言条目** 链接。在这里你可以为每种语言定义你自己的翻译。条目名称是一个通用键，用于获取正确的翻译。在我们的示例中，我们想要翻译搜索按钮的标签，因此翻译的键可以是 `search button`。在下面的字段中，为每种已定义的语言输入翻译，然后点击 **提交**。

在 **Search** 模块的模板（Extensions | Search，Search Template 选项卡）中自定义搜索按钮的标签，并将标签 `{$submittext}` 替换为 `{babel show="search button"}`，如以下代码片段所示：

```
{$startform}
<input type="text" class="search-input"
    id="{$search_actionid}searchinput"
    name="{$search_actionid}searchinput" size="18" maxlength="50"
    value="{$searchtext}" {$hogan}/>
<input class="search-button" name="submit" value="{babel show="search button"}" type="submit" />
{if isset($hidden)}{$hidden}{/if}
{$endform}
```


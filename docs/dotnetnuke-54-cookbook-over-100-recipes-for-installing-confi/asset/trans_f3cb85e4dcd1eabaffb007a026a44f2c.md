

24. 新的 ASCXContainer 现在将会显示：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_275_0.png)

25. 若要查看容器的外观，请单击“预览”链接。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_275_1.png)

26. 我们可以看到，这些图像在容器周围形成边框，模块标题位于左侧，紧邻悬停菜单。此外，可以看到我们在步骤11中创建的操作按钮出现在容器下方。还要注意，顶部和底部的线条图像会自动填充到容器的整个宽度。

## 使用图像设置菜单样式

在本教程中，我们将使用简单的 ASCX 皮肤，并展示如何通过将菜单控件与背景图像相结合来为菜单设置外观。

使用图像为 DNN 菜单设置外观包括两部分：第一部分是构成菜单背景的一组图像；第二部分是由菜单控件提供的菜单中显示的文本。

## 基本外观设置

在本教程中，我们将详细研究为模板皮肤创建的专业外观菜单。我们将看到构成菜单的所有部分、如何为其创建背景图像以及如何设置文本样式。当所有部分到位后，我们将会得到一个如下所示的菜单：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_276_0.png)

## 准备工作

要继续本教程，您必须已完成以下教程：

- 下载并安装皮肤
- 创建简单的 ASCX 皮肤

## 操作步骤

1. 首先，在测试 DNN 门户中找到默认皮肤文件夹（`\\Portals\\_default\\Skins`）。
2. 接下来，创建一个新文件夹来存放皮肤文件。将此文件夹命名为 `ASCXMenuSkin`。
3. 在其中创建一个新文件夹以存放图像：`ASCXMenuSkin\\images`。
4. 找到 *下载并安装皮肤* 教程中模板皮肤所在的文件夹。
5. 找到以下图像并将其复制到 `ASCXMenuSkin\\images` 文件夹中。
   - `menu_bg.png`
   - `menu_hov.png`
   - `menu_left.png`
   - `menu_right.png`
   - `submenu_hov.png`

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_276_1.png)

6. 图像就位后，我们现在可以打开 *创建简单的 ASCX 皮肤* 教程中创建的示例 ASCX，并进行扩展以使用菜单图像。
7. 启动开发工具。
8. 从**文件**菜单中选择**打开文件...**。
9. 浏览到 SampleASCXSkin 文件夹，并选择 SampleASCXSkin.ascx 文件。
10. 在注册 DIV 和登录 DIV 之间找到菜单皮肤对象：

```
<%@ Register TagPrefix="dnn" TagName="LOGIN" Src="~/Admin/Skins/Login.ascx" %>

<div >
    <dnn:NAV runat="server" id="dnnNAV"
        ProviderName="DNNMenuNavigationProvider"
        IndicateChildren="false"
        ControlOrientation="Horizontal" />
</div>

<div id="login_style" class="user">
```

11. 用下面这个使用 CSS 样式的 DIV 替换包含菜单的 DIV：

```
<%@ Register TagPrefix="dnn" TagName="LOGIN" Src="~/Admin/Skins/Login.ascx" %>

<div class="menu_style">
    <dnn:NAV runat="server" id="dnnNAV"
        ProviderName="DNNMenuNavigationProvider"
        IndicateChildren="false"
        ControlOrientation="Horizontal"
        CSSControl="mainMenu" />
</div>
<div id="login_style" class="user">
```

12. 从**文件**菜单中选择**将 SampleASCXSkin.ascx 另存为...**。
13. 浏览到 ASCXMenuSkin 文件夹，并将文件保存为 ASCXMenuSkin.ascx。
14. 由于真正的工作在 CSS 文件中，我们需要定义样式，为菜单提供图像、字体和颜色。从**文件**菜单中选择**打开文件...**。
15. 浏览到 SampleASCXSkin 文件夹，并选择 skin.css 文件。
16. 滚动到文件底部，首先添加以下类：

```
/* step 1: the main menu */

.mainMenu                 {font-family: Verdana, Arial,
Helvetica, sans-serif; cursor: pointer; font-size: 13px; font-
weight: bold;}
.mainMenu_bg              {background: url(images/menu
bg.png) repeat-x top left; margin: 0 19px 0 7px; height: 40px;}
.mainMenu_left            {background: url(images/menu_left.
```

```
png) no-repeat top left;}
.mainMenu_right                    {background: url(images/menu_right.png) no-repeat top right;}
.mainMenu .root                    {text-align: center; line-height: 40px; padding: 12px 12px 12px 12px; color: White;}
```

17. 如果此时保存文件，然后从门户查看皮肤，菜单将如下所示：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_278_0.png)

18. 接下来要添加的是菜单悬停和活动菜单。这些图像将在鼠标悬停在主菜单文本上或菜单处于活动状态时显示。这是一张窄图像，但 repeat-x 会使其水平扩展，以填满菜单文本的长度。将此代码添加到 CSS 文件底部：

```
/* Main menu hover */
.mainMenu .hov{ color:#fff; background:url(images/menu_hov.png) repeat-x top left;}
```

```
/* Main menu selected */
.mainMenu .sel, .mainMenu .bc {
color:#FFF;
background:url(images/menu_hov.png) repeat-x top left;
}
```

19. 这创建了一个在鼠标悬停在菜单项上时显示的图像，我们将在菜单处于活动状态时使用相同的图像：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_278_1.png)

20. 最后一步是为子菜单提供样式。将以下代码添加到 skin.css 文件底部：

```
/* general submenu css */

.mainMenu .m                        {width: 160px; font-size: 11px; font-weight: bold; z-index: 1000; line-height: 2em;}
.mainMenu .m .mi                    {background-color: #242424;}
.mainMenu .m .icn                   {padding-left: 5px; width: 20px;}
.mainMenu .m .mi *                  {color: white; margin-right: 5px;}
/* change * to .txt with latest webcontrols */
.mainMenu .m .sel, .mainMenu .m .bc {background: url(images/submenu_hov.png) repeat-x top left;}
```

```
.mainMenu .m .hov
{ background: url(images/submenu_hov.png) repeat-x top left; }
```

21. 这创建了鼠标悬停在子菜单上时显示的图像：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_279_0.png)

22. 从“文件”菜单中选择“将 skin.css 另存为...”。

23. 浏览到 ASCXMenuSkin 文件夹，并将文件保存为 skin.css。

24. 你还可以制作新容器的预览图像，类似于：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_279_1.png)

25. 将图像文件保存为 ASCXMenuSkin.jpg。

26. 要查看新皮肤在网站上的效果，请以超级用户身份登录。

27. 在 Admin 菜单下查看，然后选择 Skins。

28. 从“Skins:”下拉列表中选择 ASCXMenuSkin。

29. 新的 ASCXMenuSkin 现在将会显示：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_279_2.png)

30. 这是一个非常简单的示例，但它展示了菜单外观设置的基础知识。

## 9

## 多语言支持

- 下载并安装语言包
- 通过单个门户创建双语站点
- 编辑语言资源文件
- 创建你自己的模块翻译
- 确定需要翻译的控件
- 本地化标签、标题、面板和链接
- 使用存储过程本地化下拉列表
- 使用 DNN 列表本地化下拉列表
- 本地化存储过程
- 本地化 DataGrid 控件

## 简介

随着世界日益变小，网站支持多种语言的能力变得越来越重要。幸运的是，DNN 通过一个称为“本地化”的过程提供外语支持。

DNN 本地化建立在 .NET 框架本地化工具的基础之上。由于同一种语言往往在多个国家使用，.NET 中的外语按区域设置代码组织，该代码由语言代码和国家代码组合而成。例如，代码 `fr-CA` 代表加拿大使用的法语，`Fr-FR` 代表法国使用的法语。

## 一带一路（Belt and Road）

中国的“一带一路”倡议是一项旨在促进贸易和基础设施建设的计划。

该计划于2013年在时任国家主席胡锦涛出席博鳌亚洲论坛年会时宣布，名称源于古代丝绸之路网络。目前，该计划已涉及60多个国家，以及从公路、港口到油气田等领域的众多大型基础设施项目。

中方表示，该倡议将促进与邻国的贸易和基础设施建设，并为中国商品打开海外市场。


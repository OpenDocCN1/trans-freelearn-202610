
![](img/b21e3dc5c94405bc7b3f102758ea770c_94_0.png)

- 3. 点击**提交**。
- 4. 从样式表列表（**布局 | 样式表**）中打开**我的第一个样式表**进行编辑。
- 5. 在样式表末尾为容器`top-navi`添加样式格式，如下面的代码片段所示：

```
#top-navi ul
{
    height: 22px;
    padding: 0px;
    margin: 10px 0;
    border-top: 1px solid #e5e4e2;
    border-bottom: 1px solid #e5e4e2;
}
#top-navi li
{
    list-style: none;
    float:left;
}
#top-navi li a
{
    color: #a2a2a2;
    text-decoration: none;
    display: block;
    padding: 5px 15px;
}
```

```
#top-navi li a:hover
{
    color: #ffffff;
    background: #3d648a;
}
```

6. 点击**应用**，然后点击管理控制台右上角的放大镜图标，查看更改的结果。现在应该看起来如下面的截图所示：

![](img/b21e3dc5c94405bc7b3f102758ea770c_95_0.png)

## 刚才发生了什么？

你通过向`{menu}`标签添加`number_of_levels`参数，将导航的输出范围限制为第一级。然后，你在Smarty插件`{menu}`周围放置了一个HTML `<div>`标签。在最后一步，你添加了一些CSS样式，使顶部导航具有所需的外观和感觉。请注意，为导航添加样式并不需要任何特殊的“CMS设计”知识，你使用的是纯的、通用的CSS来格式化导航。

如果你在浏览器中逐个点击页面，并查看其他页面上导航的显示方式，你会注意到CMS使用了一些额外的格式来显示活动页面。例如，`<h3>`用于在导航中显示活动页面的名称。在浏览器中打开页面源代码，即可查看活动页面的格式。你可以添加更多样式来更改`<h3>`标签的格式。另一方面，为了节省时间，你可以使用内置模板`minimal_menu.tpl`，将这些特殊格式全部缩减为纯列表。该模板随CMS Made Simple的标准安装自动提供。要使用它，只需向`{menu}`标签添加另一个参数，如下面的代码行所示：

```
{menu number_of_levels="1" template="minimal_menu.tpl"}
```

这样，所有额外的标签都会从导航中删除，每个页面都会显示一个清晰的无序列表。

如果你查看网站的页面源代码，会发现菜单中还添加了一些特殊的class属性。CMS知道当前实际显示的是哪个页面，并会为活动页面的HTML标签`<a>`添加一个特殊的类：

```
```html
<li><a href="http://www.yourdomain.com/" class="currentpage"> Our Company </a>
```
```

你可以利用这一信息，为当前显示页面的导航项设置另一种样式。最简单的方法是组合`a:hover`和`a.currentpage`类，使它们具有相同的样式。

```
```css
#top-navi li a:hover, #top-navi li a.currentpage
{
    color: #ffffff;
    background: #3d648a;
}
```
```

如果你能用CSS设计网站，那么现在你就可以在不需要任何特殊编码知识的情况下，设计网站的简单导航。

## 添加侧边栏导航

然而，这只是完成了一半。顶部导航中显示的页面仅限于第一级。我们需要一些侧边栏导航来显示属于父页面的子页面列表。

我们必须讨论插件`{menu}`的另一个重要参数。通过`number_of_levels`参数，你可以设置页面层次结构中显示多少层。对于侧边栏导航，你不需要限制层数，而是需要定义导航的起始层。由于第一层已经添加到顶部导航中，因此在侧边栏中，你应该从第二层开始：

```
```text
{menu start_level="2" template="minimal_menu.tpl" }
```
```

使用`start_level`参数，你可以指定菜单中从哪一层开始显示页面。第二层意味着显示层次结构中至少包含两个数字的页面，例如1.2、2.3或4.2。CMS Made Simple还知道顶部导航中选择了哪个页面，并只在侧边栏中显示该所选页面的子页面。这意味着，如果在顶部导航中选择了编号为3的页面，侧边栏将只包含该页面的子页面：3.1、3.2、3.3等。

使用`template`参数，你可以再次将导航输出简化为纯HTML无序列表，从而使此处的格式化更加容易。

一个有趣且常用的参数是`collapse`，例如`{menu collapse="1"}`。该参数允许折叠菜单中的子项，因此只显示当前菜单项的子项。页面层次结构中至少需要有三层，才能看到自动折叠如何隐藏不属于当前显示页面的菜单项。

我们已经学习了模板最重要的部分：标题、内容和菜单。有了这些信息，就足以将现成的HTML模板移植到CMS Made Simple中。

## 移植HTML模板

通常，你不会在CMS Made Simple中从头设计一个完整模板。你可以在CMS Made Simple之外使用任何HTML/CSS编辑器来创建设计。也有可能你下载或购买了现成的HTML模板，或者你与一位为你的网站创建设计的设计师合作。在上述所有情况下，你都会得到一个HTML文件、一个CSS文件和一个或多个图片。在本节中，我们将学习如何将HTML模板移植到CMS Made Simple中。

假设你得到了一个如下图所示的HTML模板。

![](img/b21e3dc5c94405bc7b3f102758ea770c_97_0.png)

该模板由三个文件组成：HTML、CSS和位于右上角的Logo图片。请注意，此时除了纯HTML和CSS外什么都没有。让我们根据上述模板创建一个新的CMS Made Simple模板。

## 动手实践——将HTML模板移植到CMS Made Simple

- 1. 在管理控制台中，点击**布局 | 模板**，然后点击**添加新模板**。
- 2. 输入**Business World**作为模板的**名称**，并将建议的**内容**全部替换为前一张截图中的示例模板的HTML代码。该模板的HTML代码如下：

```
<!DOCTYPE html>
<html>
    <head>
        <title>Our Company</title>
        <link rel="stylesheet" type="text/css" media="screen" href="style.css" />
        <meta name="description" content="" />
    </head>
    <body>
        <div id="container">
            <div id="header">
                businessWorld
            </div>
            <div id="top-navi">
                <ul class="clearfix">
                    <li><a href="#"> Our Company </a></li>
                    <li><a href="#"> Products </a></li>
                    <li><a href="#"> Client Center </a></li>
                    <li><a href="#"> Contact Us </a></li>
                    <li><a href="#"> Sitemap </a></li>
                </ul>
            </div>
            <div id="content">
                <h1>Our Company</h1>
                <p>
                    Lorem ipsum quo ea eruditi epicuri dolores, mea no
                    delicata reprehendunt. Mea possit aperiri iracundia in, ad
                    veri ocurreret scribentur his, justo qualisque adipiscing
                    pro an. Has congue dissentiunt an, ea summo simul
                    repudiandae sit. Omnium aliquam disputationi est no, etiam
                    deleniti eos in, te pro reque torquatos. Adhuc cetero
                    alienum te mei, nec id solum partem populo.
                </p>
                <p>
                    Sit putent fabulas complectitur ne, pro te velit viris
                    ocurreret. Eam dico habeo aeterno ei, ut deserunt
                    eloquentiam pri. Vim ad alii populo expetendis, et pro
                    dolorum scaevola partiendo. Mea te timeam delicata
```


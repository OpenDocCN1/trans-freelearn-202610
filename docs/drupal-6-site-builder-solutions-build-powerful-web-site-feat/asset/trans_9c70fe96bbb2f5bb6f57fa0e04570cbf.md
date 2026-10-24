

- TITLE 将显示在图片和描述的上方。如果不想显示标题，可以将其留空。
- DESCRIPTION 显示在标题下方但在图片上方。如果不需要，也可以省略。
- TARGET 与 LINK 结合使用，描述当点击图片时新页面如何打开。有效值有：`_blank`、`_parent`、`_top` 和 `_self`。默认的 `_blank` 选项会根据浏览器情况在新窗口或新标签页中打开链接。`_self` 选项会在幻灯片所在的同一窗口中打开链接（因此会替换幻灯片）。`_parent` 和 `_top` 值仅由使用框架的站点使用。

可以通过用逗号分隔每个 img 部分与前一个部分来插入多张图片。

以下 filter 标签用于 Good Eatin' 首页以显示餐厅的几张图片。

```
[slideshow:2, img=|http://goodeatin.drupalbyexample.com/image/view/3/preview||Elephant Smile|This Elephant is one of the many animals you may see as you take a Safari through the Good Eatin' restaurant.||, img=|http://goodeatin.drupalbyexample.com/image/view/4/preview||Zebra Friends|Enjoy the sun with these Zebras on a Good Eatin' Safari meal. ]
```

## 设置幻灯片以显示文件夹中的所有图片

虽然使用单独的图片创建幻灯片可以让你对幻灯片的显示方式有极大的控制，但创建一个简单显示文件夹中所有图片的幻灯片会非常方便。此类幻灯片使用以下 filter 标签格式：

```
[slideshow:VERSION, dir=|DIR_IMAGE|DIR_RECURSIVE|DIR_LINK|TITLE|DESCRIPTION|TARGET|]
```

VERSION、TITLE、DESCRIPTION、DIR_LINK 和 TARGET 的使用方式与单独的 img 版本相同，只是这些值将应用于文件夹中的每张图片。文件夹特定设置如下：

- DIR_IMAGE 是文件位于你 Web 服务器上的文件夹。此路径将相对于 Drupal 安装所在的文件夹。幻灯片创建器会将此文件夹内找到的所有 PNG、GIF、JPEG、JPG 和 BMP 图片包含到幻灯片中。
- `DIR_RECURSIVE` 可以设置为 yes 以包含指定图片文件夹的子文件夹中的图片，或留空以仅包含指定的文件夹。在大多数情况下你会将其留空，因为将每张幻灯片的图片组织在单个文件夹中更方便。

你可以在单个幻灯片中组合 `img` 组和 `dir` 组，以利用每种方法的功能来满足你的需求。

当你显示文件夹中的所有图片时，图片会以随机顺序显示，并且无法可靠地控制显示顺序。

## 其他配置选项

幻灯片创建器还包含一些未记录的功能，可用于进一步自定义你的幻灯片。其中一些功能允许你覆盖通过管理菜单配置的默认设置，以便你对幻灯片布局有更精确的控制，并且可以在不同页面上拥有不同尺寸和布局的幻灯片。你甚至可以在一个页面上拥有多个幻灯片，每个幻灯片具有不同的选项。以下选项可用：

- `width`: 这允许你控制幻灯片的宽度。如果幻灯片中的图片比指定宽度更宽，则会被裁剪。
- `height`: 这允许你控制幻灯片的高度。如果幻灯片中的图片比指定高度更高，则会被裁剪。
- `layout`: 这允许你控制幻灯片中元素（标题、描述、图片和控件）的顺序。有效值有 Default、Reverse、Top、Bottom 和 None。这些会导致以下配置（元素按从上到下的出现顺序列出）：
- `Default`: Controls, Title, Picture, Description
- `Reverse`: Title, Picture, Description, Controls
- `Top`: Picture, Title, Description, Controls
- `Bottom`: Controls, Title, Description, Picture
- `None`: Picture, Title, Description
- `blend`: 这控制图片之间过渡的速度。它应以毫秒为单位设置，因此要指定 2 秒，你需要输入 2000。
- timeout: 这控制每张图片显示多长时间，然后开始加载下一张图片。这也以毫秒为单位设置。
- fx: 这控制用于在幻灯片之间过渡的效果类型。一些可用的过渡效果有 shuffle、zoom、fade、turnDown、curtainX、scrollRight、blindX、blindY、blindZ、cover 和 uncover。

应在使用任何这些附加选项时在图片列表开始之前添加它们。例如，以下代码为幻灯片添加了缩放过渡效果，并将幻灯片的控件放置在图片下方。

```
[slideshow:2,width=450,height=650, layout=top, fx=zoom, img=!http://goodeatin.drupalbyexample.com/image/view/3/preview||Elephant Smile|This Elephant is one of the many animals you may see as you take a Safari through the Good Eatin' restaurant.|, img=!http://goodeatin.drupalbyexample.com/image/view/4/preview||Zebra Friends|Enjoy the sun with these Zebras on a Good Eatin' Safari meal. ]
```

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_74_0.png)

有关 Slideshow Creator 模块的附加文档可在以下位置获得：http://drupal.org/node/237049。
有关用于构建幻灯片的 JQuery Cycle 模块的附加文档可在以下位置获得：http://www.malsup.com/jquery/cycle/。

## 版本特定问题

Slideshow Creator 的作者目前仅向当前版本的 Drupal 用户提供该模块（撰写本文时为版本 6）。如果你使用的是其他版本，你可能能够找到与你的版本兼容的下载，但可能无法通过原作者获得支持。

## 使用 Lightbox2 在页面上显示图片

Slideshow Creator 模块允许你在页面内查看幻灯片。然而，有时将图片显示在浮于主页面之上的新窗口中看起来更好。这对于摄影或艺术网站尤其如此，因为图片是页面的主要焦点。

在主页面上显示幻灯片也有助于改善幻灯片的外观，尤其是在所有图片大小不相同时。

## 创建 Good Eatin' 的在线形象

一个示例 **lightbox** 如下图所示：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_75_0.png)

要使用 Lightbox2 模块：

- 1. 从 Drupal 网站下载模块。项目页面位于 `http://drupal.org/project/lightbox2`。
- 2. 将模块复制到你的网站并在模块管理器中启用它。
- 3. 通过从 **Administer** 菜单中选择 **Site Configuration** 然后 **Lightbox 2** 来配置 Lightbox2。Lightbox2 有大量设置，分为四个部分。让我们逐一探索这些设置。

## 通用设置

通用设置页面包含用于控制 Lightbox 显示的各种设置。这些设置有：

- **Use Lightbox2 Lite:** 启用此设置将极大地限制 Lightbox2 的功能。所有控件将被移除，因此访客将无法从一张图片导航到另一张图片。如果你的网站流量很大，或你希望确保一次只显示一张图片，这可能是可取的。
- **Use Alternative Layout:** 在正常的 Lightbox 布局中，下一个和上一个按钮覆盖在图片上，并且仅在鼠标经过链接时显示。当替代布局处于活动状态时，链接始终显示，并且它们放置在图片下方，旁边是标题。我个人更喜欢启用此选项，因为始终向用户显示导航比让他们寻找控件要友好得多。


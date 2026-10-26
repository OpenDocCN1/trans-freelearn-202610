

## 更多内容……

> 要查看更多的皮肤控件示例及其工作原理，请下载并查看Extropy皮肤，可从www.codeplex.com获取（只需搜索“Extropy”）。

在本节中，我们创建了一个简单的自定义控件，并将其部署到DNN站点上，以便皮肤可以使用该控件。在下一节中，我们将看到如何在皮肤中使用自定义控件。

## 在皮肤中使用自定义控件

一旦自定义皮肤控件被创建并安装到服务器上，它就会像DNN自带的控件一样可以使用。在本节中，我们将看到如何将上一节中创建的自定义皮肤控件与第8章“基本皮肤”中的SampleASCXSkin结合起来使用。

我们需要做三件事：

1. 确保你已经完成了上一节，并更新了模板JavaScript。
2. 将自定义控件添加到ASCX文件中。
3. 部署皮肤并进行测试。

### 准备工作

要跟随本节操作，你必须完成以下章节：

- 创建一个简单的ASCX皮肤
- 创建并部署自定义控件

### 操作步骤……

1. 要在皮肤中使用ACME.SampleWidget控件，我们需要编辑“创建一个简单的ASCX皮肤”一节中的示例ASCX皮肤文件。首先找到名为SampleASCXSkin的文件夹。
2. 启动开发工具。
3. 从“文件”菜单中选择“打开文件...”。
4. 浏览到SampleASCXSkin文件夹，打开SampleASCXSkin.ascx文件。

11. 在左侧你会看到一个带有文本的小块。点击控件文本将导致DIV中的文本淡出：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_407_0.png)

### 工作原理……

这个自定义的ACME.SampleWidget通过一个简单的JavaScript点击处理程序使元素淡出，当点击包含示例文本的DIV元素时，它会动态修改该DIV的透明度。这足以演示自定义控件的工作方式，但更复杂的示例需要更多的JavaScript代码以及对JavaScript编程更深入的讨论。

> 创建自定义DNN控件本身就是一门艺术。你可以从Nik Kalyani的这篇优秀博客中了解更多关于制作自定义控件的内容：http://www.kalyani.com/2009/12/dotnetnuke-widgets-guide-part-1-of-4。

## 使用圆角超级样式表

超级样式表是DNN 5版本中的一项新功能，通过最大化利用CSS来提供重用HTML布局的方法。在撰写本文时，有三个可用的超级样式表：Yahoo YUI库、DNN布局样式表和圆角样式表。

在本节中，我们将演示如何导入并使用圆角样式表，在皮肤内容周围创建一个图形框。

为了实现盒子效果，我们首先使用一组自定义的边框图像，然后在皮肤中创建一组嵌套的DIV，最后使用DNN提供的`dnn-roundedcorners.css`文件将图像链接到DIV元素。这与第8章“基本皮肤”中的“使用图像设置容器样式”一节非常相似，但这次圆角样式表将使用CSS定位图像，而不是使用HTML表格。

### 准备工作

要跟随本节操作，你必须完成第8章“基本皮肤”中的以下章节：

- 创建一个简单的ASCX皮肤

所有部分就位后，图形框将如下所示：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_408_0.png)

### 操作步骤……

1. 找到“创建一个简单的ASCX皮肤”一节中创建的`SampleASCXSkin`文件夹。
2. 在其中创建一个新文件夹`\images`，用于存放图像。
3. 我们将使用一些图像来制作盒子的边框。在本示例中，我们将使用以下图像：

- `box_e.png`：图形框的右侧
- `box_n.png`：图形框的顶部第一段
- `box_ne.png`：图形框的右上角
- `box_nw.png`：图形框的左上角
- `box_s.png`：图形框的底部
- `box_se.png`：图形框的右下角
- `box_sw.png`：图形框的左下角
- `box_w.png`：图形框的左侧

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_409_0.png)

4. 将图像放入正确的文件夹后，启动开发工具。
5. 从“文件”菜单中选择“打开文件...”，打开skin.css文件。
6. 在文件顶部添加一条导入语句，引入DNN的roundedcorners.css文件。如果DNN运行在本地主机上，则代码为：

```
@import url("http://localhost/dotnetnuke/resources/shared/stylesheets/dnn-roundedcorners.css");
```

7. 在导入语句下方，添加以下代码，将图像链接到DNN的样式元素：

```
.RoundedCorners .N{background-image: url("images/box_n.png"); }
.RoundedCorners .NE{background-image: url("images/box_ne.png"); }
.RoundedCorners .E{background-image: url("images/box_e.png"); }
.RoundedCorners .SE{background-image: url("images/box_se.png"); }
.RoundedCorners .S{background-image: url("images/box_s.png"); }
.RoundedCorners .SW{background-image: url("images/box_sw.png"); }
.RoundedCorners .W{background-image: url("images/box_w.png"); }
.RoundedCorners .NW{background-image: url("images/box_nw.png"); }
/* 通用样式 */
```

8. 现在样式表已经设置好，下一步是修改控件。从“文件”菜单中选择“打开文件...”，打开SampleASCXSkin.ascx文件。

9. 向下滚动到包含各窗格的HTML表格，并将以下代码添加到左侧窗格中：

```html
<div class="content">
    <table width="100%" border="0" cellspacing="0" cellpadding="0">
        <tr>
            <td valign="top" id="LeftPane" class="LeftPane"
                runat="server" visible="false">
            </td>
            <td valign="top" id="ContentPane" class="ContentPane"
                runat="server" visible="false">

                <div class="RoundedCorners">
                    <div class="W">
                        <div class="E">
                            <div class="N">
                                <div class="NW">
                                    <div class="NE" align="center"
                                         style="padding: 10px; vertical-align: middle">
                                        你可以在皮肤中放置任意数量的内容，
                                        RoundedCorners样式表会自动调整
                                        图形框的大小以适配内容。即使你的内容需要换行，
                                        图形框也会相应地调整大小。
                                    </div>
                                </div>
                            </div>
                            <div class="S">
                                <div class="SW">
                                    <div class="SE" align="center">
                                        [ 你可以在这里放置页脚内容 ]
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

            </td>
            <td valign="top" id="RightPane" class="RightPane"
                runat="server" visible="false">
            </td>
        </tr>
    </table>
</div>
```

10. 最后一步是修改清单文件，在部署皮肤之前加入我们的图像。打开`SampleASCXSkin.dnn`文件。

11. 向下滚动到`SkinFiles`元素，并添加以下内容：

```xml
<SkinFile>
    <name>skin.css</name>
</SkinFile>
<SkinFile>
    <path>images</path>
    <name>box_n.png</name>
</SkinFile>
<SkinFile>
    <path>images</path>
    <name>box_ne.png</name>
</SkinFile>
<SkinFile>
    <path>images</path>
    <name>box_e.png</name>
</SkinFile>
<SkinFile>
    <path>images</path>
    <name>box_se.png</name>
</SkinFile>
<SkinFile>
    <path>images</path>
    <name>box_s.png</name>
</SkinFile>
<SkinFile>
    <path>images</path>
    <name>box_sw.png</name>
</SkinFile>
<SkinFile>
    <path>images</path>
    <name>box_w.png</name>
</SkinFile>
<SkinFile>
    <path>images</path>
    <name>box_nw.png</name>
</SkinFile>
</SkinFiles>
```

12. 从**文件**菜单中选择**保存全部**以保存更改。

13. 要查看新皮肤在站点上的效果，请以超级用户身份登录。

14. 按照“部署你的皮肤和容器”一节中的描述，将皮肤从皮肤文件夹部署到DNN门户。请务必在ZIP文件中包含新的`/images`文件夹。


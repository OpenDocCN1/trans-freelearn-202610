

6. 将图像保存为名为 `ContainerUC1.gif` 的 GIF 文件。

7. 下一张图像是容器的顶部边框，并且会随着容器宽度的变化水平平铺。将属性设置为 12 像素宽 27 像素高。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_269_0.png)

8. 将图像保存为名为 `ContainerUC2.gif` 的 GIF 文件。

9. 对于容器底部的边框，我们需要再两张图像。第一张是左下角。使此图像为 90 像素宽 27 像素高。此图像将沿底部平铺以适应容器的宽度。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_269_1.png)

10. 将图像保存为名为 `ContainerLL.gif` 的 GIF 文件。

11. 最后一张图像用于右下角。此图像不平铺。使其为 100 像素宽 27 像素高。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_269_2.png)

12. 将图像保存为名为 `ContainerLR.gif` 的 GIF 文件。

13. 此时你应该在临时文件夹中有五张图像，看起来像这样：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_270_0.png)

### 还有更多...

在本教程中，我们看到了如何为容器创建图像，以及图像如何构成容器边框的原理。在下一个教程中，我们将看到如何创建将使用这些图像的容器。

## 使用图像设置容器样式

正如我们在上一教程中所见，使用图像设置容器样式的第一步是创建图像。在本教程中，我们将迈出第二步，通过添加将创建容器图形边框的图像来扩展简单的 ASCX 容器。我们还将演示如何更改字体和文本颜色。

### 准备工作

要跟随本教程，你必须已完成以下教程：

- *创建简单的 ASCX 容器*
- *创建自定义容器图像*

### 如何操作...

1. 首先在测试 DNN 门户中定位默认容器文件夹（`\Portals\_default\Containers`）。

2. 接下来，创建一个新的文件夹来存放容器文件。将此文件夹命名为 `ASCXImageContainer`。

3. 在其中创建新的文件夹来存放图像：`ASCXImageContainer\images`。

4. 将你在上一教程中创建的五张图像复制到此新文件夹。你现在应该有以下文件：

- ContainerUL.gif：容器的左上角
- ContainerUC1.gif：第一个上部居中图像
- ContainerUC2.gif：第二个上部居中图像
- ContainerLL.gif：左下角
- ContainerLR.gif：右下角

5. 将图像放在正确的文件夹后，启动开发工具。

6. 要创建 ASCX 文件，从 **File** 菜单中选择 New File...。

7. 在 New File 对话框中，选择 Web User Control 并单击 Open。

8. 第一步是在文件顶部放置以下指令：

```
<%@ Control language="vb"
    CodeBehind="~/Admin/Containers/container.vb"
    AutoEventWireup="false" Explicit="True"
    Inherits="DotNetNuke.UI.Containers.Container" %>
```

9. 接下来，放置我们需要支持容器的 Skin Objects 的必要注册指令：

```
<%@ Register TagPrefix="dnn" TagName="ACTIONS"
    Src="~/Admin/Containers/SolPartActions.ascx" %>
<%@ Register TagPrefix="dnn" TagName="ICON"
    Src="~/Admin/Containers/Icon.ascx" %>
<%@ Register TagPrefix="dnn" TagName="TITLE"
    Src="~/Admin/Containers/Title.ascx" %>
<%@ Register TagPrefix="dnn" TagName="VISIBILITY"
    Src="~/Admin/Containers/Visibility.ascx" %>
<%@ Register TagPrefix="dnn" TagName="ACTIONBUTTON1"
    Src="~/Admin/Containers/ActionButton.ascx" %>
<%@ Register TagPrefix="dnn" TagName="ACTIONBUTTON2"
    Src="~/Admin/Containers/ActionButton.ascx" %>
<%@ Register TagPrefix="dnn" TagName="ACTIONBUTTON3"
    Src="~/Admin/Containers/ActionButton.ascx" %>
<%@ Register TagPrefix="dnn" TagName="ACTIONBUTTON4"
    Src="~/Admin/Containers/ActionButton.ascx" %>
<%@ Register TagPrefix="dnn" TagName="ACTIONBUTTON5"
    Src="~/Admin/Containers/ActionButton.ascx" %>
```

10. 为了定位图像，我们首先使用一个包含的表格来设置容器的样式，然后包含容器的顶部表格、ContentPane 的位置，然后是另一个表格来容纳容器底部的图像：

```html
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="0" class="ASCXImage_Master">
<tr>
<td class="ContainerUC2">
<table height="27" border="0" cellpadding="0" cellspacing="0">
<tr>
<td valign="top" class="ContainerUL">
<dnn:ACTIONS runat="server" id="dnnACTIONS" />
</td>
<td valign="top" class="ContainerUL">
<dnn:ICON runat="server" id="dnnICON" />
</td>
<td valign="top" class="ContainerUL">&nbsp;
<dnn:TITLE runat="server" id="dnnTITLE" CssClass="ASCXImage_Title"/>
</td>
<td valign="top" class="ContainerUC1">&nbsp;</td>
<td valign="bottom" class="ContainerUC2">&nbsp;</td>
</TR>
</table>
<table width="100%" border="0" align="center" cellPadding="0" cellSpacing="0">
<tr>
<td class="ASCXImage_Content">
<div runat="server" id="ContentPane">
</div>
</td>
</TR>
</table>
<table width="100%" height="21" border="0" cellpadding="0" cellspacing="0">
<tr>
<td valign="middle" class="ContainerLL">&nbsp;</td>
<td valign="middle" class="ContainerLR">&nbsp;</td>
</tr>
</table>
</td>
</tr>
</table>
```

11. 最后，我们创建表格来容纳操作按钮，这些按钮位于边框图像之外：

```html
<table width="100%" height="25" border="0" cellpadding="2"
       cellspacing="0">
    <tr valign="bottom">
        <td align="left" nowrap>
            <dnn:ACTIONBUTTON1 runat="server"
                id="dnnACTIONBUTTON1"
                CommandName="AddContent.Action"
                DisplayIcon="True" DisplayLink="True" />
        </td>
        <td height="22" align="right" nowrap>
            <dnn:ACTIONBUTTON2 runat="server"
                id="dnnACTIONBUTTON2"
                CommandName="SyndicateModule.Action"
                DisplayIcon="True"
                DisplayLink="False" />&nbsp;
            <dnn:ACTIONBUTTON3
            runat="server" id="dnnACTIONBUTTON3"
            CommandName="PrintModule.Action"
            DisplayIcon="True"
            DisplayLink="False" />&nbsp;
            <dnn:ACTIONBUTTON4
            runat="server" id="dnnACTIONBUTTON4"
            CommandName="ModuleSettings.Action"
            DisplayIcon="True"
            DisplayLink="False" />
            <dnn:ACTIONBUTTON5
            runat="server" id="dnnACTIONBUTTON5"
            CommandName="ModuleHelp.Action"
            DisplayIcon="True" DisplayLink="False" />
        </td>
    </tr>
</table>
```

12. 从 **File** 菜单中选择 **Save WebUserControl1.ascx As...**。

13. 浏览到 `ASCXImageContainer` 文件夹并将文件保存为 `ASCXImageContainer.ascx`。

14. 接下来我们创建 CSS 文件，从 **File** 菜单中选择 **New File...**。

15. 在 **New File** 对话框中，选择 **Style Sheet** 并单击 **Open**。

16. 用以下内容替换文件内容：

```css
/* Set the containing width and border color */
.ASCXImage_Master { width: 100%; background-color: transparent;
BORDER: #A0A0A0 1px solid; padding: 0px; }

/* Set the title font */
.ASCXImage_Title { font-family: Impact; font-size: 12px; color:
#ffffff; font-weight: normal; }

/* Set the padding */
.ASCXImage_Content { padding-left: 5px; padding-right: 5px;
padding-top: 5px; padding-bottom: 0px; font-size: 15px; color:
#404040; }

/* Set the images to draw the box around the container */

.ContainerUL { background: url(images/ContainerUL.gif) repeat-x;
height: 27px; }
.ContainerUC1 { background: url(images/ContainerUC1.gif) no-repeat;
width: 80px; height: 27px; }
.ContainerUC2 { background: url(images/ContainerUC2.gif) repeat-x;
height: 27px; }
.ContainerLL { background: url(images/ContainerLL.gif) repeat-x;
height: 27px; }
.ContainerLR { background: url(images/ContainerLR.gif) no-repeat;
width: 100px; height: 27px; }
```

17. 从 **File** 菜单中选择 **Save StyleSheet1.css As...**。

18. 浏览到 ASCXImageContainer 文件夹并将文件保存为 container.css。

19. 你还可以制作新容器的预览图像，例如：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_274_0.png)

20. 将图像文件保存为 ASCXImageContainer.jpg。

21. 要查看新容器在我们网站上的外观，首先以 SuperUser 身份登录。

22. 在 **Admin** 菜单下选择 **Skins**。

23. 从 **Containers:** 下拉列表中选择 **ASCXImageContainer**。


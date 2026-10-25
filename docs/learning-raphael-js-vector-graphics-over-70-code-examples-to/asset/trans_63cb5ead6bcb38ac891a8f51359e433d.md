

这描述了一个从中心白色到边缘黑色的渐变圆：

![](img/d8ec653db4cab13fec8b3f5718174af6_36_0.png)

与线性渐变一样，我们可以为每个颜色定义多个颜色及可选偏移量。例如，字符串 ‘r:#fff-#f00-#00f:90-#000’ 创建了一个从白色到红色再到蓝色的径向渐变，在 90% 的偏移处过渡到边缘的黑色。

绘制径向渐变的起点，称为**焦点**，可以使用 `(<fx>, <fy>)` 语法指定，其中 `fx` 和 `fy` 的值在 0 到 1 之间。元素的中心点的焦点定义为 (0.5, 0.5)。减小或增大 `fx` 值会将焦点在 `x` 轴上向左或向右移动，而增大或减小 `fy` 值会将焦点在 `y` 轴上向上或向下移动。下图显示了改变 `fx` 和 `fy` 的值如何影响焦点的位置：

![](img/d8ec653db4cab13fec8b3f5718174af6_36_1.png)

> 注意，由于 VML 中的一个错误，径向渐变不能应用于 `Path` 元素（参见第 3 章，绘制路径），因此实际上仅适用于本章介绍的基本形状。

## 分组元素

有时我们希望将相同的属性、变换或动画应用于 _多个_ 元素。我们可以在 Raphaël 中使用 set 方法对元素进行分组。请考虑以下形状：

```javascript
var paper = Raphael('my-canvas', 540, 200);
var circle = paper.circle(100, 100, 80);
var rect = paper.rect(205, 40, 120, 120);
var ellipse = paper.ellipse(430, 100, 80, 60);
```

通过创建一个包含对所有这些形状引用的 **set**，我们可以通过在 **set 上** 调用 `attr` 方法而不是对每个单独元素调用，将相同的属性应用于所有形状。我们创建一个包含所有元素的 set，并将其引用保存在名为 `group` 的变量中，如下所示：

```javascript
var group = paper.set();
group.push(circle, rect, ellipse);
```

然后我们将以下属性应用于刚刚创建的 set：

```javascript
group.attr({
    fill: '180-#09c-#fff:30-#f00',
    stroke: '#000',
    'stroke-width': 5
});
```

结果如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_38_0.png)

## 使用文本

在画布上绘制文本，而不是作为带有 CSS 样式的 HTML 标记，允许我们像对待其他形状一样对其进行动画和变换。文本使用 `text` 方法创建，其属性（如 `size` 和 `font-family`）可作为属性进行修改。`text` 方法的定义如下，其中 `text` 参数是我们的文本字符串，并接受标准转义序列 `\n` 作为输入，这会将后续文本放在新行上：

Paper.text(x, y, text)

以下代码使用此方法创建两段文本，然后将它们分组到一个 set 中，以便对两个元素应用 `font-size` 和 `font-family` 属性：

```javascript
var text1 = paper.text(0, 15, 'I am text anchored start.');
text1.attr({
    'text-anchor': 'start',
});
var text2 = paper.text(270, 100, 'I am text\nanchored middle');
text2.attr({
    'text-anchor': 'middle'
});
var group = paper.set(text1, text2);
group.attr({
    'font-size': 20,
    'font-family': 'serif'
});
```

## 嵌入自定义字体

字体转换器和渲染器 Cufón 已被 @font-face 取代，用于常规 Web 字体渲染。但是，由 Cufón 生成的字体具有完全定义的路径，并与 Raphaël 无缝集成，这意味着我们可以以许多与形状相同的方式来操作此类字体。此演示可以在 http://raphaeljsvectorgraphics.com/the-graphical-web/custom-fonts 上查看。

Cufón 支持从 **TrueType (TTF)、OpenType (OTF)、Printer Font Binary (PFB) 和 PostScript** 字体文件生成自定义字体。要使用 Cufón 生成自定义字体，请访问 http://cufon.shoqolate.com/generate/ 并填写字体生成表单。确保在标题 **Customization (for 3rd-party scripts only)** 下的文本框中输入 Raphael.registerFont（或单击其旁边的小 Raphaël 标志），如下图所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_39_0.png)

生成的字体将是一个 JavaScript 文件。您应该在 Raphaël 库之后将其包含在 HTML 文档中，即如下所示：

`<script type="text/javascript" src="raphael.min.js"></script>
<script type="text/javascript" src="MyFont.font.js"></script>`

要在画布上使用该字体，您将使用 print 方法（http://raphaeljs.com/reference.html#Paper.print）。例如，嵌入名为 Pacifico.font.js 的字体：

`<script type="text/javascript" src="Pacifico.font.js"></script>`

我们使用自定义字体在画布上打印文本，如下所示：

```javascript
var text = paper.print(
    50,
    50,
    'Vi veri veniversum vivus vici',
    paper.getFont('Pacifico'),
    38
);
```

这将在画布上的点 (50, 50) 绘制字体大小为 38px 的自定义文本。`getFont` 方法只是引用您嵌入的字体系列的名称，而 Cufón 应该已经负责将字体注册到 Raphaël 中。

属性可以以与我们已经对形状和文本应用属性的相同方式应用于自定义字体文本。例如：

```javascript
text.attr({
    fill: '315-#000-#666',
    'stroke-width': 0
});
```

这将生成如下所示的文本：

![](img/d8ec653db4cab13fec8b3f5718174af6_40_0.png)

## 总结

在本章中，我们在特定绘图区域或画布的背景下介绍了 Raphaël 中的许多基本绘图和绘画示例。您现在应该能够使用该库绘制基本矢量图形，并探索 http://raphaeljs.com 上的文档。

在下一章中，我们将探讨使用路径绘制更复杂的元素。

## 3 绘制路径

绘制和操作路径的能力是 Raphaël 的一项非常强大的功能。在上一章中，我们根据路径的几何属性（如宽度、高度和半径）绘制了基本形状。路径允许我们通过定义由线条、弧线和曲线连接的点来绘制各种形状，这些点代表形状本身的轮廓。

路径（如基本形状）是 _元素_，这意味着用户交互、绘制、变换和动画都可以应用于路径。Raphaël 提供了许多用于绘制和操作路径的方法，因此这将是本书其余部分的重点。

绘制路径的语法由 SVG 规范定义。理解此语法对于创建复杂形状至关重要，在本章过程中，我们将对其非常熟悉。

为了让您了解可以使用路径实现什么，`http://raphaeljs.com/chart.html` 上的演示将向您展示如何使用平滑曲线连接数据点，这在图表应用程序中很常见。确实，复杂的 SVG 插图（例如，使用 Adobe Illustrator 或 Inkscape 等图形包创建的插图）由许多路径组成，一个有趣的演示可以在 `http://raphaeljs.com/tiger.html` 上查看。

本章对路径的覆盖非常全面，因此建议您在每节结束时停下来回顾和实验所涵盖的概念。在本章中，我们将探讨以下主题：

- 路径绘制背后的概念
- moveto、lineto 和 closepath 命令
- 绘制二次和三次贝塞尔曲线
- 弧线绘制命令
- Raphaël 中用于处理路径的实用方法
- 由数据插值示例的 Catmull-Rom 曲线

## 路径绘制概念

在纸上用笔绘制的过程可以分解为以下步骤：

1. 您将笔放在纸上的某个特定点上。
2. 您按下笔并从这一点自由移动到另一点。
3. 您将笔保持在此点，或者抬起笔并将其放在纸上的另一点上。
4. 重复该过程，直到您完成绘制。

路径绘制的工作方式大致相同。您放置笔的点，称为 **当前点**，定义了路径的起点，而笔的自由移动描述了路径本身。

考虑绘制一个 arbelos 形状。我们首先将笔放在画布上的点 (100, 180)，并绘制一条弧线到点 (380, 180)，如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_42_0.png)

然后我们从点 (380, 180) 创建一条弧线到点 (200, 180)：

![](img/d8ec653db4cab13fec8b3f5718174af6_42_1.png)


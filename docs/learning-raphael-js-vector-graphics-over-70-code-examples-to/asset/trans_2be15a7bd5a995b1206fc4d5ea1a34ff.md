

应该清楚，圆实际上只是椭圆的一种特殊情况，其中 x 半径等于 y 半径。生成的圆和椭圆如下图所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_28_0.png)

Raphaël 的文档详尽，可在 http://raphaeljs.com/reference.html 查阅。为了全面掌握本书中使用的方法，强烈建议广泛参考该文档。

## 嵌入图像

Raphaël 允许我们将位图（.jpg 或 .png）图像嵌入到画布中。它通过 Paper 对象的 image 方法实现。以下代码演示了嵌入 .jpg 图像：

```
var paper = Raphael('my-canvas', 600, 252);
paper.image('stella.jpg', 15, 15, 144, 192);
```

.jpg 图像位于 (15, 15)，其宽度和高度分别缩放为 144px 和 192px。

## 元素属性

我们绘制的形状可以应用填充、描边以及许多其他属性。当我们创建一个形状时，会返回一个 Element 对象。该对象有一个 attr 方法，接受若干键值对属性。在本节中，我们将探讨可以使用此方法应用于绘图的各种属性（参见 http://raphaeljs.com/reference.html#Element.attr）。

## 基本填充

为了给元素添加背景，我们使用 `fill` 属性进行填充。
对于单色填充，格式遵循 CSS 规范（即，`#rrggbb` 或 `#rgb` 简写，`rgb(r, g, b)` 字符串或颜色关键字，例如 `navy`）。例如，要将矩形填充为黑色，可以编写：

```
var rect = paper.rect(200, 50, 200, 100);
rect.attr({fill: '#000'});
var square = paper.rect(450, 50, 50, 50);
square.attr({fill: 'rgb(0, 0, 0)'});
```

同样，要将圆形填充为粉色，可以编写：

```
var circle = paper.circle(100, 100, 50);
circle.attr({fill: 'pink'});
```

结果如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_29_0.png)

## 图像填充

Raphaël 还允许你使用图像填充元素。考虑以下代码：

```
var circle = paper.circle(100, 100, 80);
circle.attr({fill: 'url(bg_pattern.png)'});
```

这将创建一个圆形，其填充为名为 `bg_pattern.png` 的模式图像，如下图所示。图像填充始终在 `x` 和 `y` 方向重复：

![](img/d8ec653db4cab13fec8b3f5718174af6_30_0.png)

## 应用描边

元素可以有多种不同的描边属性。最常见的是 `stroke` 和 `stroke-width` 属性。`stroke` 属性采用符合 CSS 规范的字符串颜色值，而 `stroke-width` 是以像素为单位的数字。以下代码的效果是给圆形应用 10px 宽的石灰绿色描边：

```
var circle = paper.circle(100, 100, 70);
circle.attr({
    fill: '#09c',
    stroke: 'limegreen',
    'stroke-width': 10
});
```

生成的圆形如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_30_1.png)

描边是居中绘制的，这意味着可见描边宽度的一半在元素内部，另一半在元素外部。

你会注意到本章前面的一些示例中，即使我们没有应用属性，它们仍然被描边了。这是因为默认的 stroke-width 值为 1px。要在绘制形状时移除描边，必须始终指定 stroke-width 值为 'none'。

其他一些有用的描边属性包括 stroke-dasharray 和 stroke-linejoin，如下所示。前者允许我们指定由虚线、点和空格组成的模式，生成的描边将使用它们绘制，而后者决定路径线条应如何连接在一起：

![](img/d8ec653db4cab13fec8b3f5718174af6_31_0.png)

你可以在 http://raphaeljs.com/reference.html#Element.attr 和 http://www.w3.org/TR/SVG/painting.html#StrokeProperties 上了解更多关于不同描边属性的信息。

## 其他属性

除了描边和填充之外，创建元素时可能还想使用许多其他属性。我们将在本节中演示其中一些。

## href

我们创建的元素已注册到 DOM 中，指定 href 属性允许它们表现为超链接。例如：

```
var rect = paper.rect(30, 30, 140, 140);
rect.attr({
    href: 'http://www.packtpub.com'
});
```

这将创建一个矩形，点击时会链接到 Packt Publishing 网站。

## opacity

元素的不透明度可以设置为 0（完全透明）到 1（完全不透明）之间的值。我们还可以使用 `stroke-opacity` 和 `fill-opacity` 属性分别设置描边和填充的不透明度：

![](img/d8ec653db4cab13fec8b3f5718174af6_32_0.png)

描边始终居中绘制的一个不幸副作用是，一半的描边与填充重叠，这意味着 `stroke-opacity` 的适用性有限。

> 将 'stroke-opacity': 0.5 与与填充颜色匹配的描边颜色结合使用，可以使形状边缘非常平滑，实际上对其进行抗锯齿处理。

## clip-rect

Raphaël 支持使用 `clip-rect` 属性对元素进行矩形裁剪。裁剪允许我们只显示元素的一部分。我们定义矩形裁剪区域如下：

```javascript
var circle = paper.circle(100, 100, 80, 80);
circle.attr({
    fill: 'pink',
    'stroke-width': 0,
    'clip-rect': '20 20 80 80'
});
```

这将创建一个半径为 80px、中心位于点 (100, 100) 的圆。`clip-rect` 属性定义一个矩形区域，由空格分隔的字符串形式如下：

```
"x y width height"
```

这将只显示圆中被矩形裁剪区域 '20 20 80 80' 重叠的部分：

![](img/d8ec653db4cab13fec8b3f5718174af6_33_0.png)

## 应用渐变

Raphaël 支持对元素应用线性和径向渐变填充。要实现这一点，我们不在 fill 属性上指定颜色字符串，而是指定以下形式的字符串以创建线性渐变：

```
`<angle>-<color>[-<color>[:<offset>]]*-<color>`
```

以下语法用于创建径向渐变：

```
`r[(<fx>, <fy>)]<color>[-<color>[:<offset>]]*-<color>`
```

这种语法最好通过示例来说明。

## 线性渐变

最基本的线性渐变填充形式如下：

```
`<angle>-<color1>-<color2>`
```

这指定了一个从 color1 到 color2 按 angle 度角绘制的渐变。例如：

```javascript
var rect = paper.rect(20, 20, 160, 160);
rect.attr({
	'stroke-width': 0,
	fill: '0-#f00-#000'
});
```

这将绘制一个矩形，其线性渐变从红色到黑色，角度为 0 度，如下图左侧所示。右侧的矩形使用填充值 '90-#f00-#000' 创建，这次渐变以 90 度角绘制：

![](img/d8ec653db4cab13fec8b3f5718174af6_34_0.png)

请注意，绘制角度是从正 x 轴逆时针定义的，如下图所示，适用于从蓝色到白色的 45 度渐变：

![](img/d8ec653db4cab13fec8b3f5718174af6_34_1.png)

线性渐变可以由任意数量的颜色组成，颜色构成渐变的点定义为 **offsets**。这就是以下语法的含义：

[-<color>[:<offset>]]*

这里可以创建任意数量的 -color:offset 组合。例如，考虑使用以下字符串创建的渐变：

fill: "0-#00a9e0-#323490:20-#ea1688:40-#eb2e2e:60-#fde92d:80-#009e54"

这将创建类似彩虹的图案，如下图所示。每种颜色相对于前一种颜色偏移额外的 20 个百分点。第一种和最后一种颜色的隐含偏移量分别为 0 个百分点和 100 个百分点：

![](img/d8ec653db4cab13fec8b3f5718174af6_35_0.png)

从技术上讲，由于每个偏移量之间的差异对所有颜色都相等（20 个百分点），我们可以在前面示例的字符串中省略偏移量值。以下表达式描述的渐变产生相同的结果：

```
"0-#00a9e0-#323490-#ea1688-#eb2e2e-#fde92d-#009e54"
```

## 径向渐变

径向渐变与线性渐变类似，只是它们从元素上的固定点（默认为元素中心点）_向外辐射_绘制。最基本的径向渐变由语法 `r<color>-<color>` 给出。例如：

```
var circle = paper.circle(100, 100, 80); circle.attr({
'stroke-width': 0,
fill: 'r#fff-#000'
});
```


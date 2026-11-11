
-   **strokeColor**: 描边颜色，六位 RGB 值。示例：`#000000` 为黑色（`00` 红色，`00` 绿色，`00` 蓝色。）`#ffffff` 为白色。
-   **strokeDashstyle**: 应用于描边的样式类型。可以是 `'dot'`、`'dash'`、`'dashdot'`、`'longdash'`、`'longdashdot'` 或 `'solid'`。
-   **strokeLinecap**: 应用于线端点的样式类型。可以是 `'butt'`、`'round'`、`'square'`。
-   **strokeOpacity**: 描边的不透明度，范围从 0 到 1（1 为完全不透明，0 为完全透明）。
-   **strokeWidth**: 描边宽度，以像素为单位。

## 行动时间 – 常见样式示例

通过示例来看看这些常见属性的作用。我们还将演示如何使用两种不同的意图——`default` 意图和 `selected` 意图。

1.  复制上一个示例的文件。我们将使用相同的代码，但稍作修改特征样式。我们还将使用 `select` 意图，因此需要两个样式对象。首先让我们重新创建默认样式对象：

```
var vector_style = new OpenLayers.Style({
  'cursor': 'pointer',
  'fillColor': '#787878',
  'fillOpacity': .8,
  'fontColor': '#343434',
  'pointRadius': 14,
  'strokeColor': '#232323',
  'strokeDashstyle': 'dot',
  'strokeWidth': 3
});
```

2.  现在，让我们创建一个在要素被选中时使用的样式对象。我们将边框改为实线，改变填充颜色，稍微增加点的半径，将 `graphicName` 改为 `'square'`，并添加一个将显示 'X' 的标签。

```
var vector_style_select = new OpenLayers.Style({
  'fillColor': '#ffffff',
  'fillOpacity': .9,
  'graphicName': 'square',
  'label': 'X',
  'pointRadius': 16,
  'strokeColor': '#343434',
  'strokeDashstyle': 'solid',
'strokeWidth': 4
});
```

3.  现在更新 StyleMap 对象以包含新添加的 `vector_style_select` 样式对象：

```
var vector_style_map = new OpenLayers.StyleMap({
    'default': vector_style,
    'select': vector_style_select
});
```

4.  最后，我们只需添加一个 `selectFeature` 控制，以便能够选择要素。

```
//Add a select feature control
var select_feature_control = new OpenLayers.Control.SelectFeature(
    vector_layer
);
map.addControl(select_feature_control);
//Activate the control
select_feature_control.activate();
```

5.  现在，查看地图并选择一个要素。你应该会看到样式发生变化：

![](img/1b8cd151c9f278555f9cf255b20d244e_303_0.png)

## 发生了什么？

我们练习了使用常见的符号化属性，并演示了如何向 StyleMap 对象添加多个 Style 对象并使用两种意图。到目前为止，你可能不会太印象深刻。我们对一些要素进行了样式设置，虽然可以进行大量自定义，但我们还没有做任何太花哨的事情。

## 亲自尝试 – 样式图层

现在你对如何使用符号化器有了一些了解，打开上一个示例并更改一些符号化器。测试不同的属性和值组合——不同的颜色、不透明度、背景颜色等。看看你是否能想出比灰色和黑色圆圈更好看的东西。

在进入属性替换——我们下一个主题之前，让我们看看尚未涵盖的剩余符号化属性。

## 剩余的符号化属性

-   **backgroundGraphic**: 指定要使用的图像的 URL。这将放置在 `externalGraphic` 之下（如果已设置）。
-   **backgroundGraphicZIndex**: 指定 `backgroundGraphic` 的 z-index（相互放置的顺序）。
-   **backgroundHeight**: 确定背景图像的高度。如果未指定，则使用 `graphicHeight`。
-   **backgroundWidth**: 背景图像的高度，如果存在的话。如果未设置此属性且提供了图像，则会尝试使用 `graphicHeight` 属性。
-   **backgroundXOffset**: 指定在 X 方向上偏移图像的距离（以像素为单位）。
-   **backgroundYOffset**: 指定在 Y 方向上偏移图像的距离（以像素为单位）。
-   **display**: 如果设置为 `none`，则不会应用符号化样式。
-   **externalGraphic**: 指定当要素点显示时要使用的图像的 URL（而不是描边和填充属性）。
-   **fill**: 如果设置为 `false`，则不会显示填充。
-   **graphic**: 设置为 `false` 将禁用任何图形的显示。
-   **graphicHeight**: 外部图形的高度（以像素为单位）（如果存在）。
-   **graphicOpacity**: 指定关联图形的不透明度，范围从 0 到 1（1 为完全不透明，0 为完全透明）。
-   **graphicTitle**: 这指定了在鼠标悬停在外部图形上时出现的工具提示文本（如果提供了图形）。
-   **graphicWidth**: 外部图形的宽度（以像素为单位）（如果存在）。
-   **graphicXOffset**: 在 X 方向上偏移外部图形的距离（以像素为单位）。
-   **graphicYOffset**: 在 Y 方向上偏移外部图形的距离（以像素为单位）。
-   **graphicZIndex**: 指定外部图形的 z-index。较高的 z-index 意味着它会显示在 z-index 较低的元素之上。
-   **rotation**: 指定要素围绕其中心顺时针旋转的程度。值范围为 0 到 360，默认为 0。值为 180 时为倒置。

## 小测验 – 确定使用哪些属性

指定图像有两种方法：使用 `background` 或使用 `externalGraphic` 属性。当一起使用它们时，重要的是记住它们各自的工作方式。如果我们想显示一个标记的图像，以及标记阴影的单独图像，我们该怎么做？

## 属性替换

使用要素样式可以做的一件简单但强大的事情称为 **attribute replacement**。它是一种在图层的样式定义中使用要素的属性的方式，就像变量一样。创建符号化器时，可以将任何属性的值设置为以下格式：

```
${variable_name}
```

其中 `variable_name` 是包含在要素的 `attributes` 属性中的属性的名称。例如，假设我们的所有要素都有一个名为 'size' 的属性属性，具有随机整数值。如果我们想将 `pointRadius` 分配给每个要素的 `size` 属性，我们将使用：

```
{ 'pointRadius': ${size} }
```

然后发生的情况是，当每个要素被样式化时，`${size}` 会被要素的 `attribute.size` 属性替换（如果存在）。进一步解释这一点的最好方法是举例说明。让我们先看看，然后再深入探讨。

## 行动时间 – 使用属性替换

让我们演示如何使用属性替换根据要素的属性更改要素的样式。

1.  复制上一个示例。我们将稍作修改以演示属性替换。为此，我们首先需要为要素提供一些可以访问的属性。修改 `for` 循环中的 `addFeature` 代码以按如下方式添加属性：

```
for(var i=0; i<10; i++) {
    vector_layer.addFeatures([new OpenLayers.Feature.Vector(
new OpenLayers.Geometry.Point(
    (Math.floor(Math.random() * 360) - 180),
    (Math.floor(Math.random() * 180) - 90)
),
{
    //Attributes go here
    size: 5 + (Math.floor(Math.random() * 20)),
    label: 'F' + i,
    strokeWidth: (Math.floor(Math.random() * 10))
}
]);
}
```

2.  现在，我们只需要修改 vector_style 样式对象以包含属性替换，以便我们可以使用刚刚创建的要素的 size、label 和 strokeWidth 属性。执行此操作的语法是 '${variable_name}'，其中 variable_name 是我们想要使用的要素属性的名称。按如下方式修改 vector_style 对象，修改 label、pointRadius、strokeDashStyle 和 strokeWidth：

```
var vector_style = new OpenLayers.Style({
    'cursor': 'pointer',
    'fillColor': '#787878',
    'fillOpacity': '.8',
    'fontColor': '#343434',
    'label': '${label}',
    'pointRadius': '${size}',
    'strokeColor': '#232323',
    'strokeDashstyle': 'solid',
    'strokeWidth': '${strokeWidth}'
});
```

3.  打开地图。我们使用了随机值，因此刷新地图时每次都应看到不同的内容。你应该会看到类似下面的内容，但点的位置和大小不同：

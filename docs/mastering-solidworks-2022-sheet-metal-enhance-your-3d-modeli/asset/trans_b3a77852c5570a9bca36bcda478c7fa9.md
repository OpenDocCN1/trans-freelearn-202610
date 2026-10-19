

Jog 特性与 Sketched Bends 的创建方式非常相似：

1. 打开上一节中的 Base Flange 零件并删除多余的法兰、切割或凸片，使其仅保留 Base Flange。或者，你也可以新建一个零件文档并创建一个尺寸为 200x300mm、厚度为 1mm 的 Base Flange。
2. Jogs 需要一个草图来定义 Jog 的位置，因此在 Base Flange 的顶面上开始一个草图。
3. 使用 Line 工具在 Base Flange 上直接绘制一条线，使其为垂直方向且距离边缘 100mm（图 3.21）。

![](img/4e9c736de6bac4449096a7b92c28af4c_99_0.png)

图 3.21：为 Jog 特性创建草图

4. 从 Sheet Metal 工具中选择 Jog 特性。注意 Jog 特性也可以在创建草图前选择。然后系统会提示你选择或创建要使用的草图。
5. 选择 Fixed Face。与 Sketched Bend 特性一样，这是不会移动的面。
6. 选择草图线左侧的大区域。将显示黄色预览以显示弯折区域。默认情况下使用 Default Bend Radius 属性，但如果需要可以覆盖。

7. 设置 Jog Offset 属性（图 3.22 中标记为 i）。如果需要，可以通过按 Reverse Direction 按钮来翻转方向。将偏移量设置为 10mm，Blind：

![](img/4e9c736de6bac4449096a7b92c28af4c_100_0.png)

图 3.22：Jog 属性管理器

8. Offset Distance：偏移的实际高度（图 3.22 中标记为 ii）取决于以下三个选项中的哪一个被选中（图 3.22 中标记为 iii）：

- Outside Offset：这使用 Fixed Flange 的上表面与 Jog 区域的上表面之间的距离（图 3.23，左）。
- Inside Offset：这使用 Jog 的两个内表面之间的距离——即 Fixed Flange 的上表面与 Jog 区域的下表面（图 3.23，中间）。
- Overall Dimension：这给出 Jog 的总厚度——即 Fixed Flange 的下表面与 Jog 区域的上表面之间的距离（图 3.23，右）。

![](img/4e9c736de6bac4449096a7b92c28af4c_101_0.png)

图 3.23：Offset Distance 类型；从左到右：Outside Offset、Inside Offset、Overall Dimension

9. Fix projected length 选项（图 3.22 中标记为 iv）允许在法兰上添加额外材料以补偿被 Jog 部分占用的材料。

如果选择 Fix projected length，则 Jog 法兰的长度将与原始 Fixed Flange 相同，但如果取消选择该选项，则 Jog 法兰将比 Fixed Flange 短（图 3.24），因为部分材料被 Jog 本身使用：

![](img/4e9c736de6bac4449096a7b92c28af4c_101_1.png)

图 3.24：Fix projected length 选项

10. Jog Position：实际的 Jog 可以像 Sketched Bend 选项一样指定（图 3.22 中标记为 v）：

- Bend Centerline：这将第一个弯折的中心放置在草图线的位置。
- Material Inside：这将第一个 Jog 弯折放置在 Fixed Face 上草图线的轮廓内。
- Material Outside：这将第一个弯折法兰放置在 Fixed Face 上草图线轮廓之外（使得弯折法兰的内表面与草图线对齐）。然而，部分实际弯折区域仍可能位于 Fixed Face 上的草图线内。
- Bend Outside：这将第一个 Jog 弯折的全部置于草图线之外。这意味着弯折法兰将根据 Bend Radius 大小与草图线略有偏移。

11. Jog Angle：最后，可以设置 Jog Angle 属性（图 3.21 中标记为 vi）。Jog 特性中的两个弯折将使用相同的角度，这意味着 Jog 的上部始终与 Fixed Face 平行。

## 使用 Jogs 的实际示例

Jog 特性对于连接两张薄板同时仍保持平整的外表面特别有用。如果两张薄板相互抵靠并直接连接，则它们实际连接的表面非常小（即板的厚度），因此焊接或其他连接会非常困难（图 3.25）。

![](img/4e9c736de6bac4449096a7b92c28af4c_102_0.png)

图 3.25：没有 Jogs 的情况下薄板可能难以连接

为演示 Jog 特性如何帮助解决此问题：

1. 新建一个零件文档并创建一个尺寸为 100×50mm、厚度为 1mm、弯曲半径值为 1mm 的 Base Flange。
2. 在每个较短的边缘添加 Edge Flanges。这些应为 50mm 长、角度为 90°、Flange Length 类型为 `Outer Virtual Sharp`、Flange Position 为 `Material Inside`。两个边缘可以添加到同一个特性中。这会形成一个 U 形模型。
3. 在之前 Edge Flanges 的顶部边缘添加另外两个 Edge Flanges（图 3.26）。这两个都应为 90° 角度且长度为 49mm（Outer Virtual Sharp 且 Material Inside），以便形成一个粗糙的盒形，但上法兰的末端不会完全接触并留有小间隙：

![](img/4e9c736de6bac4449096a7b92c28af4c_103_0.png)

图 3.26：使用单个 Base Flange 和两组 Edge Flanges 创建零件，形成粗糙的盒形

4. 如果此零件在现实生活中制造，则上 Edge Flanges 的未支撑端可能相当脆弱。理想情况下，我们会将它们连接起来，但极薄的板厚会使焊接变得困难。因此，我们将使用 Jog 特性使法兰重叠。

5. 在其中一个法兰的顶面上绘制一条单线，使其与边缘平行且相距 20mm（图 3.27）。

![](img/4e9c736de6bac4449096a7b92c28af4c_104_0.png)

图 3.27：为 Jog 特性绘制线条

6. 在草图内选择 Jog 工具。这会将当前草图用于 Jog 特性。我们也可以先选择 Jog 工具，然后再绘制草图。

7. 在 Jog 工具中选择 Fixed Face。这将是法兰中较大的区域，位于法兰的弯曲侧（图 3.27 中的左侧部分）。

8. 输入以下参数：

- Jog Offset：Blind，1.5mm（朝向 Fixed Flange）
- Outside Offset
- Fix projected length：On
- Jog Angle：Bend Centerline
- Jog Angle：90°

9. 按 OK 创建 Jog。注意你可能会收到一条消息（图 3.28）警告 Jog Offset 值不正确。按 OK 仍创建 Jog，我们稍后查看问题：

![](img/4e9c736de6bac4449096a7b92c28af4c_105_0.png)

图 3.28：有时所需偏移距离不会被使用并会显示警告

10. 如果我们从侧面查看模型（图 3.29），可以看到尽管我们设置了 Offset Distance 值为 1.5mm，但 Jog 实际创建的 Offset Distance 值为 3mm。

这是因为 Jog 使用了 1mm 的 Default Bend Radius 值；因此，Jog 由两个 90° 弯折组成，每个弯折都不能小于 1mm。加上板厚就得到我们实际的 3mm 偏移。

因此，请注意 Jog 偏移距离有时可能与你认为指定的不同：

![](img/4e9c736de6bac4449096a7b92c28af4c_106_0.png)

图 3.29：有时 Jog 偏移距离会大于指定的值

11. 要减小偏移距离，我们必须减小 Bend Radius 值或调整 Jog Angle 值。

编辑 Jog 特性并使用 Offset Distance 框旁的向下箭头减小 Bend Angle 从 90°。注意随着角度减小，预览中的 Offset Distance 值也会减小，因为我们实际上减小了 Jog 中弯折的高度。

在某个点（本例中为 60°），预览中的间隙将不再减小。这表明我们可以使用以达到正确偏移距离 1.5mm 的最大角度。


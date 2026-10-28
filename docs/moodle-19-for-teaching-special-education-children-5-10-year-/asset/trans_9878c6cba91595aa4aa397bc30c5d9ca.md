

**14.** 选择 **文件 | 另存为**，为新的被修改的矢量图形保存一个新名称。输入 `image010103` 作为新名称并点击 **保存** 按钮。

**15.** 选择 **文件 | 导出位图**。一个显示许多导出选项的对话框将会出现。点击 **绘图** 按钮，然后点击 **导出**。Inkscape 将以 **PNG** 格式导出编辑后的绘图。导出的 **位图图形** 文件带有小写字母和一对小剪刀的图标为 `C:\School\image010103.png`。

**16.** 接下来打开之前被修改的矢量图形文件在 Inkscape 中（`C:\School\image010101.svg`）。

**17.** 重复上述步骤（11 到 15）来编辑矢量图形并导出一张新的位图，上面有一块小黑板和一块小橡皮擦，如下一图所示。使用 `image010104` 作为新的 Inkscape 文件。因此，新的导出的位图图形将会是 `C:\School\image010104.png`。

![](img/a18a528f2a8e54d48b64e97eaf62e865_35_0.png)

## *发生了什么？*

我们使用 Inkscape 编辑了两对 2D 可缩放剪贴画图片。由于我们使用矢量图形创建了绘图，因此我们可以改变其大小而不损失质量，并且我们将 resulting 图像导出为 PNG 格式。

> > PNG 是一种开放的、可扩展的图像格式，具有 **无损压缩**。Moodle 和 Hot Potatoes 都能很好地处理 PNG 图像。我们没有为这些图形使用 **JPEG**（联合图像专家组）格式，因为它使用 **有损压缩** 方法，会从图像中移除一些信息。由于导出的图像尺寸很小，我们可以使用 PNG 格式。

我们现在有以下四个位图图形准备用于我们的匹配复合图片练习：

- `image010101.png`：一块大黑板和一块大橡皮擦
- `image010102.png`：一个大箱子和一对大剪刀
- `image010103.png`：一个小箱子和一对小剪刀
- `image010104.png`：一块小黑板和一块小橡皮擦

## 行动时间 — 使用颜色和字体准备文本

首先，我们必须下载并安装 Hot Potatoes 6.3。接下来，我们将为我们的匹配复合图片练习准备句子。

- 1. 如果你还没有，下载并安装 Hot Potatoes 6.3（`http://hotpot.uvic.ca/#downloads`）。
- 2. 启动 Hot Potatoes 并点击 **JMatch**。一个新的窗口会显示 JMatch 应用程序。
- 3. 在 **标题** 文本框中输入 `Matching composite pictures`。
- 4. 点击相应的 **右侧（乱序）项目** 文本框并为每一行输入下一表中显示的文本，如下一张截图所示：

| 行号 | 在右侧（乱序）项目文本框中输入的文本 |
|---|---|
| 1 | `<h2 style="color:blue"><br>&nbsp;&nbsp;&nbsp;&nbsp;<em>There is a big blackboard and a big eraser</em><br></h2>` |
| 2 | `<h2 style="color:blue"><br>&nbsp;&nbsp;&nbsp;&nbsp;<em>There is a big case and a pair of big scissors</em><br></h2>` |
| 3 | `<h2 style="color:green"><br>&nbsp;&nbsp;&nbsp;&nbsp;<em>There is a small case and a pair of small scissors</em><br></h2>` |
| 4 | `<h2 style="color:green"><br>&nbsp;&nbsp;&nbsp;&nbsp;<em>There is a small blackboard and a small eraser</em><br></h2>` |

## 发生了什么？

我们安装了 Hot Potatoes 6 并使用拖放格式为我们的匹配复合图片练习准备了句子。

我们使用 JMatch 编辑并预览了以下四个句子作为右侧的项目：

- There is a big blackboard and a big eraser
- There is a big case and a pair of big scissors
- There is a small case and a pair of small scissors
- There is a small blackboard and a small eraser

![](img/a18a528f2a8e54d48b64e97eaf62e865_38_0.png)

## 使用 HTML 标签定义颜色和字体

我们不想使用默认的字体和颜色。我们添加了 HTML 代码来为上述句子指定不同的颜色。JMatch 没有提供简单的方法来为句子选择字体和颜色。然而，它允许我们使用标准的 HTML 标签。因此，我们使用了以下标题来指定一个蓝色颜色的二级标题样式和强调文本：

```
<h2 style="color:blue"><em>
```

接下来，我们输入了句子的纯文本：

```
There is a big blackboard and a big eraser
```

最后，我们使用了以下页脚：

```
</em></h2>
```

我们对其他句子使用了相同的标签。我们只是将绿色着色的句子中的 `color:blue` 代码替换为 `color:green`。

> HTML 标签有点复杂。然而，你不需要掌握 HTML 标准就能创建一个漂亮的匹配复合图片练习。你可以把颜色名称替换为其他纯色，例如红色、黑色或白色。

## 使用红色、绿色和蓝色分量定义自定义颜色

如果我们需要其他颜色，我们可以使用动态 HTML 颜色代码图表或 HTML 颜色选择器，例如 HTML Color Codes 网站提供的，http://html-color-codes.info。

我们可以点击图表中的一个颜色框，网站将显示所选的颜色代码，如下一张截图所示：

## 行动时间 – 组织复合图片和文本

我们现在必须将带有不同尺寸的复合图片添加到 JMatch 中的练习中：

- 1. 保持在 JMatch 练习 `matching0101` 中。
- 2. 点击第一个 **左侧（有序）项目** 文本框。
- 3. 从 JMatch 的主菜单中选择 **插入 | 图片 | 从本地文件插入图片**。选择 `C:\School` 中之前导出的位图图像 `image010101.png` 并点击 **打开** 按钮。一个对话框将会出现，显示图像预览和一些选项，如下一张截图所示：

![](img/a18a528f2a8e54d48b64e97eaf62e865_41_0.png)

- 4. 点击 **确定**。JMatch 将在文本框中显示以下 HTML 代码：
  ```html
  <img src="image010101.png" alt="image010101.png"
       title="image010101" width="129" height="182"></img>
  ```
- 5. 编辑文本框中的 HTML 代码，添加以下标题（前缀）。我们想要为图片指定一个背景颜色。
  ```html
  <h2 style="background:blue">
  ```

## 6.

添加以下页脚（后缀）：

```
```html
</h2>
```
```

## 7.

文本框中显示的 HTML 代码应如下所示：

```
```html
<h2 style="background:blue">
<img src="image010101.png" alt="image010101.png"
     title="image010101" width="129" height="182"></img></h2>
```
```

## 8.

点击相应的 **左侧（有序）** 项目文本框并重复上述步骤（2 到 8），使用下一表中显示的信息。你必须用每一行指定的颜色替换在 `background:` 标签后输入的颜色名称。

| 行号 | 图片文件名 | 背景颜色名称 |
|---:|---|---|
| 2 | `image010102.png` | blue |
| 3 | `image010103.png` | green |
| 4 | `image010104.png` | green |

## 9.

**左侧（有序）** 项目文本框必须显示每张图片的 HTML 代码，如以下截图所示：

![](img/a18a528f2a8e54d48b64e97eaf62e865_42_0.png)

- 10. 保存更改并选择 文件 | 创建网页 | 拖放格式。替换现有的网页并点击 在我的浏览器中查看练习。

- 11. 默认网页浏览器将会出现，显示带有拖放格式的匹配练习。你将能够看到四张图片和乱序的句子。两张图片显示蓝色背景（带大物体的图片），另外两张显示绿色背景（带小物体的图片），如下一张截图所示：

![](img/a18a528f2a8e54d48b64e97eaf62e865_43_0.png)

- 12. 重复最后两个步骤。每次匹配练习在网页浏览器中出现时，乱序的句子都会改变顺序。

## 发生了什么？

我们将带有不同尺寸的复合图片添加到我们的匹配复合图片练习中。我们在右侧每一句话的左侧插入了相应的图片。

当我们运行由 JMatch 创建的网页加载的练习时，它会打乱屏幕右侧的句子。

## 使用 HTML 标签定义背景颜色

我们不想使用图片的默认背景颜色。我们添加了 **HTML 代码** 来为上述复合图片指定不同的背景颜色。如前面在为句子中的文本定义颜色时所解释的，JMatch 没有提供简单的方法来为图片选择背景颜色。因此，我们不得不再次使用 HTML 标签。我们使用了以下标题来指定一个带有蓝色背景颜色的 `二级标题` 样式：

```
```html
<h2 style="background:blue">
```
```

我们没有更改 JMatch 添加来插入图片的 HTML 代码，最后我们使用了以下页脚：

```
```html
</h2>
```
```

我们对其他图片使用了相同的标签。我们只是将背景颜色为绿色的图片中的 `backgroundcolor:blue` 代码替换为 `backgroundcolor:green`。

> > 我们关于为文本前景色（`color` 属性）定义自定义颜色代码所学到的所有内容也适用于背景颜色属性。


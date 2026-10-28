

8. 你现在在标题块中有足够的空间添加另一个缩放视口。点击上下文的 **Layout** 功能区选项卡（在 AutoCAD 功能区上显示为蓝色）。这是因为你正在绘图的 **ISO A3 layout** 选项卡中。

9. 在 **Layout Viewports** 面板上，选择顶部右侧下拉菜单中的 **Rectangular** 视口类型。它是默认类型，因此可能已经显示。你也可以输入 `VIEWPORT` 并按 *Enter*。视口对话框将会出现。在对话框中选择 **New Viewports** 选项卡将为你提供各种视口布局的选项，包括单个、两个垂直、水平等。**Figure 8.10** 显示了 **Layout Viewports** 面板以及如何使用功能区选择 **Rectangular** 视口类型。

![](img/e5541e806f2ef46a4958b6c3d0b346a9_212_0.png)

Figure 8.10: The contextual Layout Viewports panel with the viewport type drop-down menu displayed

10. 你现在会看到 **Specify corner of viewport** 提示。使用对象捕捉，在现有视口下方放置一个新的视口。使用现有视口的左下角（**Endpoint** 捕捉）和标题块描述框的左下角（**Endpoint** 捕捉），如 **Figure 8.11** 所示。点击第二个点后，视口将自动放置。

![](img/e5541e806f2ef46a4958b6c3d0b346a9_213_0.png)

Figure 8.11: The new viewport with selected corners arrowed

11. 视口放置后会立即显示 **Model** 选项卡中内容的缩放范围视图。双击新视口边界内以激活它。你会看到视口边界高亮，并且现在可以像在 **Model** 选项卡中绘图一样在视口内工作。这在口语中被称为 *floating modelspace*。*Figure 8.12* 显示了激活的视口。当视口处于活动状态时，导航工具如 **Navigation Bar**、**ViewCube** 和 **UCS icon** 会显示在活动视口内。

13. 通过点击比例值旁边的飞出箭头，在状态栏上选择视口比例菜单。你刚刚在新视口中手动缩放，因此状态栏比例不会显示为已识别的比例值。从显示的比例列表中选择 **1:30**，如 *Figure 8.14* 所示。新视口中的视图将调整为所选比例，并且状态栏中将显示 **1:30**。

![](img/e5541e806f2ef46a4958b6c3d0b346a9_215_0.png)

Figure 8.14: The viewport scale list with the flyout arrow and 1:30 scale display arrowed

14. 你现在已使用来自 **Model** 选项卡的所需视图并按标准比例（**1:30**）设置了新视口。将光标移至 *图幅边界之外*（进入灰色区域）并双击。这将取消激活新视口。

> **Tips and tricks #26**
> 
> 一旦视口放置并缩放后，可以将其锁定以防止因错误而造成的任何更改。例如，用户可能会不小心双击视口边界并进行缩放和平移，从而改变视口比例。最简单的方法是在布局选项卡中选择视口时使用状态栏上的挂锁图标。这在 *Figure 8.15* 中显示。当挂锁图标为蓝色时，视口处于锁定状态。

![](img/e5541e806f2ef46a4958b6c3d0b346a9_216_0.png)

Figure 8.15: The padlock on the status bar, set to blue, indicating that the selected viewport is currently locked.

一旦视口被锁定，其比例将无法更改。这可避免用户因错误激活视口并在视口处于活动状态时缩放而导致视口比例意外更改。

> **AutoCAD for Mac**
> 
> 本节提到的所有视口功能在 AutoCAD for Mac 中以相同方式工作。它仍然位于状态栏上，但挂锁/锁定图标使用 MacOS 图标规范，看起来略有不同，如 *Figure 8.16* 所示：

![](img/e5541e806f2ef46a4958b6c3d0b346a9_216_1.png)

Figure 8.16: The viewport padlock displayed on the AutoCAD for Mac status bar

你现在知道如何使用现有视口以及如何创建和缩放新视口。了解更多关于视口的良好方法是尝试它们，并在绘图的布局选项卡中放置和缩放它们时发现其行为。一个典型的例子可能是设置一个 1 : 100 视口，然后将其重新缩放为 1 : 50。你会看到 1 : 50 视口显示更多细节，因为它是 1 : 100 比例的一半。尝试使用不同的比例值，然后在选中时使用蓝色夹点调整视口大小。

在本章的下一部分，你将学习如何在 Model 选项卡中利用注释，确保其在缩放视口中有效显示。

## Working with annotation scales in the Model tab

你应该已经从上一节打开了 `Office_Plan_GROUND_FLOOR.dwg` 练习文件。你现在将在 **Model** 选项卡中添加一些注释性标注，使用适当的注释比例并确保其与所选缩放视口匹配。步骤如下：

1. 确保你处于绘图的 **Model** 选项卡中，并且已缩放至平面图中 **Executive Office** 区域，如 *Figure 8.17* 所示：

![](img/e5541e806f2ef46a4958b6c3d0b346a9_217_0.png)

Figure 8.17: Zoomed into the Executive Office on the Model tab

2. 你需要在绘图的 **Executive Office** 区域放置一些标签，因此需要使用适当的文字样式。使用功能区 **Home** 选项卡上的 **Annotation** 面板，使用面板标题栏上的飞出菜单。然后你可以使用文字样式飞出菜单确保使用适当的注释文字样式。你需要使用 `Labels_MODEL_5mm`，如 *Figure 8.18* 所示。如果有帮助，你可以固定 **Annotation** 面板飞出菜单以防止其消失回功能区。它在 *Figure 8.18* 中也显示为固定打开状态：

![](img/e5541e806f2ef46a4958b6c3d0b346a9_218_0.png)

Figure 8.18: The Annotation panel pinned open with the text style arrowed

3. Labels_MODEL_5mm 文字样式是注释性的（在飞出菜单中旁边有蓝色注释符号），因此它将采用 Model 选项卡中状态栏上设置的当前注释比例。为了使注释文字在上一节创建的 1:30 视口中按预期显示，注释比例也必须设置为 1:30。选择状态栏上的注释比例飞出菜单并选择 1:30 比例，如 Figure 8.19 所示：

![](img/e5541e806f2ef46a4958b6c3d0b346a9_218_1.png)

Figure 8.19: The annotation scale set to 1:30 in the status bar

4. 你现在可以在 Executive Office 区域放置注释文字作为房间标签。使用图层面板上的图层下拉菜单确保使用 Text 图层作为当前图层。

5. 从功能区 **Annotation** 面板上的文字飞出菜单中选择多行文字选项，如 Figure 8.20 所示：

![](img/e5541e806f2ef46a4958b6c3d0b346a9_219_0.png)

Figure 8.20: The Multiline Text (MTEXT) option on the text flyout in the Annotation panel

6. 你将被提示放置多行文字（MTEXT）的矩形区域。在 Executive Office 的右上角点击 Figure 8.21 指示的两个点。这将创建放置文字的区域。注意此时你的文字样式预览（在放置的多行文字区域中由 abc 表示）。

![](img/e5541e806f2ef46a4958b6c3d0b346a9_219_1.png)

Figure 8.21: The Multiline Text area with the text preview displayed

7. 放置文字区域后，你会看到光标闪烁，并且功能区上会出现一个名为 **Text Editor** 的上下文选项卡。功能区选项卡为蓝色；你会看到所有多行文字编辑工具。**Text Editor** 显示在 *Figure 8.22* 中。它就像 Microsoft Word 等文字处理应用程序一样工作。为了了解其工作原理，在 **Text Editor** 中输入以下内容：

- Executive (press *Enter*)
- Office

显示的文字现在将与 *Figure 8.22* 中显示的相同：

![](img/e5541e806f2ef46a4958b6c3d0b346a9_220_0.png)

8. 要关闭 **Multiline Text**（*MTEXT*）命令，你可以点击 **Text Editor** 外部，或点击 **Text Editor** 上下文功能区选项卡上显示的 **Close Text Editor** 图标，该图标为绿色勾号。文字现在将按输入方式显示在绘图中，并且功能区上的 **Text Editor** 选项卡将关闭。

9. 双击鼠标滚轮作为按钮，你将在 **Model** 选项卡中缩放至绘图中可见对象的范围。滚动鼠标滚轮向后以缩小并在平面图周围留出一些空间，以便更容易放置标注。你现在将在平面图周围放置一些注释标注。

10. 如前所述，你需要在 **Model** 选项卡中设置注释比例以适应标注将显示的视口中的视口比例。

11. 在状态栏上的注释比例飞出菜单中将注释比例设置为 **1:50**。

12. 使用 **Layers** 面板上的图层下拉菜单，确保将 **Dimensions** 图层设置为当前图层。

13. 你还需要确保使用正确的注释性标注样式。点击 **Annotation** 面板标题栏上的飞出菜单并在需要时将其固定打开（如 *步骤 2*）。确保你使用的是 **Training-ANNO** 标注样式。它旁边有蓝色注释符号，如 *Figure 8.23* 所示。


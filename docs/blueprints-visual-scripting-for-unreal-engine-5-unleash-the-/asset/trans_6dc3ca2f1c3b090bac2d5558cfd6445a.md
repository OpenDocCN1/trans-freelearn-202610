

- 10. 从 Set Instance Location X 的白色输出引脚拖出一条线，并添加 For Loop 节点。在 First Index 参数中输入 1。从 Last Index 参数拖出一条线，并添加 Get Instances Per Row 节点。

- 11. 添加 Get Space Between Instances 节点。从 Index 输出节点拖出一条线，并添加 Multiply 节点。从 Get Space Between Instances 拖出一条线，并将其连接到 Multiply 节点的底部引脚。该引脚将从整数转换为浮点数。

- 12. 添加 Get Instanced Static Mesh 节点。从其输出引脚拖出一条线，并添加 Add Instance 节点。将 Loop Body 输出引脚连接到 Add Instance 节点的白色输入引脚。

- 13. 右键单击 Instance Transform 参数并选择 Split Struct Pin。右键单击 Instance Transform Location 参数并再次选择 Split Struct Pin。

- 14. 从 Instance Transform Location X 拖出一条线，并添加 Get Instance Location X 节点。

- 15. 将 Multiply 节点的输出引脚连接到 Instance Transform Location Y 的输入引脚。

- 16. 编译蓝图并关闭蓝图编辑器。从内容浏览器中拖动 **BP_ProceduralMeshes** 并将其放置在关卡中。

- 17. 在关卡编辑器的 **详情** 面板中，将每行实例数设置为 10，将行数设置为 10：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_646_0.png)

图 19.12 – 在关卡上设置 BP_ProceduralMeshes 属性

- 18. 程序化生成的结果可以在下一个截图中看到：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_646_1.png)

图 19.13 – 生成 10 行 10 列的椅子

- 19. 我们可以使用同一个 **BP_ProceduralMeshes** 实例生成种植园。在关卡编辑器的 **详情** 面板中，将 **静态网格变量** 更改为 `SM_Bush`，并将 **实例间距** 设置为 `300.0`，将 **行间距** 设置为 `300.0`：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_647_0.png)

图 19.14 – 修改关卡上的 BP_ProceduralMeshes 属性

- 20. 这是使用新参数的程序化生成结果：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_647_1.png)

图 19.15 – 生成种植园

可以看出，参数的微小变化会产生非常不同的结果。这个示例只是可用程序化生成功能的冰山一角。

在下一节中，我们将学习如何沿路径生成实例。

## 创建蓝图样条

样条是一种用于定义曲线的特殊数学函数。蓝图样条组件可用于定义路径以在关卡中移动 Actor。我们还可以获取路径沿线的位置来放置实例。我们可以通过添加、平移和旋转样条点，在关卡编辑器中编辑样条。

下一个截图显示了样条组件的三个常用函数：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_648_0.png)

图 19.16 – 样条组件的一些函数

### 函数描述如下：

- Get Spline Length：此函数返回一个浮点数值，表示样条的长度。
- Get Location at Distance Along Spline：此函数将距离作为输入参数，并返回样条中找到的位置。坐标空间可以是本地（相对）或世界。
- Get Rotation at Distance Along Spline：与前一个函数的思路相同，但它返回旋转。

我们将创建一个名为 BP_SplinePlacement 的蓝图，用于沿样条添加静态网格实例。关卡设计师可以在关卡编辑器中编辑样条，并指定实例之间的间距。

### 创建蓝图的步骤如下：

- 1. 在内容浏览器中单击添加按钮，并选择蓝图类选项。在下一个屏幕上，选择 Actor 作为父类。为蓝图命名 BP_SplinePlacement 并双击它以打开蓝图编辑器。
- 2. 在 **我的蓝图** 面板中，创建一个名为 StaticMeshVar 的变量。单击变量类型并搜索静态网格。将鼠标悬停在静态网格上并选择对象引用。单击眼图标以使变量可实例编辑。
- 3. 编译蓝图。在静态网格变量的 **默认值** 中，选择 SM_FieldArrow 静态网格：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_649_0.png)

图 19.17 – 选择生成中使用的默认静态网格

### 注意

如果 `SM_FieldArrow` 静态网格未出现，请单击内容浏览器的设置图标，并选择 **显示引擎内容**。

- 4. 在 **我的蓝图** 面板中，创建一个名为 `SpaceBetweenInstances` 的 **浮点数** 类型变量。单击眼图标，将该变量设置为 **实例可编辑**。

- 5. 编译蓝图，并将 `SpaceBetweenInstances` 的 **默认值** 设置为 `100.0`。

- 6. 在 **组件** 面板中，单击 + **添加** 按钮并搜索 `spline`。选择 **样条** 组件：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_650_0.png)

图 19.18 – 添加样条组件

- 7. 在 **组件** 面板中选择 DefaultSceneRoot 并添加一个 InstancedStaticMesh 组件：

图 19.19 – BP_SplinePlacement 使用的组件

- 8. 我们将创建一个宏来简化构建脚本图表。在 **我的蓝图** 面板中，创建一个名为 `CalculateNumberOfInstances` 的宏：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_651_0.png)

图 19.20 – 创建宏

- 9. 在 **详情** 面板中，创建一个名为 `Number Of Instances` 的 **输出** 参数，类型为 **整数**：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_651_1.png)

图 19.21 – 创建输出参数

- 10. 在 `CalculateNumberOfInstances` 宏中添加以下截图所示的节点。该宏获取样条长度，并将其除以 **实例间距** 以查找沿样条将创建的实例数量。**向下取整** 节点用于将除法结果向下取整：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_651_2.png)

图 19.22 – CalculateNumberOfInstances 宏的节点

### 注意

如果除法的分母为零，将生成除以零的运行时错误。蓝图中有一个名为 Safe Divide 的节点，如果分母为零，它将返回零，但不会生成错误。

- 11. 单击构建脚本选项卡。下一个截图显示了要添加到构建脚本的第一部分节点。我们获取存储在静态网格变量中的静态网格，并将其设置为实例化静态网格组件将使用的静态网格。For Loop 将根据实例数量重复。我们通过将 For Loop 的当前索引乘以实例间距来计算沿样条的距离：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_652_0.png)

图 19.23 – 构建脚本的第一部分

- 12. 以下截图显示了要添加到构建脚本的第二部分节点。Add Instance 节点将使用从样条接收到的位置和旋转在关卡中添加静态网格实例。请注意，坐标空间必须为本地，并且需要右键单击 Instance Transform 参数并选择 Split Struct Pin：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_653_0.png)

图 19.24 – 构建脚本的第二部分

- 13. 编译蓝图并关闭蓝图编辑器。从内容浏览器中拖动 **BP_SplinePlacement** 并将其放置在关卡中。
- 14. 样条在关卡编辑器中由白色点表示的样条点组成。您可以选择一个样条点对其进行平移和旋转。要添加样条点，请右键单击样条并选择 **在此处添加样条点**：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_653_1.png)

图 19.25 – 添加样条点

- 15. 您可以通过添加、平移和旋转样条点来创建路径。静态网格实例将沿样条添加：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_654_0.png)

图 19.26 – 静态网格实例沿样条排列

关卡设计师将能够灵活地围绕关卡内容定义样条路径。

样条还可用于变形静态网格，如下一节所示。

### 样条网格组件

还有一个名为 **样条网格** 的组件使用样条。此组件用于沿双点样条变形静态网格。我们可以在关卡编辑器中编辑样条点，或使用蓝图函数设置它们。

**设置起点和终点** 函数可用于构建脚本中以定义样条网格：


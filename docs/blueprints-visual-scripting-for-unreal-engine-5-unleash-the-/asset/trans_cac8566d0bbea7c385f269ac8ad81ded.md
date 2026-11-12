

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_82_0.png)

图 3.17 – 添加静态网格体组件

- 6. 在 **My Blueprint** 面板中，创建一个名为 **SM_Mesh** 的新变量。在 **Details** 面板中，单击 **Variable Type** 下拉菜单并搜索 `Static Mesh`。将鼠标悬停在 **Static Mesh** 上以显示子菜单，然后选择 **Object Reference**。选中 **Instance Editable** 属性，如下图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_83_0.png)

图 3.18 – SM_Mesh 变量的详细信息

### 重要说明

**Object Reference** 变量也可以引用在运行时创建的实例。

- 7. 按下工具栏上的 **Compile** 按钮。在 **Details** 面板底部为 **SM_Mesh** 变量定义一个初始静态网格体。单击 **Default** 值属性的下拉菜单，然后选择 **SM_TableRound** 静态网格体。

- 8. 单击 **Construction Script** 面板。将 `StaticMeshComp` 组件从 **Components** 面板拖动并放入 **Construction Script** 图表中以创建节点。

- 9. 单击 StaticMeshComp 节点的蓝色引脚，然后在图表中拖放以打开上下文菜单。搜索 set static mesh 并选择同名函数，如下图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_84_0.png)

图 3.19 – 在上下文菜单中选择函数

- 10. 将 SM_Mesh 变量从 My Blueprint 面板拖动，放入 Construction Script 图表中，并在出现的菜单中选择 Get SM_Mesh 选项。将 SM_Mesh 节点引脚连接到 Set Static Mesh 函数的 New Mesh 引脚。Construction Script 应类似于下图所示。当 Construction Script 执行 Set Static Mesh 函数时，它将从 SM_Mesh 变量获取静态网格体并将其设置到 Static Mesh Comp 组件上：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_84_1.png)

图 3.20 – Construction Script 操作

- 11. 编译蓝图。在关卡编辑器中，将 BPConstruction 从内容浏览器拖动并放入关卡以创建实例。再次拖放 BPConstruction 以创建另一个实例。在关卡中选择一个实例，并在关卡编辑器的 Details 面板中，检查 SM_Mesh 变量是否可见且可编辑，如下图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_85_0.png)

图 3.21 – BPConstruction 实例详细信息

- 12. 单击 **SM_Mesh** 变量的下拉菜单并选择另一个静态网格体，例如 **SM_Couch**。Construction Script 将立即执行并更改所选实例的静态网格体。下图显示了 `BPConstruction` 类的两个实例。截图左侧的实例使用默认静态网格体，但右侧实例的静态网格体已修改为 **SM_Couch**：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_86_0.png)

图 3.22 – BPConstruction 的两个实例

`Actor` 类是 Gameplay Framework 的主类，但还有其他用途的类我们需要了解。

## 探索其他 Gameplay Framework 类

创建新蓝图时的第一步之一是选择将用作模板的父类。下图显示了用于选择父类的面板。按钮上显示的类称为 **Common Classes**，是 Gameplay Framework 的一部分。要使用另一个类作为父类，请展开 **All Classes** 类别并搜索所需的类：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_87_0.png)

图 3.23 – 选择父类

下图显示了 **Common Classes** 的层次结构。在 Unreal Engine 中，有一个名为 **Object** 的父类。类继承其上方类（即其父类）的特征。基于 OOP 的继承概念，我们可以说 **Character** 类的实例是 **Character** 类型、**Pawn** 类型和 **Actor** 类型：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_88_0.png)

图 3.24 – Common Classes 层次结构

通过分析此层次结构，我们可以看出 **Actor Component** 和 **Scene Component** 类不是 Actor。这些类用于创建可以添加到 Actor 的组件。组件的两个示例是 `Static Mesh Component` 和 `Rotating Movement Component`，我们在之前的示例中使用过它们。我们将在 _第 18 章，创建蓝图库和组件_ 中介绍组件的创建。

让我们更仔细地查看一些 **Common Classes**。

### Pawn

**Pawn** 是 **Actor** 的子类。**Pawn** 是可以被游戏中 **Controller** 拥有的 **Actor**。 **Controller** 类表示玩家或 **人工智能** (AI)。概念上，**Pawn** 类的实例是游戏角色的物理身体，而拥有它的 Controller 类实例是一种大脑，允许角色在关卡中移动并执行其他操作。

基于 Pawn 类创建蓝图并单击 Class Defaults 按钮以在 Details 面板中显示它。从 Pawn 类继承的参数在下图中显示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_89_0.png)

图 3.25 – Pawn 类默认值

某些参数显示 Pawn 类可以使用拥有它的 Controller 类的旋转值。其他参数指示 Pawn 类必须如何被 Controller 类拥有。

**Pawn** 的两个主要子类是 **Character** 和 **WheeledVehicle**。

### Character

**Character** 类是 **Pawn** 类的子类；因此，**Character** 类的实例也可以被 **Controller** 类的实例拥有。此类旨在表示可以行走、奔跑、跳跃、游泳和飞行的角色。

基于 **Character** 类的蓝图将继承以下特定于角色的组件：

  - * **CapsuleComponent**：用于碰撞测试。
  - * **ArrowComponent**：指示角色的当前方向。
  - * **Mesh**：此组件是骨骼网格体，以可视方式表示角色。**Mesh** 组件的动画由动画蓝图控制。
  - * **CharacterMovement**：此组件用于定义各种类型的角色移动，例如行走、奔跑、跳跃、游泳和飞行。

这些组件在下图中显示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_91_0.png)

图 3.26 – Character 类组件

**CharacterMovement** 组件处理移动以及多人游戏中的复制和预测。它包含许多定义角色各种移动类型的参数：

| 设置 | 值 |
| :--- | :--- |
| Gravity Scale | 1.0 |
| Max Acceleration | 2048.0 |
| Braking Friction Factor | 2.0 |
| Braking Friction | 0.0 |
| Crouched Half Height | 40.0 |
| Use Separate Braking Friction | [ ] |
| Mass | 100.0 |
| Default Land Movement Mode | Walking |
| Default Water Movement Mode | Swimming |

图 3.27 – Character Movement 变量

### PlayerController

**Controller** 类有两个主要子类：**PlayerController** 和 **AIController**。 **PlayerController** 类由人类玩家使用，而 **AIController** 类使用 AI 来控制 **Pawn**。

**Pawn** 和 **Character** 类的实例只有在被 **PlayerController** 实例拥有时才能接收输入事件。输入事件可以放在 **PlayerController** 或 **Pawn** 的事件图表中。将输入事件放在 **PlayerController** 中的优势在于这些事件独立于 **Pawn**，使得更改由 **Controller** 类拥有的 **Pawn** 类更加容易。无论您选择哪种方式，请在项目中保持一致。

下图显示了如何在游戏中更改由 **PlayerController** 拥有的 **Pawn**，并展示了 **Possess** 函数的使用。在此示例中，关卡中有两个角色，玩家可以通过按 **1** 或 **2** 键来控制它们。只有当前被拥有的 **Character** 实例接收 **PlayerController** 命令：


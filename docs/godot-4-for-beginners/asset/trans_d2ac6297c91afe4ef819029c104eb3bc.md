
## LEGO – 理解 Godot 工作原理的有用隐喻

在 Godot 中构建游戏就像搭建 LEGO 模型。在这个类比中，节点就像单个 LEGO 积木。每块积木都有特定功能——有些是基础的，像简单的积木块，而其他则可能有专门用途，像轮子、窗户或齿轮。

Godot 系统真正的强大之处在于其灵活性。正如你可以拆卸并重新组合 LEGO 模块来构建全新事物一样，你可以在 Godot 中混搭场景，以快速组装游戏的不同部分。这种模块化让你能够以更易管理、高效且富有创意的方式创建复杂游戏。

在 Godot 中，你使用节点来表示游戏的不同元素，例如角色、环境和行为。就像你可以以各种方式连接 LEGO 积木来创建不同结构一样，你可以在 Godot 中组合节点来构建复杂的游戏对象和系统。

现在，把场景想象成由那些 LEGO 积木制成的预建部分或模块。一个场景可以是像汽车或树这样的小部件，也可以是像整栋建筑这样更大的东西。在 Godot 中，场景是一组按特定方式排列的节点，用来创建游戏中有意义的部分。你可以重用场景，就像你可以把预建的 LEGO 模块附加到更大的结构上一样。

![](img/d34207e07c133a9e84d9caf1d479006c_58_0.png)

但拼图还有最后一块：资源。如果节点是 LEGO 积木，场景是 LEGO 模块，那么资源就像随 LEGO 套装附带的贴纸、蓝图和说明书。贴纸代表视觉元素，如纹理和材质。蓝图或手册描述某物如何表现或动画——就像 Godot 中的动画资源、脚本和音频流。

正如你可以把同一张贴纸应用到多个积木上，或用同一本手册进行多次搭建一样，Godot 中的资源是可重用、可自定义的，并且可在整个游戏中共享。

它们本身并不构建结构，但为你已组装好的部件添加样式、数据和行为。

这种分层方法——Nodes（积木）、Scenes（模块）和 Resources（自定义细节和说明）——使 Godot 成为构建游戏的强大且直观的工具，就像 LEGO 让构建富有想象力的创作变得有趣且易于上手一样。

## 创建额外场景

假设我们想断开其中一块 LEGO 积木（参见前文中的 *LEGO – 理解 Godot 工作原理的有用隐喻*）。让我们从我们的 **Main** 场景中移除我们的 **Label** 节点，并将其放入自己的场景中：

- 1. 我们可以通过右键单击 **GreetingsLabel** 节点并选择 **Save Branch as Scene...** 来实现。这在 *图 2.6* 中显示：

![](img/d34207e07c133a9e84d9caf1d479006c_59_0.png)

Figure 2.6 – Save Branch as Scene...

- 2. 将场景保存为 greetings_label.tscn 到我们的 Scenes 文件夹中：

![](img/d34207e07c133a9e84d9caf1d479006c_60_0.png)

Figure 2.7 – Saving the greetings_label as its own Scene

你会注意到 GreetingsLabel 并没有从场景中消失。然而，现在我们有了标签的独立场景副本，我们可以将其从 Main 场景中删除，并轻松将其添加回来或向场景中添加多个标签。

- 3. 如果你在 FileSystem 区域中双击 greetings_label.tscn，它将在新的场景窗口中打开：

![](img/d34207e07c133a9e84d9caf1d479006c_60_1.png)

Figure 2.8 – Double-click greetings_label.tscn to open it in its own scene tab

- 4. 现在，从 **Main** 场景中删除 **GreetingsLabel**。
- 5. 然后通过从 **FileSystem** 区域将 greetings_label.tscn 拖放到 Viewport 中将其添加回来：

![](img/d34207e07c133a9e84d9caf1d479006c_61_0.png)

Figure 2.9 – Drag and drop the greetings_label.tscn back into the Viewport

现在我们可以看到标签返回到场景后的样子：

![](img/d34207e07c133a9e84d9caf1d479006c_62_0.png)

Figure 2.10 – Main Scene with the GreetingsLabel node returned

我们现在可以将多个 greetings_label 场景的实例拖放到 Main 场景中，并可以在 Inspector 的 Text 属性中更改每个实例的文本属性。然而，当我们运行 Main 场景时，每个标签都会恢复为 Go Go Godot!。这是因为每个实例都附加了相同的脚本，而我们之前将文本属性更改为读取 Go Go Godot!。

![](img/d34207e07c133a9e84d9caf1d479006c_62_1.png)

Figure 2.11 – Multiple instances of greetings_label in the Main scene

## 实例是什么？

实例化是从蓝图或主要设计中生成游戏对象的过程。实例化与场景配合良好。它使你能够将游戏划分为可重用组件，提供结构化复杂系统的工具，并提供一种以自然方式思考游戏结构的语言。

我们可以通过让游戏对用户输入做出反应来演示实例化。

## 对玩家输入做出反应

现在，我们可以做一个实验，根据用户收到的输入显示不同的标签。在这种情况下，我们将添加一个精灵并将其标签放在其头部上方。**sprite** 是游戏中用于表示角色、对象或其他视觉元素的 2D 图像或动画。简单来说，它就是你在游戏中看到并会移动或与环境交互的图片或图形。

如果玩家左键单击精灵，我们将显示 **Hello Folks!** 如果他们对精灵 *右键单击*，我们将显示 **Godot is Great!**，如果他们按下键盘上的 *Enter / Return*，我们将显示 **Hello World!**

首先，我们需要一个精灵。获取免费游戏资产的好地方是 **Kenney**。你可以在这里找到网站：https://kenney.nl/。我在此实验中使用的资源可以在这里找到：https://kenney.nl/assets/toon-characters-1。

我选择的精灵在你解压文件夹后可以在这里找到：`\kenney_toon-characters-1\Robot\PNG\Poses HD \character_robot_interact`。

这就是我们的精灵的样子：

![](img/d34207e07c133a9e84d9caf1d479006c_63_0.png)

*Figure 2.12 – The robot sprite character for our experiment*

然而，你可以使用任何你喜欢的角色进行此实验。我们现在将精灵添加到场景中：

- 1. 在 FileSystem 中创建一个新文件夹并命名为 Sprites：

![](img/d34207e07c133a9e84d9caf1d479006c_64_0.png)

Figure 2.13 – Create the Sprites folder

- 2. 现在将你选择的角色拖入此 Sprites 文件夹：

![](img/d34207e07c133a9e84d9caf1d479006c_64_1.png)

Figure 2.14 – Your chosen image within the Sprites folder in FileSystem

- 3. 现在我们将为角色创建一个新场景。点击 Scene 菜单并选择 New Scene（按 Ctrl + N），或点击场景标签附近的 + 符号：

![](img/d34207e07c133a9e84d9caf1d479006c_65_0.png)

Figure 2.15 – Multiple ways of creating a new scene

- 4. 现在通过按 Ctrl + A、点击场景视图中的 + 图标，或点击 Other Node 按钮来添加场景的父节点或主节点：

![](img/d34207e07c133a9e84d9caf1d479006c_66_0.png)

Figure 2.16 – Multiple ways to add a node to a scene

- 5. 搜索 Sprite2D 节点并添加它：

![](img/d34207e07c133a9e84d9caf1d479006c_66_1.png)

Figure 2.17 – Add a Sprite2D node to the scene

- 6. 将节点重命名为 Player：

![](img/d34207e07c133a9e84d9caf1d479006c_67_0.png)

Figure 2.18 – Sprite2D renamed as Player

- 7. 现在按 Ctrl + S 将场景保存为 player.tscn 到我们的 Scenes 文件夹中：

![](img/d34207e07c133a9e84d9caf1d479006c_67_1.png)

Figure 2.19 - The saved player.tscn scene

- 8. 确保你处于 2D 视图。如果需要，通过点击屏幕顶部的 2D 切换到它。

- 9. 现在将你选择的角色图像拖放到 Player 节点的 <Empty> Texture 属性上：

![](img/d34207e07c133a9e84d9caf1d479006c_68_0.png)

Figure 2.20 - Place your character image in the Texture property of the Player node

- 10. 现在你的角色已在 Player 场景中：

![](img/d34207e07c133a9e84d9caf1d479006c_68_1.png)

Figure 2.21 – The Player sprite in the Viewport

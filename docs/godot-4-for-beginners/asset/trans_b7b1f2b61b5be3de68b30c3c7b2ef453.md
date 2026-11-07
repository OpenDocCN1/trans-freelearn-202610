
11. 现在我们将为 Player 附加一个脚本。右键单击 Player 节点并选择“附加脚本...”或单击“滚动+”图标：

![](img/d34207e07c133a9e84d9caf1d479006c_69_0.png)

图 2.22 – 将新脚本附加到 Player 节点

12. 按 *Ctrl + S* 保存脚本并将其保存在 Scripts 文件夹中。现在你在 Scripts 文件夹中将有一个新的 player.gd 脚本：

![](img/d34207e07c133a9e84d9caf1d479006c_70_0.png)

图 2.23 – Scripts 文件夹中的 player.gd 脚本

在我们编写一些代码以允许玩家对不同的用户输入做出反应之前，请记住我们希望对 *左键点击*、*右键点击* 和 *Enter/回车* 被按下时显示不同的问候语。

如果你还记得我们的 LEGO 类比，我们需要将 **问候标签** 砖块（节点）附加到我们的 **玩家** 砖块（节点）上。我们通过将 greetings_label.tscn 从 **文件系统** 拖到 **玩家** 场景中的 **玩家** 节点来实现这一点：

![](img/d34207e07c133a9e84d9caf1d479006c_71_0.png)

图 2.24 – 将 greetings_label.tscn 拖到 Player 节点上使其成为子节点

在 Godot 中，原点或起始位置是从屏幕左上角或位置 0, 0 开始测量的。几乎总是情况下，你会希望保持原点不变，但仅为本示例，我们将移动它。

使用选择工具（看起来像光标）或通过在键盘上按 Q 来在视口中定位 Player 和 GreetingsLabel：

![](img/d34207e07c133a9e84d9caf1d479006c_72_0.png)

图 2.25 – 选择工具

你的 Player 场景应该看起来像这样：

![](img/d34207e07c133a9e84d9caf1d479006c_72_1.png)

图 2.26 – Player 场景

现在我们的精灵已经位于场景中心附近，我们需要编写一个脚本使其对用户输入做出响应。

## 对输入进行玩家反应脚本编写

我们编写本节代码的目标是允许用户根据按下的鼠标按钮从精灵处获得不同的响应。我们将处理三种类型的交互：

- *左键点击：* 显示问候语 **嗨，大家好！**
- *右键点击：* 显示 **Godot 很棒！**
- *按下 Enter：* 显示 **你好，世界！**

让我们首先确保问候标签初始时是隐藏的。

### 在用户提供输入之前隐藏标签

我们不希望在场景首次运行时问候语可见。我们希望通过调用其 hide() 函数来隐藏标签。

由于 _ready() 函数是在节点及其所有子节点加载到场景树后首先运行的函数，因此我们可以使用 Godot 快捷操作符 $ 访问 GreetingsLabel 节点。

一旦我们获得了对该节点的访问权限，我们只需调用 hide() 函数即可：

```gdscript
# Called when the node enters the scene tree for the first time.
func _ready() -> void:
    $GreetingsLabel.hide()
```

![](img/d34207e07c133a9e84d9caf1d479006c_73_0.png)

重要提示

请注意 GDScript 是一种缩进语言。这意味着函数后的第一行（如前述代码中所示）会缩进一次。游戏引擎要求这样做才能使代码正常工作。我建议所有使用本书数字版的人手动重新输入代码或从本书的 GitHub 仓库复制代码文件，以避免因复制和粘贴代码而导致的错误。

有了这段代码，运行游戏时 GreetingsLabel 将不再可见。你可以通过按 F6 或单击图 2.27 所示的“运行当前场景”按钮来测试此场景：

![](img/d34207e07c133a9e84d9caf1d479006c_73_1.png)

图 2.27 – 运行当前场景按钮

你会注意到你再也看不到问候语了。

### 输入处理

现在，为了让我们的实验保持简单，我们将使用内置方法来处理输入。**内置方法** 只是指已经创建并作为 Godot Engine 组成部分的方法。你可以通过方法名以下划线开头来判断它是内置的。你到目前为止已经见过两个：`_ready()` 和 `_process()`。

对于输入，我们将使用 `_unhandled_input()`。现在不必太担心它，只要知道如果没有其他函数/方法来捕获用户输入并处理它，那么此方法将被运行。

我们声明 `_unhandled_input` 函数头，然后在函数内部处理不同类型的输入。在我们的实验中，我们正在监听两种类型的输入：*鼠标* 和 *键盘*。对于键盘，我们只监听一个键 – *Enter*。对于鼠标，我们监听 *左键点击* 和 *右键点击*。让我们更详细地看一下：

- 当我们检测到来自鼠标的输入时，我们需要再次显示标签，因此我们调用标签的 show() 函数。
  - 如果听到左键点击，我们将 **问候标签** 的文本更改为 **嗨，大家好！**
  - 如果是右键点击，我们将其设置为 **Godot 很棒！**
- 当我们检测到来自键盘的输入时，我们需要再次显示标签，因此我们调用标签的 show() 函数。
  - 如果听到按下 Enter，我们将文本设置为 **你好，世界！**

现在，让我们来看代码实现：

```gdscript
extends Sprite2D
# This script is attached to a Sprite2D node.
# It listens for mouse and keyboard input and displays
# different messages on a Label depending on what was pressed.

# Called when the node enters the scene tree for the first time (i.e.,
# when the scene starts).
func _ready():
    # Hide the Label at the beginning so it's not visible until an input
    # event happens.
    $GreetingsLabel.hide()

# Called every frame. 'delta' is the time in seconds since the last frame.
func _process(delta):
    # We're not doing anything every frame in this example, but the
    # function is here in case we want to later.
    pass

# Called when the player performs an input action that hasn't already been
handled.
func _unhandled_input(event):
    # Check if the event was a mouse button press.
    if event is InputEventMouseButton:
        # Show the label whenever a mouse button is pressed.
        $GreetingsLabel.show()
        # Check which mouse button was pressed.
        if event.button_index == MOUSE_BUTTON_LEFT:
            # If the left mouse button was clicked, set the label's text
            # to "Hi Folks!"
            $GreetingsLabel.text = "Hi Folks!"

        elif event.button_index == MOUSE_BUTTON_RIGHT:
            # If the right mouse button was clicked, set the label's text
            # to "Godot is Great!"
            $GreetingsLabel.text = "Godot is Great!"

    # Check if the event was a key press.
    elif event is InputEventKey:
        # Only show the label and set the text if the Enter key was
        # pressed.
        if event.keycode == KEY_ENTER:
            $GreetingsLabel.show()
            $GreetingsLabel.text = "Hello World!"
```

现在我们已经测试了 **玩家** 场景，并且知道它按预期工作。我们可以返回到 **Main** 场景并移除 **问候标签** 节点，然后将 player.tscn 拖到 **Main** 节点上。然后再次运行游戏并检查一切是否正常工作：

![](img/d34207e07c133a9e84d9caf1d479006c_76_0.png)

图 2.28 – 我们新的 Main 场景

这是一个重要的章节，向你介绍了 Godot 的基本构建块。我建议你经常重温它，甚至可以返回章节末尾的总结，以提醒自己场景、节点和脚本的要点和目的。

## 总结

在本章中，我们探索了 Godot 4 的基础功能，从编辑现有项目和创建额外场景开始。你学习了如何导航界面、有效保存场景，并掌握了 Godot 核心的节点和场景设计理念。我们还介绍了如何通过对玩家输入做出反应使你的项目具有交互性。通过掌握这些核心概念和工具，你已迈出了自信使用 Godot 开发游戏的第一步。

在下一章中，我们将对 3D 进行介绍，并了解如何使用材质和光照。

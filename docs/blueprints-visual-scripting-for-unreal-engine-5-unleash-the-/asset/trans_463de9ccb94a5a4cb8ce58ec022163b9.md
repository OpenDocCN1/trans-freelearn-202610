

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_413_0.png)

Figure 11.23 – PauseMenu 层级结构

- 6. 将 **Button** 元素从 **Palette** 面板拖放到 **Hierarchy** 面板中 **Paused** 消息下方的 **CanvasPanel** 对象上。在 **Details** 面板中，将名称更改为 `Btn resume`，单击 **Anchors** 下拉菜单，并选择屏幕中心带有锚点的选项。

- 7. 将 Position X 设置为 `-180.0`，Position Y 设置为 `-250.0`，Size X 设置为 `360.0`，Size Y 设置为 `100.0`。

- 8. 将 **Text** 对象从 **Palette** 面板拖放到 **Hierarchy** 面板中的 **Btn resume** 元素上。在 **Details** 面板中，将名称设置为 `Txt resume`，将 **Content** 类别下的 **Text** 字段更改为 `Resume`，并将字体 **Size** 设置为 `48`。

- 9. 现在，让我们将 **Restart** 按钮转换为 **Reset All** 按钮。将 **Btn restart** 按钮名称更改为 `Btn reset`，将 `Txt restart` 更改为 `Txt reset`，然后将 **Text** 字段更改为 `Reset All`。

- 10. 编译并保存。

我们已经添加了 **PauseMenu** 的视觉元素；现在让我们来处理功能部分。

### 恢复游戏

要恢复游戏，需要将 **PauseMenu** 从视口中移除，隐藏鼠标光标，并取消暂停游戏。

添加功能的步骤如下：

- 1. 单击 **Btn resume** 元素，向下滚动到 **Details** 面板底部，单击 **On Clicked** 事件旁边的 + 按钮。

- 2. 在 **Graph** 视图中，将这些节点添加到 **On Clicked (Btn resume)** 事件：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_414_0.png)

Figure 11.24 – 恢复按钮操作

- 3. 从 **On Clicked (Btn resume)** 节点拖出一条线，添加 **Remove from Parent** 节点。

- 4. 在 Event Graph 上右键单击，添加 **Get Player Controller** 节点。

- 5. 从 **Get Player Controller** 的 Return Value 拖出一条线，添加 **SET Show Mouse Cursor** 节点。将 **Remove from Parent** 的白色输出引脚连接到 **SET Show Mouse Cursor** 的白色输入引脚。保持 **Show Mouse Cursor** 参数未勾选。

- 6. 从 **SET Show Mouse Cursor** 的白色输出引脚拖出一条线，添加 **Set Game Paused** 节点。保持 **Paused** 参数未勾选。

这些是 **Resume** 按钮的操作。现在，让我们添加 **Reset All** 按钮的操作。

### 重置存档文件

**Reset All** 按钮会在存档存在时将其删除，然后重新加载游戏关卡。我们将创建一个包含删除存档操作的宏。

按照以下步骤创建宏：

- 1. 在 **My Blueprint** 面板中，单击 **Macros** 类别中的 + 按钮创建宏。将宏的名称更改为 `DeleteFile`。在宏的 **Details** 面板中，创建一个名为 `In` 的 **Exec** 类型输入参数和一个名为 `Out` 的 **Exec** 类型输出参数。

- 2. 在为 **DeleteFile** 宏创建的选项卡上，添加以下截图中显示的节点。我们获取 **FirstPersonCharacter** 实例的引用以检索 **Save Slot Name** 变量的值。

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_415_0.png)

Figure 11.25 – DeleteFile 宏操作

- 3. 添加 **Get Player Character** 节点，使用 **Cast To FirstPersonCharacter** 节点进行转换，然后从 **As First Person Character** 引脚拖出线添加 **Get Save Slot Name** 节点。

- 4. 从 **Cast To FirstPersonCharacter** 的白色输出引脚拖出线，添加 **Does Save Game Exist** 节点。将 **Slot Name** 引脚连接到 **Get Save Slot Name** 节点。

- 5. 从 **Does Save Game Exist** 拖出线，添加 **Branch** 节点。将 **Return Value** 引脚连接到 **Condition** 引脚。

- 6. 从 **Branch** 节点的 **True** 输出拖出线，添加 **Delete Game in Slot** 节点。将 **Slot Name** 引脚连接到 **Get Save Slot Name** 节点。将白色输出引脚连接到 **Outputs** 节点的 **Out** 引脚。

- 7. 将 **Cast To FirstPersonCharacter** 的 **Cast Failed** 引脚和 **Branch** 节点的 **False** 引脚连接到 **Outputs** 节点的 **Out** 引脚。

- 8. 在 **Event Graph** 中，在 **On Clicked (Btn reset)** 事件后添加 **Delete File** 宏节点：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_416_0.png)

Figure 11.26 – 修改 On Clicked (Btn reset) 事件

- 9. 编译并保存 **PauseMenu**。

我们创建了 **PauseMenu** 屏幕。现在需要一种方法让玩家调出 **PauseMenu**。

## 触发暂停菜单

我们将使用 **Enter** 键暂停游戏并打开 **PauseMenu**。我们需要在 **Project Settings...** 中添加一个名为 **Pause** 的动作映射，并为 **Pause** 动作映射创建的 **InputAction** 事件添加操作。

按照以下步骤触发 **PauseMenu**：

- 1. 要更改游戏的输入设置，请单击工具栏最右侧的 **Settings** 按钮，并选择 **Project Settings...** 选项：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_417_0.png)

Figure 11.27 – 访问 Project Settings

- 2. 在出现的窗口左侧，查找 **Engine** 类别并选择 **Input** 选项。单击 **Action Mappings** 旁边的 + 号。将新操作命名为 **Pause**，并从下拉菜单中选择 **Enter** 键，将该键映射到 **Pause** 事件：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_418_0.png)

Figure 11.28 – 创建动作映射

- 3. 在内容浏览器中，访问 Content > FirstPersonBP > Blueprints 文件夹并双击 FirstPersonCharacter 蓝图。

- 4. 在 Event Graph 上右键单击，搜索 input_action_pause，添加 Pause Event 节点。复制 LostGame 自定义事件中的所有节点，并将它们粘贴到 InputAction Pause 节点附近：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_418_1.png)

Figure 11.29 – InputAction Pause 事件操作

- 5. 将 **InputAction Pause** 节点的 **Pressed** 引脚连接到 **Set Game Paused** 节点的白色输入引脚。

- 6. 将 **Create Widget** 节点的 **Class** 参数更改为 **Pause Menu**。

- 7. 编译、保存，然后单击 **Play** 进行测试。

现在，在游戏过程中，你应该能够按下设置的 *Enter* 键来调出暂停菜单。单击 **Resume** 按钮应该关闭暂停菜单并返回游戏。如果你在游戏中推进几轮，然后从暂停菜单中按下 **Reset All** 按钮，则应该自动重新加载关卡，进度重置为游戏的第一轮。如果你看到的情况是这样，那么恭喜！你已经在创建一个能够在多轮游戏中存储、加载和重置进度的存档系统方面取得了重大成就。

## 摘要

在本章中，我们在将游戏打造为一种他人可以游玩和享受的完整体验方面取得了显著进展。你学习了如何根据玩家是否获胜或失败来显示不同屏幕。你还学习了如何实现存档系统，允许玩家在保留进度的情况下返回之前的游戏会话。然后，我们实现了一个回合系统，每当玩家推进到新回合时，该系统都会修改游戏目标。最后，我们实现了额外的菜单系统，这些系统会向玩家提供他们所处的回合信息，并让他们有机会暂停游戏甚至重置自己的存档文件。

在下一章中，我们将探讨如何为我们创建的游戏制作构建版本，以便与他人分享体验。

## 测验

- 1. **Create Widget** 节点创建参数中所选小部件类的实例。
  - a. 正确
  - b. 错误

- 2. 用于存储将要保存的数据的类名是什么？
  - a. `SaveData`
  - b. `SaveInfo`
  - c. `SaveGame`

- 3. **Save Game to Slot** 函数接收一个结构作为输入参数，其中包含将要保存的数据。
  - a. 正确
  - b. 错误

- 4. **Load Game from Slot** 函数返回 `SaveGame` 类的一个实例。我们需要使用 **Cast To** 节点来访问已保存的变量。
  - a. 正确
  - b. 错误

- 5. 名为 **Shoot** 的 **Action Mapping** 会触发名为 **InputAction Shoot** 的事件。
  - a. 正确
  - b. 错误


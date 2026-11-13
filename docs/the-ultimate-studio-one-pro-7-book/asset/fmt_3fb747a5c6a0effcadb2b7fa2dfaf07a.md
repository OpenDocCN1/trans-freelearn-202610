
## 3. 导入伴奏轨道

如果您已准备好想要演奏的歌曲的伴奏轨道，请从资源管理器（Windows）或访达（macOS）中将它们拖拽至音频放置区（标题为 `Drop files here`）。如果伴奏轨道尚未准备好，也不必担心；您稍后随时可以添加。

## 4. 确认创建

点击 `OK`。

这将在 Studio One 中创建您的第一个 `Show` 并带您进入 `Show` 页面，这正是我们接下来要探索的内容。

# 探索 Show 页面

`Show` 页面外观类似于 `Song` 页面的简化版，因此不会太陌生。

![Figure 16.2: The Show page](image/B31512_16_02.jpg)

Figure 16.2: The Show page

`Show` 页面有三个独有的区域。让我们逐一了解：

*   **Setlist**（图 16.2 中的 **a**）：该区域包含您在现场 `Show` 中将要演奏的所有歌曲列表。如果您使用图 16.1 中的音频放置区导入了伴奏轨道，它们将显示在此窗口中。现场演出时，歌曲将按此处显示的顺序依次播放。您可以通过在列表中上下拖拽歌曲来轻松重新排序。

*   **Players** 栏（图 16.2 中的 **b**）：该栏包含您的 `Show` 中将处于激活状态的所有 `Player` 列表。我们将在 *使用 Player* 部分稍作介绍。

*   **Overview** 窗口（图 16.2 中的 **c**）：`Setlist` 中的歌曲在 `Overview` 窗口中以事件的形式水平排列。我们将使用此窗口来设置和组织您的 `Show`。

现在，我们准备深入 `Show` 页面，学习如何将其设置为运行您的下一场现场演出。

# 使用 Player

一场音乐会中演奏的歌曲可能包含多个音乐元素：

*   歌手
*   演奏实体乐器（如吉他和鼓）的乐手
*   演奏虚拟乐器（如 Studio One 的 `Presence`）的乐手
*   补充表演的伴奏轨道

在 `Show` 页面中，每个元素由一个称为 `Player` 的相应设备表示。正如我们在 `Song` 页面上为每个乐器设置不同的轨道一样，在 `Show` 页面上我们为每个乐器设置不同的 `Player`。共有三种类型的 `Player`：

*   `Backing Track Player`（伴奏轨道播放器）
*   `Real Instrument Player`（实体乐器播放器）
*   `Virtual Instrument Player`（虚拟乐器播放器）

现在，让我们探索每种播放器类型，并了解如何添加播放器以丰富您的 `Show`。

## 添加伴奏轨道播放器

`Backing Track Player` 仅播放音频文件，就像 `Song` 页面上的音频轨道一样。现场演出时，`Backing Track Player` 将播放您想用于伴奏的任何预录音频文件。

在图 16.1 创建新 `Show` 时，我将四个音频文件拖放到了音频放置区。Studio One 创建了一个 `Backing Track Player` 并将这些文件首尾相连地放置在该播放器上，如下所示：

![Figure 16.3: Backing Track Player](image/B31512_16_03.jpg)

Figure 16.3: Backing Track Player

如果我想在音乐会上演奏更多歌曲，需要执行以下操作：

1.  在资源管理器（Windows）或访达（macOS）中找到新歌曲的伴奏轨道。
2.  将新的伴奏轨道拖拽并放置在 `Backing Track Player` 右侧的空白处：

![Figure 16.4: Adding new files to a Backing Track Player](image/B31512_16_04.jpg)

Figure 16.4: Adding new files to a Backing Track Player

3.  添加新轨道后，我可以通过在 `Overview` 窗口或 `Setlist` 中拖拽它们来轻松重新排序。

您可能还希望在演出中的某首歌曲里让多个音频文件同时播放。例如，您可能想以分轨形式导入歌曲的各个元素——一轨鼓、一轨吉他等——并在现场演出时分别控制这些分轨。为此，只需将音频文件从资源管理器（Windows）或访达（macOS）拖拽并放置在 `Backing Track Player` 下方的空白处。Studio One 将为每个音频文件创建一个新播放器，并将它们置于同一歌曲下方：

![Figure 16.5: Adding new files to a song](image/B31512_16_05.jpg)

Figure 16.5: Adding new files to a song

这种灵活的设置允许您在更精细的层面上控制伴奏轨道，例如为歌曲的不同元素应用不同的效果插件。

## 添加实体乐器播放器

如果您或乐队成员将在音乐会上演奏实体乐器或进行人声表演，您可以使用 `Real Instrument Player` 将它们添加到您的 `Show` 中。这允许您使用 Studio One 的效果插件实时处理这些乐器或人声的声音。

要添加 `Real Instrument Player`，请按以下步骤操作：

1.  点击 `Players` 栏中的 `Add Player` 按钮。这将打开 `Add Player` 窗口：

![Figure 16.6: The Add Player window](image/B31512_16_06.jpg)

Figure 16.6: The Add Player window

2.  点击 `AUX` 选项卡。
3.  为即将创建的播放器起一个简短、描述性的名称，以便在 `Players` 栏中轻松识别。
4.  在 `Input` 字段中，选择该播放器将连接到的音频设备上的物理输入。例如，如果您为电吉他创建 `Real Instrument Player`，且该吉他连接到音频设备的输入 2，请在此处选择 `Input 2`。
5.  点击 `OK`。

您需要为乐队中的每位表演者创建一个 `Real Instrument Player`，以便通过为每个播放器应用不同的效果来分别处理它们。

## 添加虚拟乐器播放器

`Virtual Instrument Player` 允许您将虚拟乐器（如 `Presence`）添加到 `Show` 中并在现场演出时演奏。如果您希望虚拟乐器的声音在歌曲之间发生变化，您可以预先编排这些变化，让 Studio One 在音乐会期间自动实现切换，而您只需专注于演奏。

要向 `Show` 添加 `Virtual Instrument Player`，请执行以下操作：

1.  打开 `Browse` 窗口并选择 `Instruments` 选项卡。
2.  找到您想添加到 `Show` 的虚拟乐器。
3.  点击虚拟乐器名称左侧的小三角形。这将打开该乐器可用预设的列表：

![Figure 16.7: List of presets for Presence](image/B31512_16_07.jpg)

Figure 16.7: List of presets for Presence

4.  将您想使用的预设拖拽并放置到 `Overview` 窗口中。这将创建一个新的 `Virtual Instrument Player` 并加载您选择的预设：

![Figure 16.8: A Virtual Instrument Player](image/B31512_16_08.jpg)

Figure 16.8: A Virtual Instrument Player

5.  您现在可以使用 MIDI 键盘演奏该虚拟乐器。

图 16.8 中的 `Show` 有五首歌曲，如 `Overview` 窗口顶部所示。目前，我们创建的 `Virtual Instrument Player` 将在所有五首歌曲中播放相同的预设。但大多数情况下，您会希望每首歌曲使用不同的声音——第一首用原声钢琴，第二首用电钢琴，第三首用管风琴，以此类推。为此，只需将您想为每首歌曲使用的预设拖拽并放置到 `Virtual Instrument Player` 上对应的槽位中。在图 16.9 中，我为每首歌曲加载了不同的预设。您可以看到它们的名称显示在各自的槽位上：

![Figure 16.9: Adding a different preset for each song](image/B31512_16_09.jpg)

Figure 16.9: Adding a different preset for each song

现在，在现场演出时，当您从一首歌切换到下一首时，Studio One 将自动从一种声音切换到另一种。登台演出时，您少了一件烦心事！

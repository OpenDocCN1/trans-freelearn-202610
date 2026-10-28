

```cpp
instanceToInstanceCollision,
instanceToEdgeCollision,
NUM
};
```

首先，我们定义了两个值 `instanceToInstanceCollision` 和 `instanceToEdgeCollision`，用于通知一个实例发生了与其他实例的碰撞或与世界边界的碰撞。第一个值 `none` 用于在事件节点中忽略事件，而 `NUM` 值则需要在 `for` 循环中遍历所有值时使用。

在 `ModelInstanceCamData` 结构体中，添加了一个名为 `micNodeUpdateMap` 的映射，用于将 `nodeEvent` 值翻译为事件的可读字符串：

```cpp
std::unordered_map<nodeEvent, std::string>
micNodeUpdateMap{};
```

渲染器不仅在 `init()` 方法期间添加字符串，而且所有触发事件的调用都从渲染器发送到节点树：

```cpp
mModelInstCamData::mNodeEventCallbackFunction(
instSettings.isInstanceIndexPosition,
nodeEvent::instanceToEdgeCollision);
```

这个新的 `micNodeEventCallbackFunction` 回调被绑定到渲染器的 `addBehaviorEvent()` 方法，并在将请求传递给现有对象之后，调用最终结束于 `SingleInstanceBehavior` 类。在那里，所有请求的事件都被添加到一个向量中：

```cpp
std::vector<nodeEvent> mPendingNodeEvents;
```

事件在 `SingleInstanceBehavior` 类的 `update()` 调用中处理，调用节点的 `handleEvent()` 方法。`handleEvent()` 方法在“探索图节点的基类”部分中引入。它只是激活图中正在监听此事件类型的指定实例的所有事件节点。

包含事件的示例节点树如下所示：

![](img/3cfb9832c943f5f7366fd104b5255971_318_0.png)

图 9.9：女性模型的节点树概览

为了更好地理解，我们将此节点树的图像分为两部分，分别在图 9.10 和图 9.11 中。

图 9.10 显示了节点树中的默认路径，从根节点开始。

图 9.10：女性模型节点树中的默认路径

图 9.11 显示了事件节点以及对事件的反应。

图 9.11：女性模型节点树中的事件

在图 9.12 中，图 9.9 中女性模型的树以及男性模型的类似树的 resulting 行为如下所示：

![](img/3cfb9832c943f5f7366fd104b5255971_320_0.png)

> 图 9.12：实例在虚拟生活中

默认情况下，模型在四处行走，通过稍微旋转来对与其他实例的碰撞做出反应。碰撞世界边界后，实例会转身。经过随机时间后，实例停止移动，并播放随机动画片段。此时，正常执行结束，根节点再次被触发。

尽管节点编辑器看起来非常酷，我们可以添加不同类型的节点、创建链接并保存和加载状态，但仍应注意当前实现的某些限制。

我们基本实现缺少其他行为树的某些功能。

首先，状态直接存储在节点中。对于我们正在创建的应用程序用例而言，将状态数据存储在节点中是足够的。但对于更大的树，单独的状态存储会很方便，主要是因为我们不再需要将节点数据复制到实例中。

此外，编辑器会话中对节点树的更改不会自动复制到实例中。每次更改后，您需要选择一个实例或模型并应用当前状态。

由于只将编辑器会话的原始节点树数据保存到 YAML 文件中，而不保存各个实例的树数据，因此重新启动应用程序或重新加载配置会将编辑器状态应用于使用特定节点树的所有实例。

最后，节点无法访问其实例的数据或虚拟世界的其他数据。无法对诸如位置或旋转之类的属性做出反应，或查看您是否是碰撞的主要或次要候选者以调整旋转角度。

通过一些努力和额外的编码，可以消除其中许多限制。请参阅“实践环节”部分获取想法。

在本章的最后一节中，我们将使用到目前为止添加的代码来创建一个新功能：实例之间的交互。

## 添加实例之间的交互

我们已经有一个名为 `Interaction` 的动作，启动男性模型的交互动画片段（对于女性模型则什么也看不到，因为该模型没有动画片段来显示某种交互）。

此交互动作将被扩展以向附近的实例发送事件，说明我们想要与该实例交互。对交互请求的可能反应可以是为附近实例的男性模型重放挥手动画片段，从视觉上确认事件已被处理。

## 创建交互控制属性

渲染器需要一些变量和一点代码来支持交互。我们在 `opengl` 文件夹中的 `OGLRenderData.h` 中的 `OGLRenderData` 结构体中添加这些新变量：

```
bool rdInteraction = false;
float rdInteractionMaxRange = 10.0f;
float rdInteractionMinRange = 1.5f;
float rdInteractionFOV = 45.0f;
std::set<int> rdInteractionCandidates{};
int rdInteractWithInstanceId = 0;
```

与往常一样，对于 Vulkan，变量必须添加到 `VkRenderData.h` 文件中的 `VkRenderData` 结构体中。

有了这些新变量，我们可以打开和关闭交互，以及设置其他实例的最小和最大扫描范围，以及同伴实例需要处于的视野角度。视野允许我们仅选择我们正在看的实例，以及正在朝我们看的实例。

在 `std::set` 的 `int` 值中，我们存储配置范围内且具有正确视角的所有实例。最后，我们存储选择进行交互的实例。

交互候选选择在渲染器的 `findInteractionInstances()` 中处理。与碰撞检测类似，我们从所有实例开始，通过缩小选择属性来细化到单个实例。

用于查找适合交互的实例的简单算法如下：
- 1. 使用最大尺寸的 `BoundingBox2D` 对象，并调用四叉树的 `query()` 方法。现在我们有了该区域内的所有实例。
- 2. 筛除最小范围内的所有实例，包括我们自己。
- 3. 通过使用我们自己的旋转角度与我们和步骤 2 中剩余实例之间的距离向量之间的点积，移除所有朝向不正确 的实例。
- 4. 对步骤 3 中剩余的所有实例与我们之间的距离按升序排序，并将最近的实例设为交互实例。

如果任何步骤没有返回有效候选，则交互请求将被忽略。

## 扩展处理代码

多亏了我们的准备，添加新事件现在变得非常简单。首先，我们通过新的交互值扩展 `nodeEvent` 枚举：

```
enum class nodeEvent : uint8_t {
  ...
  interaction,
  NUM
};
```

然后，当按下交互键并且我们的模型播放交互动画片段时，我们向中央行为类发送交互事件：

```
if (glfwGetKey(mRenderData.rdWindow, GLFW_KEY_U) ==
    GLFW_PRESS) {
    if (mRenderData.rdInteractWithInstanceId > 0) {
        mBehaviorManager->addEvent(
            mRenderData.rdInteractWithInstanceId,
            nodeEvent::interaction);
    }
}
```

最后，我们在男性模型的节点树中创建一个新的事件节点，设置挥手动作。新的挥手动作被两个设置实例为空闲状态以实现平滑动画混合的其他动作包围，并且有两个等待节点让我们完成动画（我们目前还没有来自实例到节点树的反馈）。

节点树的 resulting 部分可以在图 9.13 中看到：

![](img/3cfb9832c943f5f7366fd104b5255971_323_0.png)

图 9.13：通过播放挥手动画对交互事件做出反应


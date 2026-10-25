

请注意，在场景初始化期间，将创建一个 `MessagingSystem` 单例对象，当 `EnemyManagerWithMessagesComponent` 对象或 `EnemyCreatedListenerComponent` 对象的 `Start()` 方法被调用时（以先发生者为准），因为这是它们向消息系统注册其委托的时候，而消息系统会访问 `Instance` 属性，从而创建包含单例组件所需的 `GameObject`。我们无需额外努力来创建 `MessagingSystem` 对象。

## 消息清理

由于消息对象是类，它们会在内存中动态创建，并在消息被处理并分发给所有监听器后很快被销毁。然而，正如你在第 8 章《精通内存管理》中将学到的，这最终会导致垃圾回收，因为内存会随时间累积。如果我们的应用程序运行足够长的时间，最终会导致偶尔发生垃圾回收，这是 Unity 应用程序中意外且突然的 CPU 性能激增的最常见原因。因此，明智的做法是谨慎使用消息系统，并避免在每次更新时过于频繁地发送消息。

更需要考虑的重要清理操作是，如果对象需要被销毁，则注销委托。如果我们不妥善处理此问题，消息系统将保留委托引用，这会阻止对象被完全销毁并从内存中释放。

本质上，我们需要将每个 `AttachListener()` 调用与适当的 `DetachListener()` 调用配对，当对象被销毁、禁用，或者我们决定不再需要它在发送消息时被查询时。

`MessagingSystem` 类中的以下方法定义将为特定事件分离监听器：

```csharp
public bool DetachListener(System.Type type, MessageHandlerDelegate handler) {
    if (type == null) {
        Debug.Log("MessagingSystem: DetachListener failed due to having no " +
            "message type specified");
        return false;
    }

    string msgType = type.Name;

    if (!_listenerDict.ContainsKey(type.Name)) {
        return false;
    }

    List<MessageHandlerDelegate> listenerList = _listenerDict[msgType];
    if (!listenerList.Contains(handler)) {
        return false;
    }
    listenerList.Remove(handler);
    return true;
}
```

以下是将 `DetachListener()` 方法添加到我们的 `EnemyManagerWithMessagesComponent` 类中的示例用法：

```csharp
void OnDestroy() {
    if (MessagingSystem.IsAlive) {
        MessagingSystem.Instance.DetachListener(typeof(EnemyCreatedMessage),
            this.HandleCreateEnemy);
    }
}
```

请注意，此定义如何使用在 `SingletonComponent` 类中声明的 `IsAlive` 属性。这保护我们免受上述问题的影响，即在应用程序关闭期间意外创建一个新的 `MessagingSystem`，因为我们无法保证单例总是最后被销毁。

## 总结消息系统

恭喜，我们终于构建了一个功能齐全的全局消息系统，任何对象都可以与之交互，并使用它相互发送消息。这种方法的一个有用特性是它是类型无关的，这意味着消息发送者和监听器甚至不需要派生自任何特定类即可与消息系统交互；它只需要是一个提供消息类型和匹配函数签名的委托函数的类，这使得它对普通类和 MonoBehaviours 都可访问。

就对 MessagingSystem 类进行基准测试而言，我们会发现它能够在单帧内处理数百甚至数千条消息，且 CPU 开销极小（当然取决于 CPU）。无论一条消息被分发给 100 个不同的监听器，还是 100 条消息被分发给一个监听器，CPU 使用率基本相同。无论哪种情况，成本大约相同。

即使我们主要在 UI 或游戏事件期间发送消息，这可能也远超我们所需。因此，如果它似乎确实造成了性能问题，那么更有可能是由监听器委托对消息所做的操作引起的，而不是消息系统处理这些消息的能力。

有许多方法可以增强消息系统，以提供我们将来可能需要的更多有用功能，如下：

- 允许消息发送者建议在消息传递给其监听器之前延迟（按时间或帧数计算）
- 允许消息监听者定义优先级，以决定与等待同一消息类型的其他监听者相比，它接收消息的紧迫程度。这使得监听者可以在注册晚于其他监听者时跳到队列前面
- 实现一些安全检查，以处理当监听者被添加到特定消息的监听者列表中，而该类型的消息仍在处理中的情况。目前，C# 会向我们抛出 EnumerationException，因为委托列表会被 AttachListener() 更改，而它仍在 TriggerEvent() 中被遍历

到此，我们可能已经充分探索了消息系统，因此这些任务将作为学术练习留给你，如果你在游戏中熟悉使用此解决方案，可以自行实施。让我们继续探索通过脚本代码提高性能的更多方法。

## 禁用未使用的脚本和对象

场景有时会变得非常繁忙，尤其是在构建大型开放世界时。调用 `Update()` 回调的对象越多，扩展性就越差，游戏运行就越慢。然而，如果对象在玩家视野之外，或者仅仅是太远而无关紧要，那么正在处理的许多内容可能完全没有必要。在大型城市建设模拟游戏中，这可能不可行，因为整个模拟必须始终处理，但在第一人称和赛车游戏中通常是可行的，因为玩家在广阔的区域中游荡，非可见对象可以暂时禁用，而不会对游戏玩法产生任何明显影响。

## 按可见性禁用对象

有时，我们可能希望当组件或 `GameObject` 不可见时将其禁用。Unity 自带内置渲染功能，用于避免渲染对玩家相机不可见的对象（通过一种称为 `Frustum Culling` 的技术，这是一个自动过程），以及避免渲染被其他对象遮挡的对象（遮挡剔除，将在 `第 6 章，动态图形` 中讨论），但这些仅是渲染优化。它们不影响在 CPU 上执行任务的组件，例如 AI 脚本、用户界面和游戏逻辑。我们必须自己控制该行为。

解决此问题的一个好方法是使用 `OnBecameVisible()` 和 `OnBecameInvisible()` 回调。正如名称所暗示的，当场景中任何相机视角下的可渲染对象变得可见或不可见时，会调用这些回调方法。此外，当场景中有多个相机时（例如，本地多人游戏），只有当对象对任意一个相机可见且对所有相机不可见时，才会调用回调。这意味着上述回调将在我们预期的准确时刻被调用；如果无人能看到它，`OnBecameInvisible()` 会被调用，如果至少有一个玩家能看到它，`OnBecameVisible()` 会被调用。

由于可见性回调必须与渲染管道通信，`GameObject` 必须附加一个可渲染组件，例如 `MeshRenderer` 或 `SkinnedMeshRenderer`。我们必须确保我们希望接收可见性回调的组件也附加到与可渲染对象相同的 `GameObject` 上，而不是父级或子级 `GameObject`，否则它们不会被调用。


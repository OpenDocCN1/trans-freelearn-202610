

请注意，这意味着我们不需要在单例组件类定义中实现 `static` 方法。例如，我们可以直接调用 `EnemyManagerSingletonComponent.Instance.KillAll()` 来访问 `KillAll()` 方法。

请注意，可以将 `SingletonComponent` 的实例放置在 **Hierarchy** 窗口中，因为它继承自 `MonoBehaviour`。但是，请注意 `DontDestroyOnLoad()` 方法永远不会被调用，这将阻止单例组件的 `GameObject` 在加载下一个场景时持续存在。我们可能需要在派生类的 `Awake()` 回调中调用 `DontDestroyOnLoad()` 才能使其工作，除非我们确实想要可销毁的单例。有时，允许此类单例在场景之间被销毁是有意义的，以便每次都能从头开始；这完全取决于我们的特定用例。

无论哪种情况，单例组件的关闭都可能有点复杂，因为 Unity 拆卸场景的方式。当对象在运行时被销毁时，会调用其 `OnDestroy()` 回调。在应用程序关闭期间，也会调用相同的方法，此时 Unity 会调用每个 `GameObject` 上每个组件的 `OnDestroy()` 回调。当我们在编辑器中结束播放模式并返回编辑模式时，也会发生相同的活动。但是，对象的销毁顺序是随机的，我们不能假设 `SingletonComponent` 对象会是最后一个被销毁的对象。

因此，如果任何对象在其 `OnDestroy()` 回调期间尝试对单例组件执行任何操作，则它们可能会调用 `SingletonComponent` 对象的 `Instance` 属性。但是，如果单例组件在此之前已经被销毁，则在应用程序关闭期间会创建一个新的 `SingletonComponent` 实例。这可能会损坏我们的场景文件，因为我们的单例组件的实例会留在场景中。如果发生这种情况，Unity 将抛出以下错误消息：

"Some objects were not cleaned up when closing the scene. (Did you spawn new GameObjects from OnDestroy?)"

显而易见的解决方法是，在任何 `MonoBehaviour` 组件的 `OnDestroy()` 回调期间，根本不要调用 `SingletonComponent` 对象。但是，我们确实有一些合法的理由可能希望这样做：最值得注意的是，单例通常设计为使用观察者设计模式。这种设计模式允许其他对象为某些任务向其注册/注销，类似于 Unity 如何挂钩回调方法，例如 `Start()` 和 `Update()`，但方式更严格。

使用观察者设计模式时，对象通常会在创建时向系统注册，在运行时使用它，然后在完成使用时在运行时注销，或在自身关闭期间注销以进行清理。我们将在即将到来的部分“全局消息系统”中看到这种设计模式的示例，但如果我们想象一个使用此类系统的 MonoBehaviour，那么执行关闭注销的最方便的位置将是在 OnDestroy() 回调中。因此，此类对象可能会遇到上述问题，即在应用程序关闭期间意外为 SingletonComponent 创建了一个新的 GameObject。

要解决此问题，我们需要进行三项更改。首先，我们需要为 SingletonComponent 添加一个额外的标志，以跟踪其活动状态并在适当的时间禁用它。这包括单例自身的销毁，以及应用程序关闭（OnApplicationQuit() 是 MonoBehaviours 的另一个有用 Unity 回调，在此期间会调用它）：

```
private bool _alive = true;
void OnDestroy() { _alive = false; }
void OnApplicationQuit() { _alive = false; }
```

其次，我们应该实现一种方法，让外部对象验证单例的当前状态：

```
public static bool IsAlive {
 get {
  if (__Instance == null)
   return false;
  return __Instance._alive;
 }
}
```

最后，任何在其自身 OnDestroy() 方法期间尝试调用单例的对象，都必须先使用 IsAlive 属性验证状态，然后再调用实例，如下所示：

```
public class SomeComponent : MonoBehaviour {
 void OnDestroy() {
  if (MySingletonComponent.IsAlive) {
   MySingletonComponent.Instance.SomeMethod();
  }
 }
}
```

这将确保没有人在销毁期间尝试访问单例实例。如果我们不遵循此规则，那么我们将遇到问题，即在返回编辑模式后，我们的单例对象的实例会留在场景中。

`SingletonComponent` 方法的讽刺之处在于，我们使用 `Find()` 调用来确定场景中是否已经存在此类 `SingletonComponent` 对象，然后再尝试分配 `__Instance` 引用变量。幸运的是，这只会在首次访问单例组件时发生，如果场景中没有太多 `GameObjects`，这通常不是问题，但单例组件的初始化不一定会在场景初始化期间发生，因此在游戏过程中首次获取实例并调用 `Find()` 时，可能会在不合适的时机导致性能尖峰。解决方法是让某个 God Class 通过简单访问每个单例的 `Instance` 属性，来确认重要的单例在场景初始化期间被实例化。

这种方法的另一个缺点是，如果我们后来决定想要同时运行多个此类单例，或者希望将其行为分离得更模块化，那么将需要修改大量代码。

我们将探讨的最后一种方法将尝试解决之前方案揭示的许多问题，并提供一种获得所有优点的方式，通过结合实现简便、扩展简便和严格使用，同时减少配置过程中人为错误的可能性。

## 全局消息系统

解决对象间通信问题的最后一种建议方法是实现一个全局消息系统，任何对象都可以访问并通过它向任何可能有兴趣监听特定类型消息的对象发送消息。对象可以发送消息或监听消息（有时两者都做！），监听者负责决定他们感兴趣的消息。消息发送者可以广播消息而不关心谁在监听，消息可以通过系统发送，而不考虑消息的具体内容。这种方法是迄今为止最复杂的，可能需要一些努力来实现和维护，但它是保持对象通信模块化、解耦和快速的绝佳长期解决方案，随着应用程序变得越来越复杂。

我们希望发送的消息类型可以有多种形式，包括数据值、引用、监听者指令等，但它们都应该有一个基本的共同定义，以便我们的消息系统可以用来确定消息是什么以及消息是发给谁的。

以下是 `Message` 对象的简单类定义：

```
public class Message {
    public string type;
    public Message() { type = this.GetType().Name; }
}
```

`Message` 类的构造函数将消息的 `type` 缓存在本地 `string` 属性中，以便以后用于编目和分发。缓存此值很重要，因为每次调用 `GetType().Name` 都会分配一个新字符串，我们之前学过，我们希望尽可能减少这种活动。

任何自定义消息都可以包含它们想要的任何多余数据，只要它们从这个基类派生，这将允许它们通过我们的消息系统发送。请注意，尽管在基类构造函数期间从对象获取了 `type`，但 `name` 属性仍然会包含派生类的名称，而不是基类的名称。

接下来是我们的 `MessagingSystem` 类，我们应该根据需要满足的要求来定义其功能：

- 它应该是全局可访问的
- 任何对象（`MonoBehaviour` 或非 `MonoBehaviour`）都应该能够注册/注销为监听者以接收特定消息类型（即观察者设计模式）
- 注册的对象应该提供一个方法，当给定消息从其他地方广播时调用该方法
- 系统应该在合理的时间范围内将消息发送给所有监听者，但不会因一次请求过多而卡顿

### 全局可访问的对象

第一个要求使消息系统成为单例对象的绝佳候选者，因为我们只需要系统的一个实例。但是，在决定实现单例之前，仔细考虑这是否确实是明智的。


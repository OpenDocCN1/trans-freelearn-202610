

不幸的是，成功重生 `GameObject` 要复杂得多。当我们重生一个对象时，该对象之前处于活动状态时留下的许多设置都会被保留，并且必须重置这些设置，以避免出现冲突行为。一个常见的问题是 Rigidbody 的 `linearVelocity` 和 `angularVelocity` 属性。如果在对象重新激活之前没有显式重置这些值，那么新重生的对象将继续以旧版本在被销毁时具有的相同速度移动。

由于内置组件被密封（sealed），这意味着它们不能被继承，因此问题变得更加复杂。所以，为了避免这些问题，我们可以创建一个自定义组件，在对象被销毁时重置附加的 Rigidbody：

```csharp
public class ResetPooledRigidbodyComponent : MonoBehaviour,
IPoolableComponent {
[SerializeField] Rigidbody _body;
public void Spawned() { }
public void Despawned() {
if (_body == null) {
_body = GetComponent<Rigidbody>() ;
if (_body == null) {
// no Rigidbody!
return;
}
}
_body.velocity = Vector3.zero;
_body.angularVelocity = Vector3.zero;
}
}
```

请注意，执行清理任务的最佳位置是在销毁期间，因为我们无法确定 `GameObject` 类的 `IPoolableComponent` 接口类的 `Spawned()` 方法会以什么顺序被调用。在销毁期间，另一个 `IPoolableComponent` 改变对象速度的可能性不大，但附加到同一对象的不同 `IPoolableComponent` 可能希望在其自身的 `Spawned()` 方法期间将 Rigidbody 的初始速度设置为某个重要值。因此，在 `ResetPooledRigidbodyComponent` 类的 `Spawned()` 方法中执行速度重置可能会与其他组件发生冲突，并导致一些非常令人困惑的错误。

事实上，创建不自包含且倾向于像这样干预其他组件的可池化组件，是实现池化系统时最大的危险之一。我们应该尽量减少此类设计，并在尝试调试游戏中奇怪问题时常规验证它们。

为了说明，这里是一个简单的可池化组件的定义，它使用了我们在第 2 章“脚本策略”中定义的 MessagingSystem 类。这个组件在对象每次生成和销毁时自动处理一些基本任务：

```csharp
public class PoolableTestMessageListener : MonoBehaviour,
    IPoolableComponent {
    public void Spawned() {
        MessagingSystem.Instance.AttachListener(typeof(MyCustomMessage),
            this.HandleMyCustomMessage);
    }

    bool HandleMyCustomMessage(BaseMessage msg) {
        MyCustomMessage castMsg = msg as MyCustomMessage;
        Debug.Log(string.Format("Got the message! {0}, {1}",
            castMsg._intValue,
            castMsg._floatValue));
        return true;
    }

    public void Despawned() {
        if (MessagingSystem.IsAlive) {
            MessagingSystem.Instance.DetachListener(typeof(MyCustomMessage),
                this.HandleMyCustomMessage);
        }
    }
}
```

## 预制体池化系统

希望我们现在了解了池化系统需要具备的功能，剩下的就是实现它。要求如下：

- 它必须接受从预制体生成 `GameObject` 的请求，以及初始位置和初始旋转：
  - 如果已存在已销毁的版本，则应重生第一个可用的对象
  - 如果不存在，则应从预制体实例化一个新的 `GameObject`
  - 在任一情况下，都应在附加到 `GameObject` 的所有 `IPoolableComponent` 接口类上调用 `Spawned()` 方法

- 它必须接受销毁特定 `GameObject` 的请求：
  - 如果该对象由池化系统管理，则应使其非活动状态，并在附加到 `GameObject` 的所有 `IPoolableComponent` 接口类上调用 `Despawned()` 方法
  - 如果该对象未由池化系统管理，则应发送错误

要求相当简单，但如果我们希望使解决方案对性能友好，实现过程需要一些调查。首先，典型的单例将是主入口点的良好选择，因为我们希望该系统可以从任何地方全局访问：

```csharp
public static class PrefabPoolingSystem {}
```

对象生成的主要任务涉及接受预制体引用，并确定我们是否有任何最初从同一引用实例化的已销毁 `GameObject`。为此，我们基本上希望池化系统为任何给定的预制体引用跟踪两个不同的列表：一个活动（已生成）`GameObject` 列表，以及一个非活动（已销毁）对象列表，这些对象是从该预制体实例化的。这些信息最好抽象为一个单独的类，我们将其命名为 `PrefabPool`。

为了最大限度地提高此系统的性能（因此，相对于不断从内存中分配和释放对象，可以获得最大的收益），我们希望使用一些快速的数据结构，以便在每次生成或销毁请求到来时获取相应的 `PrefabPool` 对象。

由于生成涉及给定预制体，我们希望有一个能够快速将预制体映射到管理它们的 `PrefabPool` 的数据结构。同样，由于销毁涉及给定 GameObject，我们希望有另一个能够快速将已生成的 GameObject 映射到最初生成它们的 `PrefabPool` 的数据结构。一对字典将是这两个需求的良好选择。

让我们在 `PrefabPoolingSystem` 类中定义这些字典：

```csharp
public static class PrefabPoolingSystem {
static Dictionary<GameObject, PrefabPool> _prefabToPoolMap = new
Dictionary<GameObject, PrefabPool>();
static Dictionary<GameObject, PrefabPool> _goToPoolMap = new
Dictionary<GameObject, PrefabPool>();
}
```

接下来，我们将定义生成对象时发生的情况：

```csharp
public static GameObject Spawn(GameObject prefab, Vector3 position,
Quaternion rotation) {
if (!_prefabToPoolMap.ContainsKey (prefab)) {
_prefabToPoolMap.Add (prefab, new PrefabPool());
}
PrefabPool pool = _prefabToPoolMap[prefab];
GameObject go = pool.Spawn(prefab, position, rotation);
_goToPoolMap.Add (go, pool);
return go;
}
```

Spawn() 方法将接收一个预制体引用、一个初始位置和一个初始旋转。我们需要找出预制体属于哪个 PrefabPool（如果有），要求它使用提供的数据生成一个新的 GameObject，然后将生成的对象返回给请求者。我们首先检查预制体到池的映射，以查看是否已经为此预制体存在一个池。如果不存在，我们立即创建一个。在任一情况下，我们然后要求 PrefabPool 为我们生成一个新对象。PrefabPool 最终将重生一个之前被销毁的对象，或者实例化一个新对象（如果没有非活动实例剩余）。

这个类并不特别关心 `PrefabPool` 如何创建对象。它只想要由 `PrefabPool` 类生成的实例，以便将其放入 `GameObject` 到池的映射中并返回给请求者。

为了方便，我们还可以定义一个重载，将对象放置在世界中心。这对于不可见且只需要存在于场景中的 `GameObject` 很有用：

```csharp
public static GameObject Spawn(GameObject prefab) {
    return Spawn (prefab, Vector3.zero, Quaternion.identity);
}
```

请注意，目前还没有实际发生生成和销毁。这项任务最终将在 `PrefabPool` 类中实现。

销毁涉及给定一个 `GameObject`，然后找出哪个 `PrefabPool` 正在管理它。这可以通过遍历我们的 `PrefabPool` 对象并检查它们是否包含给定的 `GameObject` 来实现。但是，如果我们最终生成了大量的 Prefab 池，那么这个迭代过程可能需要一段时间。我们最终将拥有与拥有的预制体数量一样多的 `PrefabPool` 对象（至少，只要我们通过池化系统管理所有预制体）。大多数项目往往有几十个、几百个，甚至几千个不同的预制体。

因此，`GameObject` 到池的映射被维护，以确保我们始终能快速访问最初生成该对象的 `PrefabPool`。它还可以用于快速检查给定的 `GameObject` 是否最初就由池化系统管理。以下是销毁方法的定义，它处理这些任务：

```csharp
public static bool Despawn(GameObject obj) {
    if (!_goToPoolMap.ContainsKey(obj)) {
        Debug.LogError (string.Format ("Object {0} not managed by pool system!", obj.name));
        return false;
    }

    PrefabPool pool = _goToPoolMap[obj];
    if (pool.Despawn (obj)) {
        _goToPoolMap.Remove (obj);
        return true;
    }
    return false;
}
```


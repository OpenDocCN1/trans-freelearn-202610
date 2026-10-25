

## 通过距离禁用对象

在其他情况下，我们可能希望组件或 `GameObjects` 在距离玩家足够远、几乎不可见但又远到无关紧要时被禁用。这种活动的一个很好候选对象是我们希望在远处看到的漫游 AI 生物，但不需要它处理任何事情，它可以保持空闲状态，直到我们靠近。

以下代码是一个简单的协程，它会定期检查与给定目标对象的总距离，如果偏离太远，则禁用自身：

```
[SerializeField] GameObject _target;
[SerializeField] float _maxDistance;
[SerializeField] int _coroutineFrameDelay;
void Start() {
    StartCoroutine(DisableAtADistance());
}
```

请注意，Unity 也会将 **Scene** 窗口的隐藏摄像机计入 `OnBecameVisible()` 和 `OnBecameInvisible()` 回调中。如果我们发现这些方法在 Play Mode 测试期间未被正确调用，请确保将 **Scene** 窗口摄像机转向远离所有内容，或完全禁用 **Scene** 窗口。

要使用可见性回调启用/禁用单个组件，我们可以添加以下方法：

```
void OnBecameVisible() { enabled = true; }
void OnBecameInvisible() { enabled = false; }
```

另外，要启用/禁用组件所附加的整个 `GameObject`，我们可以这样实现这些方法：

```
void OnBecameVisible() { gameObject.SetActive(true); }
void OnBecameInvisible() { gameObject.SetActive(false); }
```

但是，请注意，禁用包含可渲染对象的 `GameObject` 或其任何父对象，将使 `OnBecameVisible()` 无法被调用，因为现在没有图形表示供摄像机看到并触发回调。我们应该将组件放在子 `GameObject` 上，并让脚本禁用该子对象，而让可渲染对象始终保持可见（或稍后找到另一种方式重新启用它）。

```
IEnumerator DisableAtADistance() {
    while(true) {
        float distSqrd = (transform.position -
        _target.transform.position).sqrMagnitude;
        if (distSqrd < _maxDistance * _maxDistance) {
            enabled = true;
        } else {
            enabled = false;
        }

        for (int i = 0; i < _coroutineFrameDelay; ++i) {
            yield return new WaitForEndOfFrame();
        }
    }
}
```

我们应该将玩家角色对象（或我们想要比较的任何对象）分配给 **Inspector** 窗口中的 `_target` 字段，在 `_maxDistance` 中定义最大距离，并使用 `_coroutineFrameDelay` 字段修改协程被调用的频率。每当对象距离分配给 `_target` 的对象超过 `_maxDistance` 距离时，它将被禁用。如果它返回该距离内，将被重新启用。

此实现的一个微妙的性能增强特性是比较距离的平方而不是原始距离。这便引出了我们的下一节。

## 考虑使用距离平方而非距离

可以安全地说，CPU 对相乘浮点数相对擅长，但对从中计算平方根相对糟糕。每次我们要求 `Vector3` 使用 `magnitude` 属性或 `Distance()` 方法计算距离时，我们都在要求它执行平方根计算（根据勾股定理），与许多其他类型的向量数学计算相比，这可能会带来大量的 CPU 开销。

然而，`Vector3` 类还提供了一个 `sqrMagnitude` 属性，它提供与距离相同的结果，只是值被平方了。这意味着，如果我们也对要比较距离的值进行平方，那么我们就可以在不承担昂贵的平方根计算成本的情况下，执行基本相同的比较。

例如，考虑以下代码：
```
float distance = (transform.position - other.transform.position).Distance();
if (distance < targetDistance) {
    // do stuff
}
```

这可以替换为以下代码，并实现几乎相同的结果：
```
float distanceSqrd = (transform.position -
other.transform.position).sqrMagnitude;
if (distanceSqrd < (targetDistance * targetDistance)) {
    // do stuff
}
```

结果几乎相同的原因是浮点精度。由于值将被调整到具有不同可表示数字密度的区域，我们可能会失去使用平方根值时拥有的某些精度；它可能正好落在，或更接近一个更准确的可表示数字，或者更可能的是，它会落在一个精度较低的数字上。因此，比较并不完全相同，但在大多数情况下，它足够接近以至于无法察觉，而以这种方式替换的每条指令，性能提升可能非常显著。

如果这种微小的精度损失不重要，那么应该考虑这个性能技巧。但是，如果精度非常重要（例如运行准确的大规模银河系太空模拟），那么你可能想跳过这个技巧。

请注意，此技术可用于任何平方根计算，而不仅仅是距离。这只是你可能遇到的最常见的例子，它揭示了 `Vector3` 类的重要 `sqrMagnitude` 属性。Unity Technologies 故意暴露了这个属性，以便我们以这种方式使用它。

## 最小化反序列化行为

Unity 的序列化系统主要用于场景、预制体、ScriptableObjects 和各种资源类型（这些类型通常派生自 ScriptableObject）。当这些对象类型之一保存到磁盘时，它会被转换为使用 **Yet Another Markup Language (YAML)** 格式的文本文件，以后可以反序列化回原始对象类型。当预制体或场景被序列化时，所有 `GameObjects` 及其属性都会被序列化，包括 `private` 和 `protected` 字段、所有组件及其子 `GameObjects` 和组件，等等。

当我们构建应用程序时，这些序列化数据会在内部捆绑在大型二进制数据文件中，在 Unity 中称为序列化文件。在运行时从磁盘读取和反序列化这些数据是一个极其缓慢的过程（相对而言），因此所有反序列化活动都伴随着显著的性能开销。

每当我们调用 `Resources.Load()` 加载位于名为 `Resources` 的文件夹下的文件路径时，就会发生这种反序列化。一旦数据从磁盘加载到内存中，以后重新加载相同引用会快得多，但首次访问时总是需要磁盘活动。自然，我们需要反序列化的数据集越大，这个过程花费的时间就越长。由于预制体的每个组件都会被序列化，层级越深，需要反序列化的数据就越多。这对于层级非常深的预制体、包含许多空 `GameObjects` 的预制体（因为每个 `GameObject` 始终至少包含一个 `Transform` 组件）来说可能是个问题，对于用户界面（UI）预制体尤其成问题，因为它们通常包含比典型预制体多得多的组件。

加载这些大型序列化数据集在首次加载时可能会导致 CPU 显著峰值，如果它们需要在场景开始时立即使用，往往会增加加载时间。更重要的是，如果它们在运行时加载，可能会导致帧率下降。我们可以使用几种方法来最小化反序列化的开销。

## 减少序列化对象大小

我们应该尽量使序列化对象尽可能小，或者将它们分区为较小的数据片段，逐块组合，以便可以逐块随时间加载。对于预制体来说，这可能难以管理，因为 Unity 不原生支持嵌套预制体，因此我们需要自己实现这样的系统，这在 Unity 中是一个臭名昭著的难题。UI 预制体是分割成较小片段的良好候选对象，因为我们通常不需要在任何给定时刻加载整个 UI，因此我们通常可以负担得起一次加载一个片段。

## 异步加载序列化对象

预制体和其他序列化内容可以通过 `Resources.LoadAsync()` 异步加载，这将把从磁盘读取卸载到工作线程，从而减轻主线程的负担。序列化对象需要一些时间才能可用，可以通过调用前一方法返回的 `ResourceRequest` 对象上的 `isDone` 属性来检查。


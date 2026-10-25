

也许 Unity 脚本中性能问题最常见的根源，就是滥用 `Update()` 回调，具体表现为以下一种或多种行为：

- 反复重新计算一个很少或根本不会改变的值
- 让过多的组件为本可以共享的结果执行工作
- 执行工作的频率远高于必要程度

值得养成的习惯是牢记：我们在 `Update()` 回调以及由这些回调调用的函数中编写的每一行代码，都会侵蚀我们的帧率预算。要达到 60 fps，我们每帧只有 16.667 毫秒来完成所有 `Update()` 回调中的所有工作。在开始原型制作时，这似乎是充足的时间，但在开发中期，我们可能会开始注意到事情变得越来越慢、响应越来越迟钝，因为我们一直在逐渐消耗这个预算，原因是我们不受控制地想往项目中塞入更多内容。

让我们来看看一些直接解决这些问题的技巧。

## 缓存组件引用

在 Unity 中编写脚本时，反复重新计算一个值是一个常见的错误，尤其是在涉及 `GetComponent()` 方法时。例如，以下脚本代码试图检查生物的生命值，如果生命值低于 `0`，它将禁用一系列组件，为死亡动画做准备：

```c
void TakeDamage() {
    RigidBody rigidbody = GetComponent<RigidBody>();
    Collider collider = GetComponent<Collider>();
    AIControllerComponent ai = GetComponent<AIControllerComponent>();
    Animator anim = GetComponent<Animator>();

    if (GetComponent<HealthComponent>().health < 0) {
        rigidbody.enabled = false;
        collider.enabled = false;
        ai.enabled = false;
        anim.SetTrigger("death");
    }
}
```

每次执行这个优化不佳的方法时，都会重新获取五个不同的组件引用。这对 CPU 使用率并不友好。如果主方法是在 `Update()` 中调用的，这尤其成问题。即使不是，它仍然可能与其他重要事件同时发生，例如创建粒子效果、用布娃娃替换对象（从而触发物理引擎中的各种活动）等等。这种编码风格看似无害，但可能会带来很多长期问题和运行时工作，而收益却很小。

缓存这些引用以供将来使用，只需花费少量内存空间（每次仅 32 或 64 位；取决于 Unity 版本、平台和碎片情况）。因此，除非你在内存上极度瓶颈，否则更好的方法是在初始化期间获取引用，并保留它们直到需要：

```c
private HealthComponent _healthComponent;
private Rigidbody _rigidbody;
private Collider _collider;
private AIControllerComponent _ai;
private Animator _anim;

void Awake() {
    _healthComponent = GetComponent<HealthComponent>();
    _rigidbody = GetComponent<Rigidbody>();
    _collider = GetComponent<Collider>();
    _ai = GetComponent<AIControllerComponent>();
    _anim = GetComponent<Animator>();
}

void TakeDamage() {
    if (_healthComponent.health < 0) {
        _rigidbody.detectCollisions = false;
        _collider.enabled = false;
        _ai.enabled = false;
        _anim.SetTrigger("death");
    }
}
```

以这种方式缓存组件引用，可以避免每次需要时都重新获取它们，每次都能节省一些 CPU 开销。代价是少量额外的内存消耗，这通常是值得的。

同样的技巧适用于我们决定在运行时计算的几乎任何数据。没有必要让 CPU 在每次 `Update()` 中不断重新计算同一个值，我们可以直接将其存储在内存中以供将来引用。

## 共享计算输出

通过让多个对象共享某个计算的结果，可以节省性能；当然，这只有在所有对象都会生成相同结果时才有效。这种情况通常容易发现，但重构起来可能比较棘手，因此利用这一点高度依赖于具体实现。

一些示例可能包括：在场景中查找对象、从文件读取数据、解析数据（如 XML 或 JSON）、在大列表或深层信息字典中查找某物、为一组 **人工智能（AI）** 对象计算路径、复杂的类似轨迹的数学计算、射线检测等等。

考虑每次执行昂贵操作时，思考它是否从多个位置调用但始终产生相同的输出。如果是这种情况，那么最好重构事物，使结果只计算一次，然后分发给每个需要它的对象，以尽量减少重新计算的数量。最大的成本通常只是代码简单性的微小损失，尽管传递值可能会带来一些额外开销。

请注意，很容易养成将某个大型复杂函数隐藏在基类中的习惯，然后定义使用该函数的派生类，完全忘记该函数有多昂贵，因为我们很少再查看那段代码。最好使用 Unity 性能分析器来告诉我们那个昂贵函数可能被调用多少次，一如既往，除非已被证明是性能问题，否则不要过早优化那些函数。无论它多么昂贵，如果它不会导致我们超过性能限制（如帧率和内存消耗），那么它就不算真正的性能问题。

## Update、协程和 InvokeRepeating

另一种容易陷入的习惯是在 `Update()` 回调中以远高于必要的频率反复调用某些东西。例如，我们可能从以下情况开始：

```c
void Update() {
    ProcessAI();
}
```

在这种情况下，我们每帧都在调用某个自定义的 `ProcessAI()` 子例程。这可能是一项复杂的任务，需要 AI 系统检查某个网格系统来确定它应该移动到哪里，或确定一组飞船的舰队机动，或者我们游戏 AI 所需的任何其他内容。

如果这项活动过度侵蚀了我们的帧率预算，并且该任务可以以低于每帧一次的频率完成且没有显著缺点，那么一个改善性能的好技巧就是简单地降低 `ProcessAI()` 被调用的频率：

```c
private float _aiProcessDelay = 0.2f;
private float _timer = 0.0f;
void Update() {
    _timer += Time.deltaTime;
    if (_timer > _aiProcessDelay) {
        ProcessAI();
        _timer -= _aiProcessDelay;
    }
}
```

在这种情况下，我们通过每秒仅调用 `ProcessAI()` 约五次，降低了 `Update()` 回调的总体开销，这比之前的情况有所改进，代价是代码乍一看可能需要一点时间才能理解，以及少量额外内存来存储一些浮点数据。不过，归根结底，我们仍然让 Unity 比必要时更频繁地调用一个空回调函数。

这个函数是一个完美的示例，我们可以将其转换为协程，以利用其延迟调用属性。如前所述，协程通常用于编写短事件序列，无论是一次性还是重复动作。它们不应与线程混淆，线程会在完全不同的 CPU 核心上以并发方式运行，并且多个线程可以同时运行。相反，协程在主线程上以顺序方式运行，这样在任何给定时刻只处理一个协程，并且每个协程通过 `yield` 语句决定何时暂停和恢复。以下代码是将上述 `Update()` 回调重写为协程的示例：

```c
void Start() {
    StartCoroutine(ProcessAICoroutine());
}
```


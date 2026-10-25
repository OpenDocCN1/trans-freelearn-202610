

IEnumerator ProcessAICoroutine () {
    while (true) {
        ProcessAI();
        yield return new WaitForSeconds (_aiProcessDelay);
    }
}

前面的代码演示了一个协程，它调用 `ProcessAI()`，然后在 `yield` 语句处暂停指定的秒数（即 `_aiProcessDelay` 的值），直到主线程再次恢复该协程。此时，它将返回到循环的开头，调用 `ProcessAI()`，再次在 `yield` 语句处暂停，并永远重复（通过 `while(true)` 语句），直到被要求停止。

这种方法的主要好处是，该函数的调用频率仅由 `_aiProcessDelay` 的值决定，并且在该时间到来之前将保持空闲状态，从而减少了在大多数帧中造成的性能损耗。然而，这种方法也有其缺点。

首先，启动协程相对于标准函数调用会带来额外的开销成本（大约慢三倍），并且还需要一些内存分配来存储当前状态，直到下次调用时才会被使用。这种额外的开销也不是一次性成本，因为协程通常会不断调用 `yield`，这会一次又一次地造成相同的开销成本，因此我们需要确保降低频率带来的好处大于这一成本。

在对 1,000 个具有空 `Update()` 回调的对象进行测试时，处理耗时 1.1 毫秒，而 1,000 个在 `WaitForEndOfFrame` 上 `yield` 的协程（其频率与 `Update()` 回调相同）耗时 2.9 毫秒。因此，相对成本几乎是三倍。

其次，一旦初始化，协程将独立于触发它的 `MonoBehaviour` 组件的 `Update()` 回调运行，并且无论组件是否被禁用都会继续被调用，如果我们进行大量的 `GameObject` 构建和销毁，这可能会使它们难以处理。

第三，无论出于何种原因（无论是被设置为非活动状态还是其父对象之一被设置为非活动状态），协程都会在包含它的 `GameObject` 变为非活动状态的那一刻自动停止，并且如果 `GameObject` 再次设置为活动状态，它不会自动重新启动。

最后，通过将方法转换为协程，我们可能减少了在大多数帧中造成的性能损耗，但如果方法体的单次调用导致我们超出了帧率预算，那么无论我们调用该方法的频率多么低，预算仍然会被超出。因此，这种方法最适合于仅由于方法在给定帧中被调用的次数过多而导致超出帧率预算的情况，而不是因为该方法本身开销过大。在这些情况下，我们别无选择，只能深入研究并改进方法本身的性能，或降低其他任务的成本以腾出时间来完成其工作。

在生成协程时，我们有几种可用的 `yield` 类型。`WaitForSeconds` 相当直观；协程将在 `yield` 语句处暂停指定的秒数。然而，它不是精确的定时器，因此当此 `yield` 类型实际恢复时，请预期会有少量的变化。

`WaitForSecondsRealTime` 是另一个选项，它与 `WaitForSeconds` 的区别仅在于它使用未缩放时间。`WaitForSeconds` 与缩放时间进行比较，后者受全局 `Time.timeScale` 属性的影响，而 `WaitForSecondsRealTime` 则不受影响，因此如果您正在调整时间缩放值（例如，用于慢动作效果），请小心选择使用哪种 `yield` 类型。

还有 `WaitForEndOfFrame`，它将在下一个 `Update()` 结束时继续，还有 `WaitForFixedUpdate`，它将在下一个 `FixedUpdate()` 结束时继续。最后，Unity 5.3 引入了 `WaitUntil` 和 `WaitWhile`，我们提供一个委托函数，协程将暂停直到给定的委托分别返回 true 或 false。请注意，提供给这些 `yield` 类型的委托函数将在每个 `Update()` 中执行，直到它们返回停止所需的布尔值，这使得它们非常类似于在 `while` 循环中使用 `WaitForEndOfFrame` 并在满足特定条件时结束的协程。当然，我们提供的委托函数执行开销也不应过大。

> 委托函数是 C# 中非常有用的构造，它允许我们将局部方法作为参数传递给其他方法，通常用于回调。有关委托的更多信息，请参阅 MSDN C# 编程指南 https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/。

某些 `Update()` 回调的编写方式可能可以压缩为简单的协程，这些协程始终在上述类型之一上调用 `yield`，但我们应该注意前面提到的缺点。协程可能难以调试，因为它们不遵循正常的执行流程；调用堆栈中没有我们可以直接指责的调用者来解释为什么协程在给定时间触发，如果协程执行复杂任务并与其他子系统交互，则可能导致一些极其困难的错误，因为它们恰好在某些其他代码未预期的时刻被触发，这也往往是那些难以重现的棘手错误。如果您确实希望使用协程，最好的建议是保持它们简单且独立于其他复杂的子系统。

确实，如果我们的协程足够简单，可以归结为始终在 `WaitForSeconds` 或 `WaitForSecondsRealtime` 上调用 `yield` 的 `while` 循环（如上述示例所示），那么我们通常可以用 `InvokeRepeating()` 调用来替换它，这更容易设置并且开销略小。以下代码与之前使用协程定期调用 `ProcessAI()` 方法的实现功能等效：

void Start() {
    InvokeRepeating("ProcessAI", 0f, _aiProcessDelay);
}

`InvokeRepeating()` 与协程之间的一个重要区别是，`InvokeRepeating()` 完全独立于 `MonoBehaviour` 和 `GameObject` 的状态。停止 `InvokeRepeating()` 调用的唯一方法是调用 `CancelInvoke()`（它将停止给定 `MonoBehaviour` 发起的所有 `InvokeRepeating()` 回调，请注意它们不能单独取消）或销毁关联的 `MonoBehaviour` 或其父 `GameObject`。禁用 `MonoBehaviour` 或 `GameObject` 都不会停止 `InvokeRepeating()`。

对 1,000 个 `InvokeRepeating()` 调用的测试大约在 2.6 毫秒内处理完毕；略快于耗时 2.9 毫秒的 1,000 个等效协程 `yield` 调用。

这就涵盖了与 `Update()` 回调相关的大部分有用信息。让我们来看看一些其他有用的脚本技巧。

## 更快的 GameObject null 引用检查

事实证明，对 `GameObject` 执行 `null` 引用检查会导致一些不必要的性能开销。与典型的 C# 对象相比，`GameObject` 和 `MonoBehaviour` 是特殊的对象，因为它们在内存中有两种表示：一种存在于管理我们编写的 C# 代码的同一系统管理的内存中（托管代码），而另一种存在于单独处理的不同内存空间中（原生代码）。数据可以在这两个内存空间之间移动，但每次发生这种情况都会导致额外的 CPU 开销，并可能产生额外的内存分配。

这种效果通常被称为跨越 `Native-Managed Bridge`。如果发生这种情况，可能会为对象的数据生成额外的内存分配以跨越桥梁进行复制，这将需要垃圾回收器最终为我们执行一些自动内存清理。本主题将在第 8 章《精通内存管理》中更详细地探讨，但暂时请考虑，有许多微妙的方式可能会意外触发这种额外开销，而对 `GameObject` 进行简单的 `null` 引用检查就是其中之一：

```csharp
if (gameObject != null) {
    // do stuff with gameObject
}
```

一种替代方案是 `System.Object.ReferenceEquals()`，它生成功能等效的输出，运行速度大约快两倍（尽管它会稍微模糊代码的目的）：

```csharp
if (!System.Object.ReferenceEquals(gameObject, null)) {
    // do stuff with gameObject
}
```

这适用于 `GameObject` 和 `MonoBehaviour`，以及其他 Unity 对象，例如 `WWW` 类，它们都具有原生和托管表示。然而，一些初步测试表明，无论哪种 `null` 引用检查方法，在 Intel Core i5 3570K 处理器上都只消耗几个纳秒。因此，除非您正在进行大量的 `null` 引用检查，否则收益可能微乎其微。然而，这是一个值得牢记的警告，因为它将在未来频繁出现。


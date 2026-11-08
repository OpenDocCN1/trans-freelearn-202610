
在本节中，我们将使用 dispose() 方法来取消订阅 observable。

要创建 Disposable，我们可以使用 Rx.Disposable 中的 create() 函数：

```
var disposable = Rx.Disposable
    .create(()=>console.log('Releasing allocated resources'));

disposable.dispose();
```

如果运行此代码，它将在控制台中打印以下消息：

**释放已分配的资源**

如本章前面所述，我们可以调用 dispose() 方法来取消订阅 observable。在某些情况下，我们甚至可以定义一个 Disposable，以便在有人从此 observable 调用 dispose() 方法时使用（请记住 create() 和 createWithDisposable() 函数用于创建 observable）。

Disposable 类还为我们提供了控制 Disposable 对象组的功能。两种最重要的 Disposable 类型是 CompositeDisposable 和 RefCountDisposable。

CompositeDisposable() 对象包装其他 Disposables。因此，当你调用父对象的 dispose() 方法时，它会为所有子对象调用 dispose。你可以在以下示例中看到这种行为：

```
var firstDisposable = Rx.Disposable
    .create(()=>console.log('disposing first'));
var secondDisposable = Rx.Disposable
    .create(()=>console.log('disposing second'));

var composite = new
Rx.CompositeDisposable(firstDisposable,secondDisposable);

composite.dispose();
```

在此示例中，我们创建了两个 Disposable 对象（firstDisposable 和 secondDisposable）并将它们包装在 CompositeDisposable()（composite）之下。因此，当我们调用 CompositeDisposable() 对象（composite）的 dispose() 方法时，它也会调用两个子对象的 dispose() 方法（firstDisposable 和 secondDisposable）。

如果运行此代码，你将看到以下输出：

```
disposing first
disposing second
```

RefCountDisposable 对象则相反。它接受一个 Disposable 对象并允许你分发对此 Disposable 的引用（使用 getDisposable() 方法），但在所有引用都已被释放之前会保护它不被释放。这对于管理被多个 Observables/Observers 使用的资源非常有用。

以下代码展示了 RefCountDisposable 的用法：

```
var disposable = Rx.Disposable.create(function () {
    console.log('releasing connection');
});

var refCountDisposable = new Rx.RefCountDisposable(disposable);

var firstDisposable = refCountDisposable.getDisposable();
var secondDisposable = refCountDisposable.getDisposable();

firstDisposable.dispose();
secondDisposable.dispose();

console.log('first and second disposed. Disposing refCount');

refCountDisposable.dispose();
```

如果运行此代码，你将看到以下输出：

```
first and second disposed. Disposing refCount
releasing connection
```

输出显示我们需要释放所有内容才能释放 RefCountDisposable 子对象。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_94_0.png)

Disposables 是增强资源生命周期控制的重要特性。

## RxJS 调度器

调度器用于确定任务将在何处执行（当前线程、线程池等）。

你可以使用它们运行任何类型的任务，但 RxJS Observables 使用调度器来传播数据。因此，在创建新的 Observable 时，你可以可选地定义一个 Scheduler。

正如你所预期的那样，控制任务应在哪个执行上下文中运行（或 Observable 应在其中处理元素）特别是在多线程环境中非常有用。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_95_0.png)

由于 RxJS 在单线程环境中运行，除了默认设置外，在使用调度器从 Observable 传播数据时应格外谨慎。不当使用可能会阻塞你的线程。

## 摘要

在本章中，你学习了使用 RxJS 进行函数式响应式编程的基础知识，并且它是一个更广泛的框架这一点变得清晰。

我们开始使用不同的对象，如 Observables、Observers、Subjects、Disposables 和 Schedulers。

其中一些概念在 bacon.js 世界中甚至不存在，它们赋予我们对代码的更大控制力。

通过 Subjects，你学习了如何使用推送风格创建 Observable。

通过 Disposable，你获得了对代码生命周期的更多控制，因为它允许你优雅地清理资源。

通过 Schedulers，你学习到如果需要，你可以控制代码在哪个上下文中执行，从而对 Observables 如何传播数据拥有更大控制权。你还学习了在使用 RxJS 时避免使用除默认调度器之外的调度器的重要性。

在下一章中，我们将使用第一个操作符来转换数据，就像我们在上一章中使用 bacon.js 所做的那样。再次，我们将看到它如何带来更简洁、可读且可测试的代码。下一章讨论的操作符是最重要的；它们与 Observables 和 Observers 一起构成了函数式响应式编程的基础。

如果你使用诸如 `lodash` 或 `underscore` 之类的库，或者你跟上了 JavaScript 中 Array 的新方法，那么你可能已经使用过下一章讨论的一些操作符。这包括 `map()`、`flatMap()`、`filter()` 和 `reduce()` 操作符。

## 4

## 转换数据 - Map、Filter 和 Reduce

在上一章中，我们开始讨论在程序中使用 RxJS。为了开始使用它，我们将 Reactive Extensions 中的 Observables 与 bacon.js 中的 EventStreams 和 Properties 进行了比较。然后我们查看了一些可用于创建 Observables 的最常见数据源，如下所示：

- 数组
- Range
- Interval
- Promises 和回调
- DOM 事件
- 任意数据源

在对如何创建 Observables 的 RxJS API 进行了初步概述后，我们学习了如何订阅它们，以便在 Observable 中有新数据可用时采取行动。订阅 Observable 意味着不仅能够对新传入的数据做出反应，还能在发生错误或 Observable 完成时采取某些行动。为此，我们学习了如何使用 `subscribe()` 方法，使用函数或通过创建 Observer。

我们还学习了如何使用 Subjects。Subjects 允许我们创建新的 Observables 并持续向它们推送数据。

RxJS 为我们提供了对 Observables 甚至应用程序生命周期的更多控制。此外，在上一章中，我们使用了 Disposables 和 Schedulers。它们让我们能够拆除对象并控制如何在 Observable 序列中发布项目。

凭借我们迄今为止积累的知识，我们现在准备向我们的 observables 添加操作符，使其更具可重用性和可测试性。

在本章中，我们将查看构成任何函数式响应式程序骨干的四个操作符。它们如下：

- map()
- flatMap()
- filter()
- reduce()

这些是最广泛使用/最重要的操作符。本章将充满大量示例，帮助你理解这些操作符的用法。如果你能很好地掌握它们，它们将使你在函数式响应式编程中的道路变得容易得多，使你能够学习更具体的操作符以及如何组合操作符。

在本章中，你将学习以下主题：

- 向 Observables 添加操作符
- 使用 map() 和 flatMap() 操作符
- 使用 filter() 操作符
- 使用 reduce() 操作符

## 向 observables 添加操作符

在第 2 章《Reacting for the First Time》中，我们使用 bacon.js 使用了操作符。它们让我们在数据到达给定 Observable 的订阅者之前对其进行转换。RxJS 中的操作符的工作方式与 bacon.js 中的类似；有些甚至具有相同的名称。它们只是从 Observable 对象调用的方法，如以下示例所示：

```
Rx.Observable
  .just('Hello ')
  .map((msg)=>msg+'World')
  .subscribe((msg)=> console.log(msg));
```

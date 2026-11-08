
在下面的示例中，我们将使用 skip() 操作符来跳过 Observable 中的第一个元素：

```
Rx.Observable
    .of(1,2,3)
    .skip(1)
    .subscribe((i)=>console.log(i));
```

如果运行这段代码，它将打印以下内容：

2
3

此操作在下面的图表中进行了说明：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_144_0.png)

此操作符可以与 take() 操作符一起使用，从原始 Observable 中提取一个范围：

```
Rx.Observable
    .of(1,2,3,4)
    .skip(1)
    .take(2)
    .subscribe((i)=>console.log(i));
```

在这个示例中，我们从值 1、2、3 和 4 创建一个 Observable。然后我们跳过第一个，因此现在我们有元素：2 和 3。我们最后只取其中两个，所以如果运行这段代码，我们将看到以下输出：

2
3

## skipWhile() 操作符

本章的最后一个操作符 skipWhile() 操作符与 takeWhile() 操作符类似，但它会在给定的条件函数满足时跳过元素。它的签名如下：

```
observable.skipWhile(condition, [context]);
```

第一个参数是必需的，第二个是可选的：

- condition：这是一个用于测试要跳过的元素的函数
- context：这是要在条件函数中使用的参数

为了说明此操作符，我们可以使用 takeWhile() 中的相同示例，但改用 skipWhile() 函数：

```
Rx.Observable
    .of(2,4,5,6)
    .skipWhile((i)=>i%2===0)
    .subscribe((i)=>console.log(i));
```

在这段代码中，我们将从序列开始跳过偶数元素，并在找到第一个非偶数时停止。因此如果运行这段代码，我们将看到以下输出：

5
6

此操作在下面的图表中进行了说明：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_146_0.png)

## 总结

在本章中，我们学习了如何处理大量数据。我们称之为背压问题。我们可以使用两种不同的方法来处理背压：有损策略和无损策略。

我们还学习了每种方法的优点和缺点，以及可以用来缓解它们的运算符。

在本章结束时，我们还获得了更多查询 observables 中数据的工具，例如 `first()` 操作符，一些 `take()` 操作符和一些 `skip()` 操作符。

在下一章中，我们将学习用于组合不同数据源的操作符。

## 6

## 太多来源 - 组合 Observables

在上一章中，我们学习了如何使用不同的操作符来处理背压问题，学习了两种不同的策略来处理此问题：

- Lossy strategy
- Loss-less strategy

对于每种策略，我们学习了不同的操作符实现有损策略来处理背压问题。学习了以下操作符：

- `throttle()`
- `sample()`
- `debounce()`
- `pause()`

为了实现无损策略来处理背压问题，我们学习了以下操作符：

- `bufferWithCount()`
- `bufferWithTime()`
- `bufferWithTimeOrCount()`
- `pause()` using buffering
- `controlled()`

我们还学习了何时使用每种策略，这取决于我们可用的内存量以及是否能承受丢失任何数据。

最后，我们学习了用于过滤数据的新的操作符，以及 filter() 操作符。此操作符让我们避免在 observable 上计算不必要的事件。我们学习了以下用于过滤数据的操作符：

- `first()`
- `take()`
- `takeLast()`
- `takeWhile()`
- `skip()`
- `skipWhile()`

如果你没有熟悉背压问题。请花时间重新阅读本章，因为这个问题非常常见，任何自称函数式响应式编程大师的人都必须能够识别并缓解这个问题。

在本章中，我们将看到如何组合不同的 observables 以创建新的 observables，我们还将使用更复杂和详细的示例来教授新概念，让你走出舒适区并进一步提高你的函数式响应式编程技能。

在本章中，我们将学习以下主题：

- 我何时需要组合 observables？
- 运行 observables in parallel
- 串联 observables
- 组合来自不同 observables 的数据
- 在更复杂的示例中重用来自前几章的方法和操作符

## 我何时需要组合 observables？

函数式响应式编程的主要优点之一是它使代码重用变得容易，因为我们将数据源（observable）与我们对该数据进行的转换（操作符）分离，以及它发生时引起的影响（观察者采取的操作），我们可以轻松重用代码的每个部分。

在前几章中，我们看到了如何通过向其添加多个观察者来重用 observable。现在我们可以更进一步，将这些 observables 组合起来创建一个新的数据源；这将给我们更大的能力。

当我们组合（具体来说是串联）observables 时，我们还可以避免重复代码，这与良好代码的主要概念之一一致，即 **Don't Repeat Yourself (DRY)**。

一个简单且易于理解的组合 observables 以避免订阅两个 observables 的示例可能发生在常见任务中。想象一下，我们想在网站中实现一个搜索工具。在我们的搜索工具中，我们希望在用户按下 *Enter*（并且搜索框获得焦点）或用户点击搜索按钮时执行搜索。使用 RxJS，很容易使用 `fromEvent()` 方法为搜索框中的回车键点击创建 observable，并且也很容易使用相同的方法为搜索按钮的点击创建 observable。如果没有连接这两个 observables 的能力，我们将不得不订阅每个 observable 创建两个不同的订阅。这不仅是不必要的，而且还可能导致错误，因为任何订阅都可能单独终止。但正如我们将在本章中看到的，我们可以将这两个 observables 串联起来创建一个新的 observable，当任何源发出数据时它就会发出数据，并且我们只订阅此 observable 来创建更易读、容易出错且语义正确的代码。

我们还学习了如何使用 observables 来帮助我们控制应用程序的流程，使用它来包装 promises 或甚至回调。因此我们也可以组合 observables 以并行运行这些异步计算。

## 运行 observables

在 JavaScript 中处理异步计算是一项艰巨的任务，因为所有代码都在单个线程中运行。因此，为了保持该线程可用，大多数处理 I/O 的操作符使用回调在数据可用时将控制权返回给你的程序。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_149_0.png)

> 线程的概念以及 JavaScript 虚拟机内部的工作方式超出了本书的范围，了解它并不是必要的。

大量使用回调函数的通常会导致一个称为 **callback hell** 的问题，这基本上是代码中有太多回调，使其从程序员的角度来看很难阅读。为了缓解这个问题，promises 现在是标准 JavaScript 的一部分，并且被所有现代浏览器实现。由于 promises 本质上是可组合的，它使你的代码更简洁，并为你提供摆脱回调地狱的安全途径。

但 promises 只有基本的构造可以相互组合，因此 RxJS（以及 JavaScript 中大多数函数式响应式编程库）为你提供了内置方法将 promise 转换为 observable 使其在使用操作符时更具可组合性。

话虽如此，很容易看出为什么函数式响应式编程，凭借串联和组合 observables（可以是异步计算）的能力，是处理你的应用程序流程控制的自然解决方案。

## 串联 observables

RxJS 提供了一些操作符将多个 observables 组合成一个包含所有事件的 observable，最常见的操作符是 `concat()` 和 `merge()`，它们只是创建一个包含其他所有数据的新的 observable，但在本章中我们还将学习如何使用其他操作符进行更有趣的组合。

## 使用 concat() 操作符

串联两个数据源（observables）最常见的操作符是 `concat()` 操作符。此操作符接收多个 observables 作为参数，并从左到右串联所有 observables。它保留 observables 中元素的顺序，并且只有在当前 observable 已经终止时才传播下一个 observable 的数据。

此操作符具有以下签名：

```
observable.concat(observables);
```

它接受任意数量的参数，但它们都必须是 observables：

- `observable`：它是任意数量的 observables

# 3 变化的世界 - 响应式扩展来救援

# 变化的世界 - 响应式扩展来救援

In the previous chapter, you learned the very basics of functional reactive programming, using the `bacon.js` library. We discussed what is an observable and saw two different flavors of it in the `bacon.js` library (EventStream and Property). We also looked at how to create observables from common event sources (interval, array, or user input) and even from any other possible source ( using the `fromBinder()` method).

After an introduction to observables, we looked at how to subscribe to it. You can react to events, errors, or even detect the end of an observable when you subscribe to it. So, you learned how to create an observer for your observables.

One of the really important lessons from the previous chapter was the usage of diagrams to explain how observables and operators occur, along with text explanations and example code. Understanding diagrams is the key to understanding the behavior of operators and observables; you will see a lot of diagrams throughout the book. Read this section with special attention. Read it again in case you're not comfortable with it yet, but don't be afraid if it seems complicated (it seemed that way to me the first time I saw it). You will keep seeing it with text explanations and will eventually learn how it works.




前一章为你的工具箱添加了许多新概念，其中之一就是操作符。利用操作符将事件转换为全新事物，让你体验到函数式响应式编程的精髓，并让你看到其强大且可组合的特点。如果你喜欢最初使用的操作符，那就高兴吧；我们在接下来的章节中会看到更多操作符。操作符是函数式响应式编程的支柱，它们能轻松帮助你编写更简洁、更紧凑且可测试的代码。

最终，我们在 bacon.js 库中看到了可观察对象的惰性。本章我们将讨论 RxJS 中可观察对象的行为，所以不用担心。

本章将带你进入 Reactive Extensions 的世界。在本书中，我们将基于 RxJS 实现。然而，Reactive Extensions 是一个与语言无关的框架（这意味着它有多种语言的实现），因此这里描述的许多概念可以在其他平台中使用（如 RxJava、RxSwift 等）。这使得学习 Reactive Extensions（以及函数式响应式编程）非常有用，因为你可以用它来改进不同平台中的代码。

本章的重点是教你 RxJS 世界中可能的各种类型的可观察对象。你会看到它们与 bacon.js 的可观察对象略有不同，但使用起来很容易。你还将学习如何使用观察者对可观察对象中的事件做出反应，以及如何结束你的观察者。

本章将涵盖以下要点：

- Reactive Extensions 上下文中可观察对象的不同类型以及它们之间的区别
- 使用 RxJS API 创建可观察对象
- 对我们可观察对象中的事件做出反应
- 对我们可观察对象中的错误做出反应

## RxJS observables

RxJS 让你对数据的来源有更多控制。在本节中，我们将学习 RxJS 可观察对象与 bacon.js EventStreams 和 Properties 之间的差异。我们还将学习可观察对象的不同变体，以及如何更好地控制其生命周期。

## Difference between bacon.js and RxJS observables

在第 2 章 *Reacting for the First Time* 中，你了解到可观察对象基本上是对可能异步数据的抽象。可观察对象赋予你使用不同操作符转换数据并在新数据可用时采取行动的能力，使用订阅者。`bacon.js` 库使用术语 **subscriber** 来指代监听传入数据的对象，但在 Reactive Extensions 中，我们将使用不同的术语；我们称之为 Observer。

概念上，两者没有区别。基本上，这只是两个库的开发者选择的名称；然而，了解这两个名称很重要，因为你在库的文档中会看到两者都被使用。

在 bacon.js 中，我们看到了两种不同的可观察对象变体：

- EventStreams
- Properties

在 RxJS 中，我们将始终讨论：

- Observables
- Observers

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_65_0.png)

在本章中，你还将了解 RxJS 中的三个特殊对象。它们被称为 Subject、Disposable 和 Scheduler。

## Hot and cold observables

在 bacon.js 中，可观察对象只有在订阅者订阅时才会发出值；因此，我们称之为冷可观察对象。在 Reactive Extensions 中，情况会变得有点棘手：我们同时拥有热可观察对象和冷可观察对象。

热可观察对象是在是否有关联的 Observer 的情况下独立传播数据的可观察对象。热可观察对象的示例是从鼠标移动创建的可观察对象。

另一方面，冷可观察对象是为所有订阅者触发相同序列的可观察对象。冷可观察对象的示例是从数组创建的可观察对象。

理解这种行为很重要，RxJS 提供了特殊的方法来在热可观察对象中重放事件或将冷可观察对象转换为热可观察对象。

## Installing RxJS

RxJS 被划分为模块。这样，你就可以只使用感兴趣的模块创建自己的 bundle。在本书中，我们将始终使用包含 RxJS 所有内容的官方 bundle；这样做，我们就不必担心某个模块是否存在于我们的 bundle 中。所以，让我们按照这里描述的步骤安装 RxJS。

要在服务器上安装它，只需在 node 项目中运行以下命令：

```
npm i rx@4.1.0 --save
```

要将其添加到 HTML 页面，只需在 HTML 中粘贴以下代码片段：

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/rxjs/4.1.0/rx.all.js">
</script>
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_66_0.png)

对于使用其他包管理器的用户，你可以从 Bower 或 NuGet 安装 RxJS。

如果你在 node 程序中运行，则需要在每个要使用的 JavaScript 文件中都有 RxJS 库。为此，在 JavaScript 文件开头添加以下行：

```
var Rx = require('rx');
```

在所有示例中将省略前一行，因为我们期望你在测试示例代码前已添加它。

## Creating an observable

这里我们将看到从常见事件源创建可观察对象的一些方法列表。这不是一个详尽的列表，但包含了最重要的内容。

## Creating an observable from iterable objects

你可以在 RxJS GitHub 页面上查看所有可用方法 (https://github.com/Reactive-Extensions/RxJS)。

我们可以使用 from() 方法从可迭代对象创建可观察对象。JavaScript 中的可迭代对象可以是数组（或类数组对象）或 ES6 中添加的其他可迭代对象（如 Set() 和 Map()）。from() 方法的签名如下：

```
Rx.Observable.from(iterable, [mapFunction], [context], [scheduler]);
```

通常，你只会传递第一个参数。其他参数是可选的；你可以在这里看到它们：

- iterable: 这是要转换为可观察对象的可迭代对象（可以是数组、集合、映射等）
- mapFunction: 这是一个为数组中的每个元素调用的函数，将其映射到不同的值
- context: 当提供 mapFunction 时使用此对象
- scheduler: 用于迭代输入序列

如果不知道调度器是什么也不用担心。我们将看到它如何改变我们的可观察对象，但我们将在本章后面讨论它。现在，只关注此函数的其他参数。

现在让我们看看如何从可迭代对象创建可观察对象的一些示例。

要从数组创建可观察对象，可以使用以下代码：

```
Rx.Observable
  .from([0,1,2])
  .subscribe((a)=>console.log(a));
```

此代码打印以下输出：

0
1
2

现在让我们在代码中做一个小的改动，添加 mapFunction 参数，而不是创建可观察对象来传播此数组的元素。让我们使用 mapFunction 来传播以下数组中每个元素的两倍：

```
Rx.Observable
  .from([0,1,2],  (a) => a*2)
  .subscribe((a)=>console.log(a));
```

这打印以下输出：

```
0
2
4
```

我们也可以使用此方法从 arguments 对象创建可观察对象。为此，我们需要在函数中运行 from()。这样，我们就可以访问该函数的 arguments 对象。我们可以用以下代码实现它：

```
var observableFromArgumentsFactory = function(){
  return Rx.Observable.from(arguments);
};
observableFromArgumentsFactory(0,1,2)
  .subscribe((a)=>console.log(a));
```

如果我们运行此代码，将看到以下输出：

```
0
1
2
```

此方法的最后一种用法是从 Set() 或 Map() 创建可观察对象。这些数据结构是在 ES6 中添加的。我们可以按如下方式为集合实现它：

```
var set = new Set([0,1,2]);
Rx.Observable
  .from(set)
  .subscribe((a)=>console.log(a));
```

此代码打印以下输出：

```
0
1
2
```

我们也可以将一个 Map 作为 from() 方法的参数，如下所示：

```
var map = new Map([['key0',0],['key1',1],['key2',2]]);
Rx.Observable
  .from(map)
  .subscribe((a)=>console.log(a));
```

这将打印该 Map 上的所有键值元组：

```
[ 'key0', 0 ]
[ 'key1', 1 ]
[ 'key2', 2 ]
```

通过此方法创建的所有 Observable 都是冷 Observable。正如之前所讨论的，这意味着它为所有观察者触发相同的序列。要测试此行为，创建一个 Observable 并向其添加一个观察者；在 1 秒后向其添加另一个观察者：

```
var observable = Rx.Observable.from([0,1,2]);

observable.subscribe((a)=>console.log('first subscriber receives => '+a));

setTimeout(()=>{
    observable.subscribe((a)=>console.log('second subscriber receives => '+a));
},1000);
```

如果运行此代码，你将在控制台中看到以下输出，显示两个订阅者按预期接收相同的数据：

```
first subscriber receives => 0
first subscriber receives => 1
first subscriber receives => 2
second subscriber receives => 0
second subscriber receives => 1
second subscriber receives => 2
```

## 从序列工厂创建 Observable

现在我们已经讨论了如何从序列创建 Observable，让我们看看如何从序列工厂创建 Observable。RxJS 有一个内置方法 called generate()，它可以让你从迭代（如一个 for() 循环）创建 Observable。此方法具有以下签名：

```
Rx.Observable.generate(initialState, conditionFunction, iterationFunction, resultFactory, [scheduler]);
```

在此方法中，唯一可选的参数是最后一个参数。所有参数的简要说明如下：

- `initialState`: 这可以是任何对象，它是迭代中使用的第一个对象
- `conditionFunction`: 这是一个用于停止迭代条件的函数
- `iterationFunction`: 这是在每个元素上用于迭代的函数
- `resultFactory`: 这是一个函数，其返回值会传递给序列
- `scheduler`: 这是一个可选的调度器

在查看此方法的示例代码之前，让我们看看实现程序中最基本构造之一的代码：一个 `for()` 循环。这用于从初始值生成到最终值的数组。我们可以用以下代码生成此数组：

```
var resultArray=[];
for(var i=0;i < 3;i++){
    resultArray.push(i)
}
console.log(resultArray);
```

此代码打印以下输出：

```
[0,1,2]
```

当你创建一个 `for()` 循环时，你基本上向它提供以下内容：初始状态（第一个参数）、停止迭代的条件（第二个参数）、如何迭代值（第三个参数）以及对值做什么（代码块）。它的用法与 `generate()` 方法非常相似。让我们做同样的事情，但使用 `generate()` 方法并创建 Observable 而不是数组：

```
Rx.Observable.generate(
    0,
    (i) => i<3,
    (i) => i+1,
    (i) => i
).subscribe((i) => console.log(i));
```

此代码将打印以下输出：

```
0
1
2
```

## 使用 range () 创建 Observable

另一个 Observable 的常见数据源是范围。通过 range() 方法，我们可以轻松地为范围内的值序列创建 Observable。range() 方法具有以下签名：

```
Rx.Observable.range(first, count, [scheduler]);
```

下列列表中的最后一个参数是此方法中唯一可选的参数：

- `first`: 这是序列中的初始整数值
- `count`: 这是从序列开始迭代的连续整数的数量
- `scheduler`: 这用于生成值

我们可以用以下代码使用范围创建 Observable：

```
Rx.Observable
  .range(0, 4)
  .subscribe((i)=>console.log(i));
```

这会打印以下输出：

```
0
1
2
3
```

## 使用时间段创建 Observable

在上一章中，我们讨论了如何在 bacon.js 中创建定时序列。在 RxJS 中，我们有两种不同的方法来实现以给定间隔发出值的 Observable。第一个方法是 interval()。此方法每 x 毫秒发出一次从 1 开始的无限整数序列；它具有以下签名：

```
Rx.Observable.interval(interval, [scheduler]);
```

interval 参数是强制性的，第二个参数是可选的：

- `interval`: 这是作为此序列值之间间隔使用的整数
- `scheduler`: 这用于生成值

运行以下代码：

```
Rx.Observable
  .interval(1000)
  .subscribe((i)=> console.log(i));
```

你将看到如下输出；你必须停止程序（按 Ctrl+C），否则它会一直发送事件：

0
1
2

interval() 方法在给定的间隔期后发送序列的第一个值，并在每个间隔后继续发送值。

RxJS 还有一个名为 timer() 的方法。此方法让你指定开始序列的到期时间，甚至生成在到期时间后发出仅一个值的 Observable。它具有以下签名：

```
Rx.Observable.timer(dueTime, [interval], [scheduler]);
```

以下是参数：

- dueTime: 这可以是日期对象或整数。如果是日期对象，则表示它是开始序列的绝对时间；如果是整数，则指定在发送序列的第一个元素之前等待的毫秒数。
- interval: 这是表示元素之间时间的整数。如果未指定，则只生成一个事件。
- scheduler: 这用于生成值。

我们可以用以下代码从 timer() 方法创建 Observable：

```
Rx.Observable
  .timer(1000,500)
  .subscribe((i)=> console.log(i));
```

你将看到类似以下的输出；你必须停止程序，否则它会一直发送事件：

0
1
2

我们也可以使用此方法仅生成一个值并结束序列。我们可以省略 interval 参数来实现，如以下代码所示：

```
Rx.Observable
.timer(1000)
.subscribe((i)=> console.log(i));
```

如果运行此代码，它只会在控制台打印 0 并结束。

## 从回调创建 Observable

在 JavaScript 中，我们有很多使用回调的 API 来让你控制应用程序的流程。我们可以使用函数式响应式编程更好地控制它。我们可以使用 Observable 来包装回调（遵循 Node.js 回调模式）。这样，我们可以重用和组合这些回调。

为此，我们可以使用 fromCallback() 方法。它具有以下签名：

```
Rx.Observable.fromCallback(func,[context],[selector]);
```

第一个参数是强制性的，接下来的两个是可选的。它们如下：

- func: 这是在完成时通常接收回调的函数
- context: 这是回调中使用的上下文
- selector: 这是一个函数，它接收回调的参数以生成一个由此 Observable 传播的单个项

在展示此方法的使用示例之前，让我们创建自己的接受回调函数的函数，如下所示：

```
var myAsyncComputation = function(name,callback){
  setTimeout(()=>{
    callback(null,'Finished computation for '+name);
  },100);
};
```

myAsyncComputation 变量保存一个接受回调函数的函数。它在 100 毫秒后调用此回调，传入成功消息：Finished computation for SOME_NAME。要测试我们的函数，我们可以按以下方式调用它：

```
myAsyncComputation('John Doe',(err,result)=>console.log(result));
```

这将打印 `Finished computation for John Doe` 消息。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_74_0.png)

请记住，回调可以接收两个参数：第一个是 `error`（如果发生任何错误），第二个是 `result`，表示函数所做的异步计算的结果。

现在让我们为我们的异步函数创建 Observable，如下所示：

```
var observableFromCallback =
Rx.Observable.fromCallback(myAsyncComputation);

observableFromCallback('John Doe')
    .subscribe((result)=> console.log(result));
```

如果运行此代码，你将在控制台看到以下消息被打印：

```
[ null, 'Finished computation for John Doe' ]
```

这是因为正如我们所讨论的，回调接收两个参数：第一个是错误（如果有），第二个是成功结果。如果我们不想传播这两个值，我们可以使用 `fromCallback()` 方法的 selector 参数以不同的方式映射响应：

```
var observableFromCallback = Rx.Observable.
    fromCallback(myAsyncComputation, null, (error,result)=>result);
observableFromCallback('John Doe')
    .subscribe((result)=> console.log(result));
```

现在我们添加了一个参数来省略错误，因此当我们运行此代码时，将看到以下输出：

```
Finished computation for John Doe
```

## 从 Promise 创建 Observable

Promise 是保存异步计算结果的对象。它们现在变得越来越常见。Promise 的优点之一是你可以将它们与其他 Promise 或同步值组合，为异步计算提供额外的抽象层。我们可以将 Promise 包装成 Observable。这使我们能够使用 RxJS 的操作符更强大地处理计算结果。



为此，我们可以使用内置方法 `fromPromise()`。该方法可以将任何 A+ Promise 转换为可观察对象。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_75_0.png)

Promises/A+ 是 Promise 的规范。JavaScript 中所有著名的 Promise 库都遵循此模式；标准 ES6 Promise 也是遵循相同的模式。

此方法具有以下签名：

```
Rx.Observable.fromPromise(promise);
```

它只接收一个参数：

- `promise`: 这是一个 Promises/A+ 对象或返回 A+ Promise 对象的工厂函数。

要从 Promise 创建可观察对象，请使用以下代码：

```
Rx.Observable
.fromPromise(Promise.resolve('Hello World'))
.subscribe((result)=> console.log(result));
```

这会打印以下输出：

```
Hello World
```

我们也可以使用此方法从返回 Promise 的工厂函数创建可观察对象，如以下代码所示：

```
var promiseFactory = () => Promise.resolve('Hello World')
Rx.Observable
.fromPromise(promiseFactory)
.subscribe((result)=> console.log(result));
```

运行此代码时，你将看到以下输出：

```
Hello World
```

## 创建空的可观察对象

有时，在组合多个可观察对象时，你可能需要创建一个空的可观察对象。你可以在 RxJS 中使用两种不同的方法来做到这一点。第一个是 `empty()` 方法；该方法只会触发 `onComplete` 事件。

以下代码创建一个完成且不传播任何值的可观察对象：

```
Rx.Observable.empty();
```

有时你可能需要一个不会发出任何值并且永远不会终止的可观察对象。要创建这种可观察对象，可以使用 never() 方法：

```
Rx.Observable.never();
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_76_0.png)

这两种方法更多用于组合或作为防止缺少可观察对象的后备方案。两者之间的唯一区别是只有第一个会终止。

## 从单个值创建可观察对象

在组合多个可观察对象或为测试目的模拟可观察对象时，你可能需要创建一个只发出一个值然后自行终止的可观察对象。RxJS 有两种方法来实现此行为：return() 方法和 just() 方法。它们的工作方式完全相同，并且具有以下签名：

```
Rx.Observable.return(value, [scheduler]);
Rx.Observable.just(value, [scheduler]);
```

第一个参数是强制性的，第二个是可选的：

- value: 这可以是任何对象；它是序列中要发出的值
- scheduler: 用于发出值

以下代码说明了此方法的示例：

```
Rx.Observable
  .just('Hello World')
  .subscribe((i)=> console.log(i));
```

此代码使用 just() 方法，但我们也可以通过简单更改使用 return() 方法：

```
Rx.Observable
  .return('Hello World')
  .subscribe((i)=> console.log(i));
```

如果运行这些代码行中的任何一个，你将在控制台中看到打印的 Hello World 消息。

## 从工厂函数创建可观察对象

RxJS 中有一个方法可以从返回可观察对象或可观察对象工厂函数的函数创建可观察对象；它称为 defer()。此方法的签名如下：

```
Rx.Observable.defer(factoryFunction);
```

它只接收一个参数，且此参数是强制性的：

- factoryFunction: 这是一个返回可观察对象的函数

以下代码展示了此方法的实现：

```
Rx.Observable
  .defer(function(){
    return Rx.Observable.just('Hello World');
  })
  .subscribe((data)=>console.log(data));
```

如果运行此代码，你将看到以下输出：

```
Hello World
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_77_0.png)

顾名思义，defer() 方法仅在 Observer 订阅它时才调用工厂函数。

## 从任意参数创建可观察对象

你已经学会了如何使用 from() 方法从序列创建可观察对象，但有一种方法可以从任意参数序列创建可观察对象。我们可以使用 of() 方法来做到这一点。

此方法创建一个可观察对象，发出传递给它的每个参数。

of() 方法具有以下签名：

```
Rx.Observable.of(...args);
```

args 前的三点表示任意数量的参数。

- …args: 这指的是任意数量的对象

以下代码展示了 of() 方法的实现：

```
Rx.Observable
  .of(0,1,2)
  .subscribe((i)=>console.log(i));
```

如果运行此代码，你将看到以下输出：

```
0
1
2
```

现在让我们看看如果对 of() 方法只使用一个参数会发生什么。

假设你运行以下代码：

```
Rx.Observable
  .of('Hello World')
  .subscribe((i)=>console.log(i));
```

这样做时，你将看到以下输出：

```
Hello World
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_78_0.png)

正如你所见，如果使用只有一个参数运行 of() 方法，它的工作方式与 just() 方法相同。

现在你可能想知道如果不带任何参数运行 of() 方法会发生什么。为了检查这一点，让我们运行以下代码：

```
Rx.Observable
  .of()
  .subscribe((i)=>console.log(i));
```

如果运行上述代码，你的程序将在控制台中没有任何输出的情况下结束。因此，如果不带任何参数运行 of() 方法，它将具有与 empty() 方法相同的行为。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_79_0.png)

请记住，empty() 方法创建一个没有任何元素并结束它的可观察对象，而 never() 方法创建一个永远不会结束的空可观察对象。因此，使用 of() 方法不带任何参数创建可观察对象与使用 empty() 方法创建可观察对象相同。

## 从错误创建可观察对象

我们可以使用可观察对象包装错误；这样，我们将拥有相关可观察对象的所有运算符和组合。

RxJS 为我们提供了两种包装错误的方法。两者以完全相同的方式工作，并且基本上是彼此的别名。

将错误包装到可观察对象的方法是 throw() 和 throwError()。我们还有一个名为 throwException() 的方法，但该方法已弃用，不应再使用。

两种方法具有相同的签名，如下所示：

```
Rx.Observable.throw(err, [scheduler]);
Rx.Observable.throwError(err, [scheduler]);
```

第一个方法是你要包装到可观察对象的错误，它是强制性的；第二个是可选的：

- err: 这是表示错误的任何对象
- scheduler: 用于生成可观察对象的序列

让我们看一个使用 throw() 方法的代码片段：

```
Rx.Observable
    .throw(new Error('AN ERROR HAPPENED'))
    .subscribe((data)=>console.log(data));
```

如果运行上述代码，你将在控制台中看到错误堆栈跟踪，你的程序将结束。

现在让我们对代码做一个小的改动。

RxJS 的 subscribe() 方法可以接收第二个函数来响应错误，因此让我们添加另一个函数来打印错误的<|message|>：

```
Rx.Observable
    .throw(new Error('AN ERROR HAPPENED'))
    .subscribe(
    (data)=>console.log(data),
    (err)=>{
        console.log('Running the subscription function for error');
        console.log(err.message)
    }
);
```

如果运行代码，你将在控制台中看到以下错误消息：

```
Running the subscription function for error
AN ERROR HAPPENED
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_80_0.png)

本章后面我们将看到 subscribe() 方法的可能参数。

在我们的示例中，我们为 throw() 方法传递了一个 Error 对象，但如果传递另一种类型的对象，例如数字会怎样？为了回答这个问题，让我们对代码做一个小的改动：

```
Rx.Observable
    .throw(1)
    .subscribe(
    (data)=>console.log(data),
    (err)=>{
        console.log('Running the subscription function for error');
        console.log(err.message)
    }
);
```

如果运行上述代码，你将看到以下输出：

```
Running the subscription function for error
undefined
```

因此正如你所见，对象是什么并不重要。你可以使用 throw() 方法将任何对象包装为错误。使用另一个对象来表示错误是一种反模式，因此请小心，始终将错误包装在 Error 对象中。



![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_81_0.png)

切勿在未将其包装在 Error 对象中时抛出字符串（或任何对象）。

## 从 DOM 事件（或 EventEmitter）创建可观察对象

在前端应用中，RxJS 最重要的用途之一是从用户输入（如鼠标点击、鼠标移动或按键）创建可观察对象。

如果存在这些库，RxJS 可以绑定到 DOM 元素、jQuery（或 Zepto.js）元素、Ember 元素或 Angular 元素。RxJS 会尝试自动检测这些库。

此方法也可以用于绑定到 Node.js EventEmitter。

`fromEvent()` 方法用于从 DOM 元素创建可观察对象。该方法具有以下签名：

```
Rx.Observable.fromEvent(element, eventName, [mapFunction]);
```

前两个参数是强制性的，最后一个是可选的：

- `element`：这表示要附加事件的 DOM 元素、jQuery 元素、Angular 元素、Ember 元素、NodeList 或 EventEmitter
- `eventName`：这是一个表示事件的字符串
- `mapFunction`：这是一个从 EventEmitter 获取参数并将其映射为单个值的函数

要使用 `fromEvent()` 方法，请创建一个包含单个元素的 HTML 页面，如下所示：

```
<html>
<head></head>
<body>
<div id="myDiv">Foo</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/rxjs/4.1.0/rx.all.js"></script>
</body>
</html>
```

现在让我们在用户点击单词 Foo 时每次都显示一个警告。在第一个示例中，让我们从 DOM 元素创建一个可观察对象。

在 HTML 文件中添加一个 <script> 标签来监听 <div> 的点击：

```
Rx.Observable
  .fromEvent(document.getElementById('myDiv'),'click')
  .subscribe(function(e){
    alert('Clicked');
  });
```

在浏览器中打开此文件并点击单词 Foo；一旦点击它，警报将弹出在你的屏幕上。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_82_0.png)

请记住，在将此代码添加到 HTML 文件之前，你必须将 RxJS 作为脚本添加到你的页面。

现在让我们改用 jQuery 元素而不是 DOM 元素。将你的 <script> 标签更改为使用以下代码：

```
Rx.Observable
  .fromEvent($('#myDiv'),'click')
  .subscribe(function(e){
    alert('Clicked');
  });
```

在浏览器中打开此 HTML 文件并点击单词 Foo；一旦点击它，警报将弹出在你的屏幕上。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_82_1.png)

请记住，在将此代码添加到 HTML 文件之前，你必须将 jQuery 作为脚本添加到你的页面。

如你所见，你不需要任何更改就能让 RxJS 支持 jQuery 元素；它开箱即用。

我们也可以在 Node.js EventEmitters 上使用 `fromEvent()` 方法。为了说明这种用法，让我们使用 RxJS 读取文件并将其内容打印到控制台：

```
var fs = require('fs');
var Rx = require('rx');
var readStream = fs.createReadStream(__filename,'utf8');
Rx.Observable
    .fromEvent(readStream,'data')
    .subscribe((i)=> console.log(i));
```

在此示例中，我们首先导入 fs 模块来读取文件和 RxJS 模块。然后，我们从自己的文件创建 readStream（`__filename` 变量保存当前文件的位置），并添加字符串 utf8 以将文件作为字符串读取（如果省略此项，它将作为字节读取文件）。最后，我们使用 `fromEvent()` 方法从此流创建可观察对象，监听所有类型为 data 的事件。我们需要订阅可观察对象以便在控制台中打印内容。

如果你运行前面的代码，你将在控制台中看到自己的程序被打印出来。

现在让我们对代码做一个小的更改。如果我们在 `createReadStream()` 方法中删除 utf8 字符串并重新执行程序，我们将看到如下输出：

```
<Buffer 76 61 72 20 66 73 20 3d 20 72 65 71 75 69 72 65 28 27 66 73
27 29 3b 0a 76 61 72 20 52 78 20 3d 20 72 65 71 75 69 72 65 28 27
72 78 27 29 3b 0a 0a 76 ... >
```

我们已经知道可以添加 utf8 字符串作为参数，使 `createReadStream()` 方法将其转换为字符串。

如果没有，我们可以使用 RxJS。

在 JavaScript 中，buffer 对象有一个名为 `toString()` 的方法，可使用给定的字符编码将其转换为字符串。因此，如果我想将 buffer 转换为 utf8 字符串，我只需按如下方式编写：

```
var myString = myBuffer.toString('utf8');
```

`fromEvent()` 方法可以接收第三个参数，在将其传播给监听器之前映射 EventEmitter 的输入到另一个值。

我们可以使用此方法避免将 readStream 标记为 utf8 字符串，并且只能在可观察对象中执行此操作。为此，请使用以下代码：

```
var fs = require('fs');
var Rx = require('rx');

var readStream = fs.createReadStream(__filename);
Rx.Observable
    .fromEvent(readStream,'data',(chunk)=>chunk.toString('utf8'))
    .subscribe((i)=> console.log(i));
```

## 从任意源创建可观察对象

在上一章中，我们讨论了如何使用 fromBinder 事件在 bacon.js 中从任意源实现可观察对象。在 RxJS 中，我们也可以使用 `create()` 方法从我们想要的任何源创建可观察对象。此方法还有一个别名，称为 `createWithDisposable`。

`create()` 方法具有以下签名：

```
Rx.Observable.create(sourceFunction);
```

此方法只接受一个参数且是强制性的：

- sourceFunction：这是一个接收能够向可观察对象推送数据的对象的函数

我们可以使用以下代码使用 `create()` 方法创建可观察对象：

```
Rx.Observable.create(function(source){
    source.onNext(0);
    source.onNext(1);
    source.onNext(2);
    source.onCompleted();
}).subscribe((i)=> console.log(i));
```

如果你运行此代码，你将看到以下输出：

0
1
2

我们也可以用它向可观察对象发送错误，如此代码所示：

```
Rx.Observable.create(function(source){
  source.onNext(0);
  source.onNext(1);
  source.onError(new Error('ops'));
  source.onNext(2);
  source.onCompleted();
}).subscribe(
  (i)=> console.log(i),
  (err)=> console.log('An error happened: '+err.message )
);
```

如果你运行此代码，你将看到以下输出：

0
1
An error happened: ops

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_85_0.png)

如你所见，如果出现错误，可观察对象将停止发送数据。在后面的章节中，我们将看到如何从错误中恢复。

有时在创建可观察对象时，你可能需要释放一些已分配的资源（如数据库连接或文件句柄）。为此，RxJS 有一种特殊的对象类型称为 Disposable。

`create()` 方法让你定义一个函数（或一个 Disposable 对象），当你不再想持有刚刚分配的资源时触发它。

为此，你所要做的就是返回一个函数（或一个 Disposable 对象）并调用订阅的 `dispose()` 方法，如以下代码所示：

```
var observable = Rx.Observable.create(function(source){
  source.onNext(0);
  source.onNext(1);
  source.onNext(2);
  source.onCompleted();
  return function(){
    console.log('dispose called: releasing connections or other resources');
    };
});
var subscription = observable.subscribe(
    (i)=> console.log(i)
);
subscription.dispose();
```

如果你运行此代码，你将看到以下输出：

```
0
1
2
dispose called: releasing connections or other resources
```

如所见，传递给 `create()` 方法的参数返回的函数在你对可观察对象的订阅调用 `dispose()` 方法时被调用。我们也可以返回一个 Disposable 对象而不是一个函数，以便在订阅的 `dispose()` 方法被调用时执行。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_86_0.png)

我们将在本章后面了解更多关于 Disposable 对象的知识。

## 订阅更改（观察者）

要监听可观察对象上的数据，我们必须调用 `subscribe()` 方法。此方法返回一个订阅，我们稍后可以使用它来停止对传入数据的反应，如果我们不再感兴趣的话。

`subscribe()` 方法的可观察对象具有以下签名：

```
observable.subscribe(onNext,onError,onCompleted);
```

所有参数都是可选的，如果我们不感兴趣，可以省略此类型事件：

- onNext：这是一个每次有新数据通过可观察对象传播时调用的函数
- onError：这是一个每次可观察对象发生错误时调用的函数
- onCompleted：这是一个在可观察对象完成时调用的函数




最简单的方式订阅一个 Observable 就是只传递 onNext 参数（正如我们在本章的大多数代码片段中所做的那样）：

```
Rx.Observable
.just('Hello World!!!')
.subscribe((message)=>console.log(message));
```

如果运行上述代码，它将在你的控制台中打印 Hello World!!! 消息。

我们还可以添加一个 onError 函数，在发生错误时调用：

```
Rx.Observable
.throw(new Error('ops'))
.subscribe(
    (message)=>console.log(message),
    (err)=>console.log('An error happened: '+err.message)
);
```

如果运行此代码，你将在控制台中看到以下输出：

```
An error happened: ops
```

最后，我们还可以添加 onCompleted() 函数，以便在 Observable 完成时执行操作：

```
Rx.Observable
.just('Hello World!!!')
.subscribe(
    (message)=>console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);
```

如果运行此代码，你将看到以下输出：

```
Hello World!!!
END
```

所有订阅都有一个名为 dispose() 的方法。此方法可用于停止监听 Observable 中的传入数据。我们可以在以下代码中测试它：

```
var subscription = Rx.Observable
    .interval(100)
    .subscribe(
        (message)=>console.log(message),
        (err)=>console.log('An error happened: '+err.message),
        ()=>console.log('END')
    );

setTimeout( () => subscription.dispose(), 290 );
```

如代码所示，我们首先创建一个每 100 毫秒发出一次数据的 Observable 并订阅它。然后我们使用 `setTimeout()` 函数调用此订阅的 `dispose()` 方法并停止监听原始 Observable 的数据。

如果运行此代码，你将在控制台中看到以下输出：

```
0
1
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_88_0.png)

请记住，`interval()` 方法会在发送数字 0 之前等待 100 毫秒。然后，它会再等待 100 毫秒发送数字 1。

在 RxJS 中，我们还有一种名为 Observer 的特殊对象类。基本上，此对象封装了用于订阅 Observable 的 `onNext()`、`onError()` 和 `onCompleted()` 函数，为你提供额外的抽象层。

要创建观察者，我们可以使用 `create()` 方法，然后使用此观察者订阅 Observable，如以下代码所示：

```
var observer = Rx.Observer.create(
    (message) => console.log(message),
    (err) => console.log('An error happened: ' + err.message),
    () => console.log('END')
);

Rx.Observable
    .just('Hello World!!!')
    .subscribe(observer);
```

如果运行此代码，你将看到以下输出：

```
Hello World!!!
END
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_88_1.png)

Observer 是负责对 Observable 发送的数据做出反应的对象。

## RxJS 主题

Subject 既可以是 Observable 也可以是 Observer。它们可以被视为可推送的 Observable；它们允许你添加更多要通过它们传播的数据。

Subject 暴露了三个重要方法：onNext()、onError() 和 onCompleted()。这些方法可用于通过可观察序列发送事件。你可以在以下示例中看到它们的使用：

```js
var subject = new Rx.Subject();

subject.subscribe(
    (message)=> console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);

subject.onNext('Hello World!!!');
subject.onCompleted();
```

在此示例中，我们使用 subscribe() 方法创建了一个新的 Subject 并订阅它。然后，我们使用 onNext() 方法向此 Subject 推送数据，最后通过调用 onCompleted() 方法完成它。

如果运行此代码，你将看到以下输出：

```
Hello World!!!
END
```

我们还可以使用其 onError() 方法通过 Subject 传播错误：

```js
var subject = new Rx.Subject();

subject.subscribe(
    (message)=> console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);

subject.onNext('Hello World!!!');
subject.onError(new Error('ops'));
```

首先，我们传播值 Hello World，它将被打印到控制台。然后，我们传播一个错误。如果运行上述代码，你将在控制台中看到以下输出：

```
Hello World!!!
An error happened: ops
```

还有另一种名为 AsyncSubject () 的 Subject 类型。它可用于表示异步操作的结果。AsyncSubject () 只能接收一个值，并且该值随后会被缓存供此 Subject 的所有未来订阅使用。

AsyncSubject () 创建的示例可在以下代码中看到：

```
var subject = new Rx.AsyncSubject();

subject.subscribe(
  (message)=> console.log(message),
  (err)=>console.log('An error happened: '+err.message),
  ()=>console.log('END')
);

subject.onNext('Hello World!!!');
subject.onCompleted();
```

如你所见，与 Subject 相比创建它的唯一区别出现在代码的第一行，我们在其中使用 new Rx.AsyncSubject () 而不是 new Rx.Subject ()。如果运行此代码，你将在控制台中看到以下输出：

```
Hello World!!!
END
```

如前所述，AsyncSubject () 必须仅用于传播一个数据片段。因此，让我们将其与常规 Subject 进行比较。

首先，让我们看看当我们多次调用 onNext () 方法时 Subject 的输出会是什么：

```
var subject = new Rx.Subject();

subject.subscribe(
  (message)=> console.log(message),
  (err)=>console.log('An error happened: '+err.message),
  ()=>console.log('END')
);

subject.onNext(0);
subject.onNext(1);
subject.onNext(2);
subject.onCompleted();
```

如果运行此代码，控制台将打印以下内容：

```
0
1
2
END
```

这表明 Observer 被触发了三次（对应元素 0、1 和 2）。现在让我们更改代码以使用 AsyncSubject()：

```
var subject = new Rx.AsyncSubject();

subject.subscribe(
    (message)=> console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);

subject.onNext(0);
subject.onNext(1);
subject.onNext(2);
subject.onCompleted();
```

如果在运行此代码后查看控制台，我们将看到以下打印内容：

```
2
END
```

正如预期的那样，AsyncSubject() 只传播一个元素，因此它只触发一次 Observer（有两个元素）。

最后，让我们看看如果我们在 Subject 中不调用 onCompleted() 方法会发生什么。

首先，让我们使用常规 Subject：

```
var subject = new Rx.Subject();

subject.subscribe(
    (message)=> console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);

subject.onNext (0);
subject.onNext (1);
subject.onNext (2);
```

如果运行此代码，我们将看到以下输出：

0
1
2

我们可以看到 Subject 不会等待调用 onCompleted() 方法来传播元素。

现在让我们查看 AsyncSubject() 的输出：

```
var subject = new Rx.AsyncSubject();

subject.subscribe(
    (message)=> console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);

subject.onNext (0);
subject.onNext (1);
subject.onNext (2);
```

如果运行此代码，控制台中不会打印任何内容，因此 AsyncSubject() 会等待调用 onCompleted() 方法后才能开始传播元素。因此，很容易看出在使用 AsyncSubject() 时，我们必须始终一起调用 onNext() 和 onCompleted() 方法。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_92_0.png)

理解 Subject 的最简单方式是将其视为可推送的 Observable。

## RxJS 可释放对象

Disposable 类为我们提供了一种释放已分配资源（数据库连接、文件句柄等）的方法。我们可以通过调用此对象的 dispose() 方法来实现。



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

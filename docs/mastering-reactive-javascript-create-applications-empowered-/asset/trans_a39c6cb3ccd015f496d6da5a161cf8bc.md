
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

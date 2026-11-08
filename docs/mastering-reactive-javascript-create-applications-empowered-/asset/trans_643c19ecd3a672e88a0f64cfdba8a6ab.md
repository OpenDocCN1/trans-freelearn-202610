
正如我们所期望的，输出如下：

```
John Doe
Mary Jane
```

最后，我们可以用相同的代码与其他形式的迭代对象，如 Set（）：

```
var namesObservable = Rx.Observable.of('John', 'Mary');

var lastNamesArray = new Set(['Doe', 'Jane']);

namesObservable
.zipIterable(
  lastNamesArray,
  (name,lastName) => name + ' ' + lastName
).subscribe((k)=>console.log(k));
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_165_0.png)

zipIterable() 操作符与 zip() 操作符类似。它只是让可观察对象和普通迭代对象之间的互操作变得更容易。

## 摘要

在本章中，我们重点关注了组合可观察对象的优势；我们了解到，可以使用它们并行运行异步任务，在监听多个源时避免代码重复，并提高代码的重用性和可读性。

我们还学习了不同用于组合和统一可观察对象的操作符。最常用且使用最广泛的操作符是 concat()、merge() 和 zip()。

但我们也学习了一些其他用于组合可观察对象的操作符，因为它们为我们提供了更多互操作代码的工具。

到目前为止，我们只是忽略了可观察对象可能传播的错误。在下一章中，重点将放在处理这些错误以及为我们的函数式响应式应用程序创建测试上。因此，我们将学习在可观察对象中检测错误、处理可观察对象中的错误、模拟可观察对象以及测试函数式响应式程序。

## 7

## 有问题 - 测试和处理错误

在上一章中，我们学习了提高代码重用性和可读性的强大工具。这个工具是通过组合可观察对象来创建新的数据源。我们还学习了如何并行运行多个异步可观察对象。

我们在上一章中学到的最重要的操作符是：

- `concat()`
- `merge()`
- `forkJoin()`
- `zip()`

使用这些操作符可以帮助我们避免代码重复，给出了可读代码的一个非常重要的座右铭：**不要重复自己 (DRY)**。

在本章中，我们将重点关注两个非常重要的议题，不仅对于使用函数式响应式编程实现的程序，而且对于所有程序都适用：

- 处理错误
- 测试我们的代码
  - 我们将看到当错误发生时可观察对象会发生什么，以及我们可以做什么来处理此错误并确保我们的代码按预期继续运行。

- 另一方面，函数式响应式编程最酷的事情之一就是能够将数据源与要执行的操作解耦。这使得我们的代码更具可测试性，因为我们可以模拟其中的任何部分。
- 在本章中，我们将学习如何在浏览器和 Node.js 上测试程序。对于那些对测试 JavaScript 代码没有任何熟悉的人，我们将快速浏览一些用于此目的的框架。
- 随着本书的推进，主要思路是使我们的示例更复杂。我们将在测试部分看到这一发展，因此请现在花时间回顾前面的章节，或在您遇到不熟悉的操作符时随时回顾。

### 处理错误

在异步代码中检测和处理错误是困难的任务。对于同步代码，我们可以使用著名的 try/catch/finally 块。使用此块处理异常很容易，如以下代码所示：

```
try{
    throw new Error('An error occurred');
} catch(err) {
    console.log('Treating the error');
} finally{
    console.log('Last computation');
}
```

不幸的是，我们不能在异步代码上使用 try/catch/finally。在 JavaScript 中我们大部分时间都在运行异步代码，因此语言和社区制定了一些策略。

最初，处理异步代码的唯一可用策略是使用回调函数，因此 JavaScript 中的大多数库都添加了 onSuccess() 和 onError() 方法，它们接受一个在发生错误或数据可用时执行的回调函数。不同的框架使用不同的方法来实现这一点，例如，在 jQuery 中是 ajaxError() 和 ajaxSuccess()。Node.js 社区看到了标准化这种行为的必要性（因为这在 JavaScript 中非常常见），因此创建了 Node.js 回调标准。

该标准强制使用单个函数，该函数接收异步计算的两个参数：第一个是错误对象（如果发生错误），第二个是成功结果（如果成功）。来自 Node 的回调标准仍然可以在核心函数和多个库中看到。此标准的示例可在 fs 模块中使用，该模块是 Node 与文件系统交互的模块：

```
var fs = require('fs');

fs.readFile('this_file_doesnt_exist.txt',function(e,data){
  if(e){
    console.log('An error happened');
    return;
  }
  console.log(data);
});
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_169_0.png)

如果您之前有 Node.js 的经验，您已经熟悉这种模式。

正如您可能知道的那样，使用回调会导致一个称为回调地狱的问题，当您有多个回调彼此嵌套时就会发生这种情况，这使得代码难以理解。因此，创建了 Promise 对象，其中包含一个用于处理错误的 catch() 方法：

```
new Promise(function(resolve,reject){
  reject(new Error('An error occurred'));
}).catch(function(err){
  console.log('Treating the error');
});
```

在此代码中，我们有一个失败的 Promise，其中错误使用 catch() 方法处理。如您所见，这些选项都不像标准的 try/catch/finally 那样简单，但这种解决方案目前还不适用于异步代码，因此我们必须使用其中一种可用策略。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_169_1.png)

在即将到来的 JavaScript 版本中，我们将拥有 async/await 功能，这将让我们再次使用 try/catch/finally 块。

在函数式响应式编程中，我们有不同的策略来处理错误，我们将在本节中看到。

### 错误发生时采取行动

当错误发生时，可观察对象停止发送新数据。我们可以使用不同的策略，例如切换到另一个可观察对象或重试，但这些更高级，因此首先我们将看到观察者如何被错误通知。正如您可能记得的，可观察对象中的 subscribe() 方法允许您传递最多三个函数，或允许观察者在发生错误时收到通知。

请看以下签名：

```
observable.subscribe([onNext], [onError], [onCompleted]);
```

参数是可选的：

- onNext：当此可观察对象中有新数据可用时调用此函数。该函数接收数据本身作为参数。
- onError：当错误通过此可观察对象传播时调用此函数。该函数接收 Error 作为参数。
- onCompleted：当可观察对象耗尽并且所有数据都已传播时调用此函数。

此方法还具有以下签名的重载实现：

```
observable.subscribe(observer);
```

它接受以下单个参数：

- observer：当有数据可用、发生错误或可观察对象耗尽时，观察者对象会收到通知。观察者对这些可能的每个事件都有特定的方法。

因此，我们有两种可能的方式来接收可观察对象中错误的通知；第一种如下：

```
Rx.Observable
.throw(new Error('An error occurred'))
.subscribe(
  (data)=> console.log(data),
  (e)=> console.log('ERROR'),
  ()=>console.log('finished')
);
```

第二种，是显式使用观察者对象的实现：

```
var observer = Rx.Observer
.create(
  (data)=> console.log(data),
  (e)=> console.log('ERROR'),
  ()=>console.log('finished')
);
Rx.Observable
.throw(new Error('An error occurred'))
.subscribe(observer);
```

这两种实现是等价的，如果运行其中任何一个代码，您将看到以下输出：

```
ERROR
```

正如您所见，当错误传播到观察者时，onCompleted() 函数不会被调用。

正如我们已经讨论过的，当错误发生时，可观察对象会停止。我们可以在以下代码中看到这种行为：

```
var observable = Rx.Observable.create(function(source){
  source.onNext(1);

  source.onError(new Error('An error occurred'));

  source.onNext(2);
  source.onCompleted();
});

observable.subscribe(
  (data)=> console.log(data),
  (e)=> console.log('ERROR'),
  ()=>console.log('finished')
);
```

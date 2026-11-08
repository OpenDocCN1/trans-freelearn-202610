
![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_31_0.png)

这里我们从一个间隔创建 EventStream。bacon.js 有很多内置方法来创建事件流；我们将在第 2 章《首次响应》中更详细地介绍这些方法。

恭喜，你刚刚使用函数式响应式编程创建了你的第一个程序。现在，让我们稍微修改一下。

我们将实现相同的代码，但使用一个 frp 运算符。为此我们将使用 map () 运算符——该运算符接收一个映射函数作为参数。该函数接收一个输入值并将其转换为另一个值。与其监听流上的事件并在控制台打印当前日期，不如监听流中的事件，将这些事件映射到当前日期，然后打印结果。创建一个名为 baconIntervalMap.js 的文件并粘贴以下代码：

```javascript
var Bacon = require("baconjs");
Bacon
    .interval(1000)
    .map(()=>new Date())
    .onValue((currentDate)=>console.log(currentDate));
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_31_1.png)

使用 map () 运算符我们可以编写更具描述性的代码。这也使我们能够解耦代码并测试每个函数，而不是整个代码。我们将在第 4 章《转换数据 - Map、Filter 和 Reduce》中更详细地介绍 map () 运算符。

如果运行此代码，你将看到与之前代码相同的输出。

在第一个示例（没有 bacon.js）中，我们有一个难以测试的硬编码，因为代码的所有逻辑都紧密结合在一起。在下一个示例中，我们改进了代码的可测试性，因为我们将事件源与操作分离，因此我们可以通过模拟 EventStream 来测试我们的 onValue() 函数，但获取当前日期的逻辑仍然与操作（在控制台打印）绑定在一起。最后一个示例（使用 map()）的可测试性高得多，因为我们可以单独测试代码的每个部分。

让我们看看我们问题的最后一个版本。与其每秒永远打印当前日期，我们将每秒只打印当前日期五次。我们可以在不使用 frp 的情况下实现它；创建一个名为 traditionalInterval5times.js 的文件并粘贴以下代码：

```javascript
var count = 0;

var intervalId = setInterval(()=>{
    console.log(new Date());
    count++;
    if(count===5){
        clearInterval(intervalId);
    }
},1000);
```

现在我们的代码变得复杂得多。要实现所提议代码的新版本，我们需要一个外部计数器（以确保我们只运行代码五次），并且我们需要间隔调度程序的引用，以便以后可以停止它。通过查看此代码，很难理解它试图做什么，而且正如你可能已经注意到的那样，它真的很难测试。

你可能想知道我们如何更改上一个 frp 代码来实现；如果我们有一种方法只能监听 Bacon 的 EventStream 上的五个事件，那就太好了，所以我向你介绍 take() 方法。此方法让你只能监听 EventStream 上的给定数量的事件。因此，将我们之前的代码更改为只运行五次非常简单；创建一个名为 baconIntervalMap5times.js 的文件并粘贴以下代码：

```javascript
var Bacon = require("baconjs");

Bacon
    .interval(1000)
    .take(5)
    .map(()=>new Date())
    .onValue((currentDate)=>console.log(currentDate));
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_32_0.png)

take() 运算符让我们只能监听 EventStream 上的前 x 个事件。我们将在第 8 章《还有什么？ - 更多运算符》中更详细地介绍此运算符。

我们的新代码就是这么简单。我们不需要更改流源、映射函数或订阅函数；我们只需要从流中取出五个事件即可。

在这个最终示例中，我们可以看到函数式响应式编程如何改善我们的代码可读性。在使用 setInterval() 的代码中，我们必须添加一个计数器变量，以确保我们按正确的次数运行日志函数。我们还必须记住清除间隔，以免无限期运行此代码。另一方面，在使用 frp 的代码中，我们所要做的就是使用 take() 描述转换以显示我们想要多少项，使用 map() 获取当前日期，最后订阅以对该流的事件做出反应。

如果进展对你来说太快了，不用担心——我只是想让你用一些 `frp` 代码动手实践一下。在接下来的章节中，我们将以更多细节看到这些概念。

## Summary
## 摘要

In this chapter, we learned what functional reactive programming is and how it compares with imperative programming. We also learned where it comes from and modeled and implemented our first functional reactive program using the `bacon.js` library. We compared it with a program with and without `frp` and saw how it can improve our code maintainability and testability by separating the source of the events from the actions caused by them.
在本章中，我们学习了什么是函数式响应式编程以及它与命令式编程的比较。我们还学习了它的来源，并使用 `bacon.js` 库建模并实现了我们的第一个函数式响应式程序。我们将其与有和没有 `frp` 的程序进行了比较，并看到了它如何通过将事件源与由此引起的操作分离来改善代码的可维护性和可测试性。

In the next chapter, we will take a more in-depth look into the `bacon.js` library, and what we can do with event streams, and we will also start to model and implement more complex examples using new operators.
在下一章中，我们将更深入地研究 `bacon.js` 库，以及我们可以用事件流做什么，并且我们还将开始使用新运算符建模和实现更复杂的示例。

## 2
## 2
### Reacting for the First Time
### 首次响应

In the previous chapter, you started understanding the motivations behind using functional reactive programming in your systems; you also saw how a program using this paradigm fared against a program without it. You learned how reactive programming can improve code readability and testability by decoupling your event sources from the action you take when the action occurs.
在上一章中，你开始理解在系统中使用函数式响应式编程背后的动机；你还看到了使用这种范式的程序与不使用它的程序相比如何。你学习了响应式编程如何通过将事件源与操作发生时采取的操作解耦来改善代码可读性和可测试性。

We started with some basic examples using `bacon.js` as the reactive programming library for JavaScript. In the examples, we began with creating our first EventStream from an interval. Then we started using some operators (`map()` and `take()`). Finally, we subscribed to this event source to take actions in the case of an event occurrence. This was just a kind introduction to functional reactive programming.
我们从使用 `bacon.js` 作为 JavaScript 的响应式编程库的一些基本示例开始。在示例中，我们首先从间隔创建第一个 EventStream。然后我们开始使用一些运算符（`map()` 和 `take()`）。最后，我们订阅了此事件源以在事件发生时采取行动。这只是对函数式响应式编程的友好介绍。

When reading most of the functional reactive programming libraries (for any language), you will see a lot of diagrams explaining how the operator works. In the previous chapter, I presented the following diagram to illustrate a **filter function**:
在阅读大多数函数式响应式编程库（适用于任何语言）时，你会看到很多解释运算符如何工作的图表。在上一章中，我展示了以下图表来说明一个 **过滤函数**：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_35_0.png)

We will cover what this illustration means in detail in this chapter. Learning how to read this will make reading documentation on libraries a lot easier.
我们将在本章中详细介绍此插图的意思。学习如何阅读它将使阅读库文档变得容易得多。

Functional reactive programming is a paradigm which is hard to master, so after the quick introduction, you'll now move on to learning how to implement more complex programs using the bacon.js library.
函数式响应式编程是一个难以掌握的范式，因此在快速介绍之后，你现在将继续学习如何使用 bacon.js 库实现更复杂的程序。

This chapter will cover the following points:
本章将涵盖以下几点：

- Understanding bacon.js observable objects (EventStream and Property) and their differences
- Modeling a functional reactive program
- Subscribing to an observable
- Unsubscribing from an observable
- Reading a reactive programming operator diagram
- Using operators to transform an observable
- 理解 bacon.js 可观察对象（EventStream 和 Property）及其差异
- 建模函数式响应式程序
- 订阅可观察对象
- 取消订阅可观察对象
- 阅读响应式编程运算符图
- 使用运算符转换可观察对象

### The bacon.js observables
### bacon.js 可观察对象

In functional reactive programming, an observable is an object where you can listen for events. This way, you can, for instance, create an observable for a button and then listen and act when a click happens.
在函数式响应式编程中，可观察对象是一个可以监听事件的对象。这样，你就可以例如为按钮创建一个可观察对象，然后在点击时监听并采取行动。

The bacon.js gives you two flavors of an observable: the first one is EventStream and the other is Property. We will see the difference between the two objects later. To listen to events in an observable (or subscribe to an observable), you can use the onValue() method with a callback. So if you want to log every event in an EventStream, you can use the following code:
bacon.js 为你提供了两种可观察对象的风格：第一个是 EventStream，另一个是 Property。我们稍后会看到这两个对象的区别。要监听可观察对象中的事件（或订阅可观察对象），你可以使用 onValue() 方法并带有一个回调。因此，如果你想记录 EventStream 中的每个事件，可以使用以下代码：

```
myEventStream.onValue(function(event){
  console.log(event);
});
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_36_0.png)

We will see how to create an EventStream in more detail later.
我们稍后将更详细地介绍如何创建 EventStream。

As we saw in the example in the last chapter, we can transform our observable using bacon.js operators. These operators let us filter, combine, map, buffer, and do a lot of other interesting things with our EventStream.
正如我们在上一章的示例中看到的，我们可以使用 bacon.js 运算符转换我们的可观察对象。这些运算符让我们可以过滤、组合、映射、缓冲，并对我们的 EventStream 执行许多其他有趣的操作。

An observable can either finish or stay open to propagate events forever; it can also contain (and propagate) errors. We will see this in more detail later.
可观察对象要么结束，要么保持打开状态以永远传播事件；它也可以包含（并传播）错误。我们稍后会更详细地介绍这一点。

### Creating our first observable
### 创建我们的第一个可观察对象

The bacon.js gives us a multitude of methods to create observables. We can create them from DOM events, promises, interval, and so on. Sometimes we might need to create an observable from our own source, so let's learn how to create our own event streams.
bacon.js 为我们提供了多种创建可观察对象的方法。我们可以从 DOM 事件、promise、间隔等创建它们。有时我们可能需要从自己的源创建可观察对象，因此让我们学习如何创建自己的事件流。

### Observables from DOM events (asEventStream)
### 来自 DOM 事件的可观察对象（asEventStream）

To create an EventStream from DOM events (a mouse click for instance), we will need an HTML page with jQuery (or Zepto.js); bacon.js adds the asEventStream() method for all jQuery objects. So if we want to create an EventStream from a button click, we can use the following code:
要从 DOM 事件（例如鼠标点击）创建 EventStream，我们需要一个带有 jQuery（或 Zepto.js）的 HTML 页面；bacon.js 为所有 jQuery 对象添加了 asEventStream() 方法。因此，如果我们想从按钮点击创建 EventStream，可以使用以下代码：

```
var clickEventStream = $('#myButton').asEventStream('click');
```

This line creates an EventStream from button clicks on a DOM object with the ID myButton. If we want to execute an action every time this button is clicked, we will need to use the onValue() method from this event stream. The following code shows an alert on the screen every time the user clicks on the button:
这一行从 ID 为 myButton 的 DOM 对象的按钮点击创建 EventStream。如果我们想在每次点击此按钮时执行一个操作，我们需要使用此事件流的 onValue() 方法。以下代码在用户每次点击按钮时在屏幕上显示一个警报：

```
clickEventStream.onValue(function(){
  alert('Button clicked');
});
```

This code adds a function to be called every time an event happens in this EventStream object, as it emits an event every time myButton is clicked. The following code will show an alert every time this button is clicked.
此代码添加了一个函数，每当此 EventStream 对象中发生事件时调用它，因为每次点击 myButton 时它都会发出一个事件。以下代码将在每次点击此按钮时显示一个警报。


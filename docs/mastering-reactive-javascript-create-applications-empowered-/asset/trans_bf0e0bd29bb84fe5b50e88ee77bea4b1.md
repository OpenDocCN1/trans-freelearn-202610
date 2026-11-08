
如果运行此代码，你将看到如下输出（值可能因程序的不同执行而有所变化）：

```
The average of the last five items is: 54.8
The average of the last five items is: 60.6
The average of the last five items is: 62.6
//... continues printing until you stop the program
```

## bufferWithTime() 操作符

下一个用于缓冲数据的操作符让你在一定时间内缓冲数据。它叫做 bufferWithTime()。此操作符会将数据存储在数组中，直到经过给定的时间。

bufferWithTime() 方法具有以下签名：

```
observable.bufferWithTime(time, [skipTime], [scheduler]);
```

第一个参数是必需的，另外两个是可选的：

- time: 这是缓冲数据的时间（以毫秒为单位），在传播之前
- skipTime: 这与 bufferWithCount() 中的 skip 参数类似，但它是跳过的毫秒时间
- scheduler: 用于传播数据

此方法的使用示例如下：

```
Rx.Observable
  .interval(100)
  .bufferWithTime(210)
  .subscribe((arr)=>console.log(arr));
```

它从 interval() 创建一个 Observable，将数据缓冲 1000 毫秒，然后使用 subscribe() 将其记录到控制台。

运行此代码，你将看到以下输出：

```
[ 0, 1 ]
[ 2, 3 ]
[ 4, 5 ]
//continues printing until you stop the program
```

此操作在以下图表中进行了说明：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_129_0.png)

现在，你可能会想知道如果时间已过而原始 observable 没有发送数据会发生什么；你有两个合理的猜测：

- 数据不会被传播
- 会传播一个空数组

为了测试它，让我们从 interval() 创建一个 Observable 并对其使用 filter()，以便只传播几个元素：

```
Rx.Observable
.interval(50)
.filter((i)=>i<10)
.bufferWithTime(150)
.subscribe((arr)=>console.log(arr));
```

如果运行此代码，我们将看到以下输出：

```
[ 0, 1 ]
[ 2, 3, 4 ]
[ 5, 6, 7 ]
[ 8, 9 ]
[]
[]
[]
//keeps printing empty array
```

因此，查看控制台中的输出，很容易看出正确答案：会传播一个空数组。

我们还可以将来自 `bufferWithCount()` 操作符的最后一个示例更改为，每秒计算最后 5 秒内数字的平均值。为此，我们所要做的就是使用操作符 `bufferWithTime()` 并调整 time 和 `skipTime` 参数：

```
var randomNumbersObservable = Rx.Observable
    .interval(100)
    .map((i)=>Math.floor(Math.random()*100));

randomNumbersObservable
    .bufferWithTime(5000,1000)
    .map((arr)=>{
    var sum = arr.reduce((acc,b)=>acc+b);
    return sum/arr.length;
    })
    .subscribe((i)=>console.log('The average of the last five second is: '+i));
```

正如你所见，我们将操作符 `bufferWithCount()` 改为使用 `bufferWithTime()`。我们将缓冲 5000 毫秒并使用 1000 毫秒的窗口，并且还更改了要在控制台中打印的消息，因为现在我们显示的是 `The average of the last five seconds` 而不是最后五个项目。

如果运行此代码，你将看到如下输出：

```
The average of the last five seconds is: 48.875
The average of the last five seconds is: 50.08163265306123
The average of the last five seconds is: 49.61224489795919
The average of the last five seconds is: 49.3125
//continues printing until you stop the program
```

## bufferWithTimeOrCount() 操作符

我们将看到的最后一个用于缓冲数据的操作符是 `bufferWithTimeOrCount()` 操作符。在最近的两次课程中，我们学习了 `bufferWithCount()` 操作符，它让我们可以对数组的大小设置限制，以及 `bufferWithTime()` 操作符，它让我们可以对填充缓冲区的時間设置限制。有时我们可能希望对两者都设置最大值。

`bufferWithTimeOrCount()` 方法具有以下签名：

```
observable.bufferWithTimeOrCount(time,count,[scheduler]);
```

前两个参数是必需的，最后一个是可选的：

- `time`: 这是以毫秒为单位 `buffer` 数据的时间，在传播之前
- `count`: 这是 `buffer` 的最大大小
- `scheduler`: 用于传播数据

我们可以从其他 `buffer` 操作符中使用相同的示例来查看此操作符的实际效果：

```
Rx.Observable
  .interval(100)
  .bufferWithTimeOrCount(1000,5)
  .subscribe((arr)=>console.log(arr));
```

在此示例中，我们将始终基于 count 进行限制（因为在 1000 毫秒的时间跨度内，我们在 `Observable` 上生成超过 5 个项目），因此运行此代码，我们将看到以下输出：

```
[ 0, 1, 2, 3, 4 ]
[ 5, 6, 7, 8, 9 ]
[ 10, 11, 12, 13, 14 ]
[ 15, 16, 17, 18, 19 ]
//continues printing until you stop the program
```

但如果我们使用较短的时间更改参数，例如 150 毫秒，它将在时间上达到上限：

```
Rx.Observable
  .interval(100)
  .bufferWithTimeOrCount(150,5)
  .subscribe((arr)=>console.log(arr));
```

以下输出将被打印：

```
[ 0 ]
[ 1, 2 ]
[ 3 ]
[ 4, 5 ]
//continues printing until you stop the program
```

这些是基本示例。此操作符的一个更酷的用途是在 DOM 元素中检测鼠标的三次点击。

在 Vanilla JS 中实现这种行为可能非常混乱，但使用 RxJS 则非常简单：

```
<html>
<head>
</head>
<body>
<span id="my_span">TRIPLE CLICK</span>
<ul id="my_ul"></ul>
<script src="https://cdnjs.cloudflare.com/ajax/libs/rxjs/4.1.0/rx.all.js"></script>
<script>
Rx.Observable
.fromEvent(document.getElementById("my_span"),"click")
.bufferWithTimeOrCount(1000,3)
.filter(function(arr){
return arr.length===3;
})
.subscribe(function(){
var node = document.createElement("li");
var textnode = document.createTextNode("TRIPLE CLICK DETECTED");
node.appendChild(textnode);
document.getElementById('my_ul').appendChild(node);
});
</script>
</body>
</html>
```

在此 HTML 页面中，我们有一个 span，我们将在其中监听三次点击。为此，我们使用 fromEvent() 方法从其点击事件创建一个 Observable，然后我们缓冲点击一秒钟或三次点击（以先发生者为准），因为我们想要检测三次点击。我们现在过滤 Observable，仅在数组大小等于 3 时传播（这就是我们进行三次点击的情况），最后我们订阅以在 HTML 页面上添加一个 li，显示该 span 被点击了三次（现在，尝试在没有函数式响应式编程的情况下实现相同的行为）。

如果你实现此示例（并在 span 中进行一些 TRIPLE CLICK），你将看到如下页面：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_133_0.png)

## 可暂停的 observable（带缓冲）

我们已经学习了如何以有损的方式暂停和恢复 observable；现在我们将看到一个操作符，用于在暂停时暂停 observable 并保留缓冲区，以便不会丢失任何数据。

用于暂停并保留缓冲区的操作符称为 pausableBuffered()，其签名如下：

```js
observable.pausableBuffered();
```

正如你所见，它不接受任何参数。

我们可以在以下代码中看到此操作符的使用示例：

```js
var pausableObservable = Rx.Observable
    .interval(500)
    .pausableBuffered();

pausableObservable.subscribe((i)=>console.log(i));

pausableObservable.pause();

setTimeout(()=>{
    pausableObservable.resume();
},4000);
```

在此示例代码中，我们创建一个 `Observable` 并在其上调用 `pausableBuffered()`。然后我们 `pause` 它并等待 4000 毫秒 `resume`。如果运行此代码，你将在控制台中看到以下输出：

```
0
1
2
3
4
//continues printing until you stop the program
```

查看输出，我们还可以注意到，即使被缓冲，数据在 `pausable` observable 上始终逐个传播。此方法还为程序员提供了完全控制何时停止和 `resume` observable 的能力。

## 受控 observable

我们将看到的最后一个无损的应对背压策略是 `controlled()` 操作符。此操作符为程序员提供了对缓冲区的完全控制，因为它允许你 `buffer` 所有数据并随时请求你想要的数据量。

此操作符具有以下签名：

```
observable.controlled();
```

正如你所见，此方法不接收任何参数。

创建 `controlled()` observable 后，你可以使用 `request()` 方法请求给定数量的项目。`request()` 方法具有以下签名：

```
controlledObservable.request(count);
```

参数 `count` 是必需的：

- `count`: 这是从 `Observable` 中提取的项目数（如果不存在，则默认为零）。

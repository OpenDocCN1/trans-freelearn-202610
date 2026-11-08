

这在下面的截图中有所说明：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_121_0.png)

## pausable() 可观察对象

实现有损反压处理技术的最后一个操作符是 pausable() 操作符。该操作符返回一个 pausable() 可观察对象，这样我们就可以暂停它（暂停期间丢弃所有数据），并在准备好接收更多数据时恢复它。

pausable() 方法的签名如下：

```
observable.pausable();
```

如你所见，它不接受任何参数。

我们可以在以下代码中看到此操作符的使用示例：

```
var pausableObservable = Rx.Observable
    .interval(50)
    .pausable();
pausableObservable.subscribe((i)=>console.log(i));

pausableObservable.pause();

setTimeout(()=>{
    pausableObservable.resume();
},2000);
```

如果运行此代码，你将在控制台看到以下输出：

```
0
1
2
3
//continues printing until you stop the program
```

在此示例中，我们创建了一个每 50 毫秒发出一次数据的 Observable，但在订阅后立即暂停它，因此没有数据发出。然后在 2000 毫秒后，一个函数被调用，我们恢复可观察对象，于是它开始打印数据。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_122_0.png)

此方法仅适用于热可观察对象。通过 interval() 方法创建的可观察对象是冷可观察对象，因此不会丢弃任何数据。

现在让我们在 subject 中检查此方法：

```
var subject = new Rx.Subject();
var pausableObservable = subject.pausable();

pausableObservable.subscribe(
  (i)=>console.log(i),
  (err)=>console.log(err),
  ()=>console.log('Finished')
);

pausableObservable.pause();

subject.onNext(0);
subject.onNext(1);

setTimeout(()=>{
  subject.onNext(2);
  subject.onNext(3);
  subject.onCompleted();
},200);
setTimeout(()=>{
  pausableObservable.resume();
},2000);
```

我们使用的是本节所有示例中使用的同一个 subject。唯一的变化是我们先暂停它，然后再推送任何数据，并且只在所有数据推送完毕后才恢复它，因此如果运行此代码，我们将在控制台看到以下打印内容：

```
Finished
```

所有数据都被丢弃，因为数据是在可观察对象暂停期间推送的。此操作在下面的图表中有所说明：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_123_0.png)

## 处理反压的无损操作符

处理反压的无损策略是我们无法承受数据丢失时必须使用的策略。如果你使用它们来显示网页聊天中的消息，你可能希望对它们进行缓冲。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_123_1.png)

使用无损策略缓解反压问题时要小心，因为这会增加程序所需的内存。

## 缓冲

处理反压的第一个无损技术是使用缓冲区存储数据，以便我们可以批量处理。

所有用于缓冲数据的操作方法名称都以 buffer 开头（这里我们将学习 bufferWithCount()、bufferWithTime() 和 bufferWithTimeOrCount()），基本上它们会将数据存储在内存中，直到缓冲区被填满并传播包含数据的数组。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_124_0.png)

如果可观察对象传播错误消息，则该消息将立即发送，而不会先发出缓冲区，即使缓冲区包含数据。

## bufferWithCount() 操作符

第一个用于缓冲数据的方法是 bufferWithCount()。bufferWithCount() 方法允许你指定缓冲区的大小。使用此方法，数据将存储在数组中，直到该数组的大小达到给定计数，然后此数组将被传播。

bufferWithCount() 方法的签名如下：

```
observable.bufferWithCount(size, [skip]);
```

第一个参数是必需的，第二个是可选的：

- size：这是一个指定缓冲区大小的数字。
- skip：这是一个指定创建下一个缓冲区时应跳过多少个项目的数字。如果未提供，则默认为 size（这样下一个缓冲区将只包含新元素）。

现在让我们看一个此方法的使用示例：

```
Rx.Observable
  .interval(50)
  .bufferWithCount(2)
  .subscribe((arr)=>console.log(arr));
```

在此示例中，我们首先创建一个间隔为 50 毫秒的 Observable。我们然后使用 bufferWithCount() 方法将其缓冲为 10 个一组，最后打印到控制台。

如果运行此代码，你将看到以下输出：

```
[ 0, 1]
[2, 3]
//continues printing until you stop the program
```

此操作在下面的图表中有所说明：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_125_0.png)

现在让我们看看如果在我们的 `buffer` 中传播错误会发生什么。为此，让我们创建一个 `subject`：

```
var subject = new Rx.Subject();

var bufferedObservable = subject.bufferWithCount(2);

bufferedObservable.subscribe(
  (i)=>console.log(i),
  (err)=>console.log('An error happened ' + err.message),
  ()=>console.log('Finished')
);

subject.onNext(0);
subject.onNext(1);
subject.onNext(2);

subject.onError(new Error(':('));

subject.onNext(3);
subject.onCompleted();
```

在此示例中，我们首先创建一个 `subject` 并使用 `bufferWithCount()` 创建每两个项目一组的数据块并订阅它。然后我们推送三个值（0、1 和 2）。前两个（0 和 1）将被传播并打印到控制台，第三个被缓冲，直到错误被推送到 `subject`。这使得错误直接通过可观察对象传播。

如果运行此代码，你将看到以下输出：

```
[ 0, 1 ]
An error happened :(
```

现在，你可能会想知道如果一个可观察对象完成而计数未达到期望计数会发生什么。为了测试它，我们可以使用一个简单的程序：

```
Rx.Observable
  .of(0,1,2,3,4)
  .bufferWithCount(2)
  .subscribe((i)=> console.log(i));
```

如果运行此代码，你将看到以下输出：

```
[ 0, 1 ]
[ 2, 3 ]
[ 4 ]
```

因此，如你所见，如果原始可观察对象完成，RxJS 将刷新当前缓冲区。

现在让我们看看第二个参数是如何工作的，比较两个订阅：

```
var observable = Rx.Observable.interval(1000);

var observableWithDefaultSkip = observable.bufferWithCount(4);
var observableWithSkipOne = observable.bufferWithCount(4,1);

observableWithDefaultSkip.subscribe((arr)=>console.log('with default skip :'+arr));
observableWithSkipOne.subscribe(
  (arr)=>console.log('with default skip equals to one:'+arr)
);
```

如果运行此代码（并等待片刻），你将看到如下输出：

```
with default skip :0,1,2,3
with default skip equals to one:0,1,2,3
with default skip equals to one:1,2,3,4
with default skip equals to one:2,3,4,5
with default skip equals to one:3,4,5,6
with default skip :4,5,6,7
with default skip equals to one:4,5,6,7
//continues printing until you stop the program
```

从输出可以看出，如果你不提供 skip 值，缓冲区在被传播后基本上会被清除。这样它每 4 秒才运行一次（因为我们的原始可观察对象每秒推送一个值，需要 4 秒才能填满数组中的四个项目），但如果你提供一个 skip 值，它将移除这数量的项目（从左到右）并始终在数组末尾推送新值。这样它每秒运行一次（因为我们的缓冲区始终保留上一个缓冲区的三个项目，只需 1 秒即可填满数组）。

最后，bufferWithCount() 方法的一个有用应用是计算序列中最后 X 个数字的平均值。为了展示此应用，让我们首先创建一个用于生成随机数的 Observable：

```javascript
var randomNumbersGenerator = Rx.Observable
    .interval(100)
    .map((i)=>Math.floor(Math.random()*100));
```

这里我们创建一个每 100 毫秒生成一个项目的 Observable。然后我们将其映射为 0 到 99 之间的整数值。

现在让我们计算最后五个项目的平均值。为此，我们调用 bufferWithCount() 方法，大小为 5，skip 为 1。然后我们计算平均值并打印到控制台。

完整代码如下：

```javascript
var randomNumbersObservable = Rx.Observable
    .interval(100)
    .map((i)=>Math.floor(Math.random()*100));

randomNumbersObservable
    .bufferWithCount(5,1)
    .map((arr)=>{
        var sum = arr.reduce((acc,b)=>acc+b);
        return sum/arr.length;
    })
    .subscribe((i)=>console.log('The average of the last five items is: '+i));
```

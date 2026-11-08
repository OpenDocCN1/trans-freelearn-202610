
现在我们提供给 map() 运算符的函数返回一个 Observable，其包含单个值，即原始值加 1，当你运行这段代码时，你会看到以下输出：

```
JustObservable { _value: 2, _scheduler: ImmediateScheduler {} }
JustObservable { _value: 3, _scheduler: ImmediateScheduler {} }
JustObservable { _value: 4, _scheduler: ImmediateScheduler {} }
```

所以它创建了一个包含 observable 的 observable；此操作可以用下图描述：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_196_0.png)

虽然这并不错误且可以实现，但大多数时候我们关注的是 observable 的值，而此时 flatMap() 运算符派上用场。该运算符接收一个参数，即将给定值映射到 observable 的函数，因此该函数的返回值必须始终是 observable，并且该运算符会生成一个新的 observable，将所有 observable 的所有值传播出来；它基本上映射值然后展平你的 observable of observables。

以下是使用 flatMap() 的相同代码：

```
Rx.Observable
.of(1,2,3)
.flatMap((i)=>Rx.Observable.just(i+1))
.subscribe((i)=>console.log(i));
```

这段代码会给出你期望的输出：

2
3
4

此操作可以用下图描述：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_197_0.png)

你可能会想，如果提供给 flatMap() 运算符的函数没有返回 observable 会发生什么；让我们在以下示例中查看：

```
Rx.Observable
.of(1,2,3)
.flatMap((i)=>i+1)
.subscribe((i)=>console.log(i));
```

好事不会发生；运行这段代码时会发生异常，你会得到如下截图中所示的堆栈跟踪：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_197_1.png)

这显然是错误的。

因此请记住，提供给 map() 运算符的函数可以返回任何类型，如果你返回一个 observable，它将创建一个 observable of observables，而提供给 flatMap() 运算符的函数必须始终返回一个 observable。

我们可以使用运算符来模拟其他运算符，而 flatMap() 非常强大，因为你可以用它来模拟许多不同的运算符；例如，让我们仅使用 flatMap() 运算符来模拟 filter() 运算符。

## 挑战 - 仅使用 flatMap() 运算符创建过滤元素的函数

为了展示 flatMap() 运算符的强大功能，我们可以创建一个函数，仅使用 flatMap() 运算符来模拟 filter() 运算符；该函数必须具有以下签名：

```
filterUsingFlatMap(observable,filter);
```

它将接收两个参数，两者都是必需的：

- `observable`：它是用于过滤数据的 observable
- `filter`：它是一个函数，用于选择元素是否应被过滤

它必须返回一个用于过滤的 observable。

在编写函数之前，让我们先创建一个测试套件来测试它。

让我们使用上一章学到的 mocha。我们可以使用 describe() 函数来定义我们的测试组：

```
var assert = require('assert');
var Rx = require('rx');
var filterUsingFlatMap = require('./example8_6');
describe('Test for filter function', function() {
  //Test goes here
});
```

在这段代码中，我们导入了 assert 模块和 Rx 模块，前者用于对我们的测试进行断言，后者用于创建 observable 进行测试。第三个模块包含我们想要测试的函数；该函数包含在名为 example8_6 的文件中（可以从网站下载）。我也会在这里给出它的实现。

describe() 函数为我们的实现创建一组测试，我设计了四个测试；它们并不详尽，你当然可以想出更多测试来确保实现正确。

第一个测试确保当将一个空 observable 作为参数传递给 filter() 函数时，该函数返回的 observable 不会发出任何数据；可在以下代码中看到：

```
it('should not propagate any data if the observable is empty',
function(done) {
    var eventCalled = false;
    var observable = Rx.Observable.empty();
    filterUsingFlatMap(observable,function noop(){})
        .subscribe(
            function(){eventCalled=true},
            function(){eventCalled=true},
            function(){
                assert.equal(eventCalled,false);
                done();
            }
        )
});
```

对于此测试，我们必须确保 onNext() 和 onError() 事件永远不会被调用。为了测试它，我决定创建一个标志（eventCalled），每当结果 observable 发出这些事件中的任何一个时就更改该标志的值。然后我们创建一个空 observable 并将其作为参数传递给 filterUsingFlatMap() 函数；我们还添加了一个 empty() 函数，因为在测试中我们不关心它，并添加了一个 onCompleted() 函数来断言标志未发生变化并通过调用 done() 函数结束测试。

下一个测试确保当我们的 filter() 函数返回 true 时数据会被传播，可在以下代码中看到：

```
it('must propagates all the data if the filter function always returns true', function(done) {
    var counter=0,elements = [1,2];
    var observable = Rx.Observable.fromArray(elements);
    filterUsingFlatMap(observable,function(){return true;})
        .subscribe(
            function(){counter++;},
            function noop(){},
            function(){
                assert.equal(counter,elements.length);
                done();
            }
        )
});
```

对于此测试，我们添加一个计数器来计算传播的元素数量，每当调用 onNext () 方法时就将 1 加到该计数器；我们还创建一个数组（elements）并从中创建一个 observable，并在我们的 onCompleted() 方法中确保计数器等于数组中的元素数量（这确保了所有数据都被传播）并结束测试。

第三个测试做完全相反的事情，确保当我们的 filter() 函数返回 false 时所有数据都被过滤，可在以下代码中看到：

```
it('should not propagate any data if the filter function always return false',function(done){
    var counter=0,elements = [1,2];
    var observable = Rx.Observable.fromArray(elements);
    filterUsingFlatMap(observable,function(){return false;})
        .subscribe(
            function(){counter++},
            function noop(){},
            function(){
                assert.equal(counter,0);
                done();
            }
        );
});
```

这个测试与上一个非常相似；唯一的变化是它期望计数器为零（这意味着没有数据被传播）。

对于最后一个测试，我们取一个数组并过滤偶数，其实现可在以下代码中看到：

```
it('must respect the filter',function(done){
    var counter=0,elements = [1,2];
    var elementFound;
    var observable = Rx.Observable.fromArray(elements);
    filterUsingFlatMap(observable,function(i){return i % 2===0;})
        .subscribe(
            function(i){
                counter++;
                elementFound=i;
            },
            function noop(){},
            function(){
                assert.equal(counter,1);
                assert.equal(elementFound,2);
                done();
            }
        );
});
```

在这里我们更改了 filter() 函数，使其对偶数返回 true，对奇数返回 false；我们还将找到的元素存储在一个变量中，并且由于该数组中唯一的偶数是 2，我们期望计数器等于 1 且找到的元素等于 2。

现在我们有了测试，可以准备仅使用 flatMap() 实现我们的 filter() 函数了；一个好的做法是在尝试真正实现之前先让测试失败，所以让我们尝试以下 filter() 函数的实现：

```
var Rx = require('rx');
module.exports = function(observable,filter){
return observable;
};
```

在我们第一次尝试实现 filter() 函数时，我们只是返回原来的 observable，因此我们的一些测试必须失败。如果我们运行测试，会发生以下情况：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_201_0.png)

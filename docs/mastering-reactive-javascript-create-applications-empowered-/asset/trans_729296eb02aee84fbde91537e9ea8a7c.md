
在这个示例中，我们创建了一个 Observable，它只发出一个字符串，并在此 Observable 上调用了 map() 操作符并订阅它以在控制台中显示结果：

```
Hello World
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_99_0.png)

如果你记不起第 2 章《Reacting for the First Time》中介绍的 map() 操作符，不用担心！我们将在本章后面看到更多关于它的内容以及它的作用。

每次你在 observable 上调用一个操作符时，它都会返回一个新的 observable，并应用该转换。这样，我们就可以轻松地链式调用多个操作符，如下所示：

```
Rx.Observable
.of(1,2,3)
.map((num)=>num*2)
.filter((num)=> num>2)
.subscribe((num)=>console.log(num));
```

在前面的示例中，我们创建了一个发出三个数字的 Observable。然后我们在此 Observable 上调用 map() 操作符来先转换数据，然后应用 filter() 操作符来生成一个新的 Observable。这段代码也可以写成如下形式：

```
var initialObservable = Rx.Observable.of(1,2,3);
var initialMappedObservable = initialObservable
    .map((num)=>num*2);
var initialMappedAndFilteredObservable = initialMappedObservable
    .filter((num)=> num>2);
initialMappedAndFilteredObservable.subscribe((num)=>console.log(num));
```

两者在执行时提供相同的输出：

```
4
6
```

应用操作符后创建的每个 Observable 都可以被订阅，从而提高代码的复用性。使用操作符，我们可以编写更简洁且更易于测试的代码，因为它让我们能够将数据的来源与对数据的处理分离。

## map() 操作符

map() 操作符在函数式语言和框架中处理数组或其他类型的序列时很常见。在 JavaScript 中，数组对象有一个名为 map() 的方法。该方法现在在所有现代浏览器中都可用。

map() 操作符对 Observable 中的每个元素调用提供的函数一次。该函数以 Observable 中的对象作为输入并返回另一个对象。因此，map() 操作符返回的 Observable 将传播对原始 Observable 的每个元素调用 map 函数的结果。

提供的函数在原始 Observable 传播对象时立即被调用；调用顺序相同。

因此 map() 操作符具有以下签名：

```
observable.map(mapFunction, [thisContext]);
```

第一个参数是必需的，第二个是可选的：
- mapFunction: 这是一个函数，它以 observable 的元素作为输入并返回另一个要传播的对象
- thisContext: 这是任何用作 mapFunction 的 this（JavaScript 上下文）的对象

如果我们有一个数字的 Observable 并想通过将这些数字加倍来创建一个新的 Observable，我们可以使用 map() 操作符，如下所示：

```
Rx.Observable
  .of(1,2,3)
  .map((i)=>i*2)
  .subscribe((i)=>console.log(i));
```

在这段代码中，首先我们创建了一个传播三个数字（1、2 和 3）的 Observable。然后，我们在此 Observable 上应用 map() 操作符；这创建了一个具有映射后对象的新 Observable。然后我们订阅此 Observable 以记录所有元素。如果运行前面的代码，你将在控制台中看到以下输出：

```
2
4
6
```

此操作可以用以下图表表示：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_101_0.png)

此操作符可用于任何类型的对象，并且如本章所述，它不会更改初始 Observable，并且始终创建一个新的 Observable：

```
var namesObservable = Rx.Observable.of("John", "Mary")

var helloObservable = namesObservable.map((name)=>"Hello "+name)
helloObservable.subscribe((i)=>console.log(i));

var thanksObservable = namesObservable.map((name)=>name+" thanks for your visit.")
thanksObservable.subscribe((i)=>console.log(i));
```

在前面的代码中，我们从两个用户名（John 和 Mary）创建了一个 Observable。然后我们将其映射为一个对每个用户说 hello 的字符串 Observable，并订阅该 observable 以在控制台中打印每条消息。我们还使用原始 Observable（仅包含名称）为不同的消息创建了一个新的 Observable，然后订阅它以记录消息。如果运行前面的代码，你将看到以下输出：

```
Hello John
Hello Mary
John thanks for your visit.
Mary thanks for your visit.
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_102_0.png)

此操作符有一个别名，称为 `select`。

## flatMap() 操作符

在上一节中，我们看到了 `map()` 操作符以及如何使用它将 observable 中的一个对象映射为另一个对象。`flatMap()` 操作符的工作方式类似。它接收一个函数作为参数。作为参数提供的函数必须返回一个 Observable，并且该 Observable 的元素将被传播。

这个操作符可能看起来有点令人困惑，但还有另一种理解方式。`flatMap()` 操作符对原始数组中的每个元素运行给定的函数。它创建一个 Observable，该 Observable 将发出来自其他 Observable 的每条数据，同时保持顺序。

`flatMap()` 操作符的签名如下所示：

```
observable.flatMap(flatMapFunction, [thisContext]);
```

第一个参数是必需的，第二个是可选的：
- `flatMapFunction`: 这是一个函数，它以 observable 的元素作为输入并返回另一个其元素将被传播的 observable
- `thisContext`: 这是任何用作 `flatMapFunction` 的 `this`（JavaScript 上下文）的对象

你可以在以下代码中看到此操作符的示例用法：

```
Rx.Observable
  .of(1,2,3)
  .flatMap((i)=>Rx.Observable.of(i,i*2))
  .subscribe((i)=>console.log(i));
```

在前面的示例中，我们首先创建一个将传播三个数字（1、2 和 3）的 Observable。然后，我们在此 Observable 上应用 `flatMap()` 操作符；这将创建一个新的 Observable，该 Observable 将发出每个 Observable 发出的每个值，并且它将由 `flatMapFunction` 返回。`flatMapFunction` 返回一个包含该值及其两倍的 Observable。因此在值 1 的示例中，我们创建一个发出值 1 和 2 的 Observable；对于值 2，我们发出 2 和 4；对于值 3，我们发出 3 和 6。

如果我们运行前面的代码，它将显示以下输出：
1
2
2
4
3
6

此操作可以用以下图表表示：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_103_0.png)

我们可以轻松地链式调用不同的操作符，如下所示：

```
Rx.Observable
    .of(1,2,3)
    .map((i)=>i+1)
    .flatMap((i)=>Rx.Observable.of(i,i*2))
    .subscribe((i)=>console.log(i));
```

在前面的示例中，我们链式调用了 map() 和 flatMap() 操作符。首先我们为 1、2 和 3 创建一个 Observable，然后使用 map() 操作符为每个值加 1（现在我们有一个值为 2、3 和 4 的 Observable）。然后，我们使用 flatMap() 操作符发出一个包含该值及其两倍的新序列。

如果运行前面的代码，你将看到以下输出：
2
4
3
6
4
8

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_104_0.png)

此操作符有一个别名，称为 `selectMany`。

## filter() 操作符

`filter()` 操作符让你在从原始 Observable 中省略部分数据后创建一个新的 Observable。它接收一个作为参数的函数，该函数对原始 Observable 中的每个元素调用，并且必须返回 true 或 false（实际上它可以是任何 truthy 或 falsy 值）。如果对某个对象执行 `filterFunction` 的结果为 true，则该对象将被传播；否则将被省略。

`filter()` 操作符具有以下签名：

```
observable.filter(filterFunction,[thisContext]);
```

第一个参数是必需的，第二个是可选的：
- `filterFunction`: 这是一个函数，它以 observable 的元素作为输入并返回任何 truthy 或 falsy 值。如果对给定对象执行此函数的结果为 true，则该对象将被传播；否则将被省略。
- `thisContext`: 这是任何用作 `filterFunction` 的 `this` 的对象

我们可以按如下方式看到 `filter()` 的使用示例：

```
Rx.Observable
.of(1,2,3)
.filter((i)=> i % 2 === 1 )
.subscribe((i)=>console.log(i));
```

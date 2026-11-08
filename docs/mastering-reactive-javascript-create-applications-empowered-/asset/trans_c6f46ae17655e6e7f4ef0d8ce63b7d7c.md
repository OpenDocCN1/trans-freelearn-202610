
想象一个输入时搜索的功能。在此功能中，每次按键你都会执行一次搜索并在屏幕上展示当前结果；如果你正在访问外部资源，例如数据库或 API，一些搜索可能比其他搜索花费更长时间，而这种搜索的天真实现（例如使用 flatMap()）可能会显示错误的结果。请看下面的示例：

- 1. 一个用于在数据库中搜索姓名的搜索框。
- 2. 用户输入字母 J 并开始搜索所有以字母 J 开头的可用姓名。
- 3. 用户输入字母 O 并开始搜索所有以 JO 开头的可用姓名。
- 4. 以 JO 开头的姓名搜索完成并展示结果（例如 John 和 Joe）。
- 5. 第一次搜索（以 J 开头的姓名）最终完成（它花费的时间比 JO 搜索更长），并使用以 J 开头的姓名更改展示的结果（例如 Jean、June、John）。
- 6. 展示的结果是错误的，因为用户搜索框显示的是 JO，而其中一些展示结果并不以 JO 开头（Jean 和 June）。

flatMapLatest() 操作符非常适合这种情况，因为在示例中，如果 J 的结果花费更长时间完成，则会被丢弃。

flatMapLatest() 操作符具有以下签名：

```
observable.flatMapLatest(selectorFunction, [selectorContext]);
```

它接收最多两个参数；第一个是强制性的，第二个是可选的：

- selectorFunction：它是一个将值映射到可观察序列的函数
- selectorContext：在 selectorFunction 中使用的此参数（上下文）

以下代码展示了此操作符的使用示例：

```
Rx.Observable
.of(1,2,3)
.flatMapLatest(
  (i)=>Rx.Observable.just(i+1)
).subscribe((k)=>console.log(k));
```

此示例很简单，它的工作方式与 flatMap() 操作符相同；它会给出以下输出：

2
3
4

我们可以实现输入时搜索功能来查看差异；为此，让我们创建一个包含实现功能的 HTML 页面并比较结果：

```html
<html>
    <head></head>
    <body>
        <input type="text" id="my_input"/>
        <button id="my_button">Search</button>
        <span id="my_span"></span>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/rxjs/4.1.0/rx.all.js"></script>
        <script>
            var enterKeyUpObservable = Rx.Observable
                .fromEvent(document.getElementById("my_input"),"keyup")
                .flatMap(function(e){
                    var value = document.getElementById("my_input").value;
                    return Rx.Observable
                        .just("results for: "+value)
                        .delay(Math.max((10 - value.length)*1000,100));
                });
            enterKeyUpObservable.subscribe(function(text){
                document.getElementById('my_span').innerHTML = text;
            });
        </script>
    </body>
</html>
```

此 HTML 页面将公开一个搜索框，当你在其中输入时它会执行一次假搜索，以文本形式显示搜索中使用的值；我们使用 flatMap() 操作符来模拟搜索功能，并使用延迟来模拟执行搜索所需的时间，如果你运行此 HTML 页面并在搜索框中输入内容，你将看到以下截图：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_209_0.png)

它仅显示字母 s 的结果，但我们想要的是所有给定输入的结果；要修复此问题，我们只需改用 flatMapLatest() 操作符即可。使用以下代码：

```
var enterKeyUpObservable = Rx.Observable
    .fromEvent(document.getElementById("my_input"),"keyup")
    .flatMapLatest(function(e){
        var value = document.getElementById("my_input").value;
        return Rx.Observable
            .just("results for: "+value)
            .delay(Math.max((10 - value.length)*1000,100));
    });
```

你将看到类似以下内容的 HTML 页面：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_210_0.png)

## The flatMapFirst() operator

此操作符从第一个映射的可观察对象传播数据，并且只有在它完成并丢弃完成前所有其他可观察对象后才会转到下一个。

flatMapFirst() 操作符具有以下签名：

```
observable.flatMapFirst(selectorFunction,[selectorContext]);
```

它接收最多两个参数；第一个是强制性的，第二个是可选的：

- selectorFunction：它是一个将值映射到可观察序列的函数
- selectorContext：在 selectorFunction 中使用的此参数（上下文）

以下代码展示了此操作符的使用示例：

```
Rx.Observable
    .interval(50)
    .flatMapFirst(
        (i)=>Rx.Observable.interval(30)
            .map((k)=>'running for: '+i+' flatMapFirst: '+k)
            .take(3)
    ).subscribe((i)=>console.log(i));
```

在此示例中，我们从 interval() 创建一个 Observable；此可观察对象将每 50 毫秒发出从零开始的连续整数。我们使用 flatMapFirst() 将其映射到也从 interval() 创建的可观察对象以生成字符串（此字符串仅用于调试目的，以显示原始可观察对象中正在运行的值），然后我们取三个项目（以确保它完成并可以转到下一个）并订阅它们。

如果你运行此代码，你将看到以下输出：

```
running for: 0 flatMapFirst: 0
running for: 0 flatMapFirst: 1
running for: 0 flatMapFirst: 2
running for: 3 flatMapFirst: 0
running for: 3 flatMapFirst: 1
running for: 3 flatMapFirst: 2
running for: 5 flatMapFirst: 0
running for: 5 flatMapFirst: 1
//keeps printing until we stop the program
```

通过查看输出，更容易理解此操作符的工作方式；它开始为值 0 运行（从原始可观察对象传播的第一个值），并传播来自 flatMapped 可观察对象的所有元素（格式为 running for: 0 flatMapFirst: X 的前三个字符串）。它丢弃在 flatMapFirst() 的可观察对象运行时传播的所有值；因此，值 1 和 2 被丢弃，它转到下一个值，即值 3，执行重复。

此操作符有一个别名，称为 selectSwitchFirst。

## The finally() operator

此操作符简单且有用，其工作方式类似于 try/catch/finally 块中的 finally。它在原始可观察对象完成（即使在失败的情况下）后执行指定的函数。

它具有以下签名：

```
observable.finally(action);
```

它只接收一个参数且是强制性的：

- **action**：当原始可观察对象完成时（即使在失败的情况下）要执行的函数

此操作符很简单，两个示例就足以理解它。

第一个示例展示了其在可观察对象优雅完成时的用法：

```
Rx.Observable
.just(1)
.finally(()=>console.log('FINALLY'))
.subscribe((i)=>console.log(i));
```

如果你运行此代码，你将看到以下输出，显示在可观察对象完成之后提供的 finally 函数被执行：

```
1
FINALLY
```

另一个示例展示了此操作符在失败情况下的行为：

```
Rx.Observable
.throw(new Error('ERROR'))
.finally(()=>console.log('FINALLY'))
.subscribe((i)=>console.log(i));
```

在这种情况下，我们的可观察对象中发生了错误，如果你运行此代码，你将看到以下输出，显示在可观察对象完成之后提供的 finally() 函数被执行：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_212_0.png)

## The groupBy() operator

此操作符允许你对可观察序列的值进行分组。它将生成一个包含每个组的可观察对象的可观察对象。

它具有以下签名：

```
observable.groupBy(groupFunction,[valueSelector]);
```

它接收两个参数，第一个强制性，第二个可选：

- `groupFunction`：它是一个用于决定值属于哪个组的函数
- `valueSelector`：它是一个用于选择要传播的值的函数；如果未指定，则传播其自身的值

此操作符的一个简单使用示例是在可观察序列中将奇数和偶数分开：

```
Rx.Observable
.of(0,1,2,3,4,5)
.groupBy((i)=>i%2)
.subscribe((obs)=>{
  obs.toArray().subscribe((arr)=>console.log(arr))
});
```

在此示例中，我们使用 groupBy() 操作符将偶数和奇数分开，因为它为每个组生成一个可观察对象，订阅函数接收到的参数是一个可观察对象，我们然后将其转换为仅包含一个数组的可观察对象并记录它；因此，如果我们运行此代码，我们将在控制台中看到打印偶数数组和奇数数组：

```
[ 0, 2, 4 ]
[ 1, 3, 5 ]
```

使用第二个参数的另一个使用示例是按首字母对姓名进行分组：

```
Rx.Observable
.of(
  {name:'Mary',lastName:'Jane'},
  {name:'John',lastName:'Doe'},
  {name:'Jean',lastName:'Carter'},
  {name:'Erich',lastName:'Oliveira'})
.groupBy(
  (person)=>person.name[0],
  (person)=>person.name+' '+person.lastName
```

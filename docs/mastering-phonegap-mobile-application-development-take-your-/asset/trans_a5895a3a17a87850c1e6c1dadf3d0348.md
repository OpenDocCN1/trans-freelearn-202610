
回调函数接受 `err` 和 `result` 两个参数。如果 `err` 的值不是 `undefined`，则表示发生了错误，我们可以适当处理。否则，我们可以记录操作的结果。

### Note

`(err, result)` 模式是 Node.js 应用程序中的典型模式。

另一种类似的回调模式如下所示——这是使用 Cordova 插件时常见的模式：

```
function slowDiv(a, b, success, failure) {
  setTimeout(function() {
    if (b === 0) {
      failure("Can't divide by zero");
    } else {
      success(a / b);
    }
  }, 2500 + Math.random() * 10000);
}
slowDiv(10, 5, function success(result) {
  console.log(result);
}, function failure(err) {
  if (err !== undefined) {
    console.log(err);
    return;
  }
});
```

前两个代码片段的功能完全相同——它们都非常缓慢地返回除法结果。只要只需要执行一次，这看起来还不错。但如果需要将多个除法串联起来，生成的代码很快会变得非常丑陋。

为了对抗“厄运金字塔”以及随之而来的代码理解困难，**Promise** 被开发出来。它允许你编写一个函数，承诺在未来的某个时刻返回结果。通过在生成的 Promise 上调用 `then` 方法，Promise 也可以被串联。通过提供 `catch` 处理程序，也可以捕获错误。

已经有许多库为 JavaScript 提供了 Promise。然而，ES2015 将 Promise 纳入了语言标准。这并不一定消除了对其他库的需求（它们可能提供其他有趣的功能），但这意味着你可以在不依赖库的情况下使用 Promise。

### Note

Promise 的完整入门指南超出了本章范围。如果你想了解更多关于 Promise 的信息，请参阅 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 和 [https://www.promisejs.org](https://www.promisejs.org)。

借助 Promise 和其他 ES2015 语法，我们可以重写之前的代码如下：

```
function slowDiv(a, b) {
  let p = new Promise((resolve, reject) => {
    setTimeout(() => {
      if (b === 0) {
        reject("Can't divide by zero");
      } else {
        resolve(a / b);
      }
    }, 2500 + (Math.random() * 10000));
  });
  return p;
}
slowDiv(10, 5)
  .then(result => console.log(result))
  .catch(err => console.log("error", err));
```

这比回调示例更好；当我们将两个慢速除法串联在一起时，改进的程度将变得显而易见，如下所示：

```
slowDiv(10, 5)
  .then(result => slowDiv(20, result))
  .then(result => console.log(result)) // 20 / ( 10 / 5 ) = 10
  .catch(err => console.log("error", err));
```

尽管这很棒（并且已被许多 JavaScript 开发者广泛采用），但它仍然不像自然代码。如果我们能像编写同步代码一样编写异步代码，那就太好了。

在 ES2015 中，我们做不到。我们可以通过生成器和生成器运行器的组合来接近，但这仍然不够自然。然而，下一个版本的 ECMAScript (ES2016) 给了我们一种几乎完全实现我们想要的方法：`await` 和 `async` 关键字。

### Note

如果你想了解如何在 ES2015 中模拟 `await`，请参阅 [http://davidwalsh.name/async-generators](http://davidwalsh.name/async-generators)。

有了这两个关键字，我们现在可以按如下方式编写串联除法：

```
async function doCalculation_a() {
  try {
    let result1 = await slowDiv(10, 5);       // 2, eventually
    let result2 = await slowDiv(20, result1); // 10, eventually
    console.log(result2);
  } catch (err) {
    console.log("error", error);
  }
}
```

在前面的示例中有几点需要注意。首先，有一个新的 `async` 关键字（见 `[1]`）——它标记任何返回异步结果的函数。其次，`await` 用于标记我们的异步函数正在等待来自 Promise 的值（见 `[2]`）。最后，它看起来几乎与等效的同步代码完全相同！这意味着我们可以更轻松地推断代码的工作原理。

只有一个问题：`await` 必须存在于 `async` 函数中，这意味着任何调用函数也必须正确处理异步性。因此，如果不做计划，你不能随心所欲地在任何地方使用它。即使有这个警告，它也非常有用。

### Tip

`await` 和 `async` 的语法尚未最终确定。与已被标准机构接受的 ES2015 不同，此功能的语法（甚至语义）在未来可能会发生变化。因此，在生产代码中使用 `await` 和 `async` 时要小心。

你也可以在表达式中使用 `await`。因此，我们可以按如下方式编写 `doCalculation` 方法：

```
async function doCalculation() {
  try {
    // 10, eventually
    console.log( await slowDiv(20, await slowDiv(10, 5)));
  } catch (err) {
    console.log("error", error);
  }
}
```

如何使用 `await` 很大程度上取决于你；第二个示例稍微难读一些，但功能相同。

### Note

使用 ES2016 特性被认为是实验性的。因此，Babel 默认不会识别它。我们将在本章后面的*修改 Gulp 配置*部分解决这个问题。

## 类

类可能是 ES2015 中最具争议的特性之一。但事实上，它们只是 JavaScript 现有原型本质的语法糖。如果你不想使用 ES2015 类，你不必使用——你仍然可以像以前一样创建对象。然而，从可读性的角度来看，有一些明确的好处。

### Note

本节中的代码片段位于代码包的 `snippets/02/ex9-classes/a` 中。使用交互式代码片段演练场，在第一个下拉菜单中选择 **2: ES2015 and Browserify**，在第二个下拉菜单中选择示例 **9**。

让我们快速看一下新语法。在下面的片段中，我们定义了一个简单的 `Point` 类，表示二维平面上的 x 和 y 坐标：

```
class Point {
  constructor(x = 0, y = 0) { // [1]
    [this._x, this._y] = [x, y];
  }

  get x() { // [2]
    return this._x;
  }
  set x(x) { // [3]
    this._x = x;
  }
  get y() {
    return this._y;
  }
  set y(y) {
    this._y = y;
  }

  toString() {
    return `(${this.x}, ${this.y})`;
  }
  copy() {
    return new Point(this.x, this.y);
  }
  equal(p) {
    return (p.x === this.x) && (p.y === this.y);
  }
}
```

请注意，我们使用 `constructor` 关键字定义了构造函数（`[1]`）。属性的 getter 和 setter 分别使用 `get`（`[2]`）和 `set`（`[3]`）定义。现在，假设我们要创建一个矩形。我们可以简单地指定左上角和右下角的点；让我们允许开发者指定*大小*。虽然我们可以将大小设为 `Point`，但使用 `w` 和 `h` 代替 `x` 和 `y` 会更方便。这很容易实现，如下片段所示：

```
class Size extends Point { // [1]
  constructor(w = 0, h = 0) {
    super(w, h);
  }
  get w() {
    return this.x;
  }
  set w(w) {
    this.x = w;
  }
  get h() {
    return this.y;
  }
  set h(h) {
    this.h = h;
  }
  copy() {
    return new Size(this.w, this.h);
  }
}
```

请注意新的 `extends` 关键字（`[1]`）——它允许你指明一个类基于另一个类。新类会自动获得所有先前的属性和方法。更棒的是，子类可以使用 `super` 调用其父类的方法。

现在我们可以创建一个 `Rectangle` 类：

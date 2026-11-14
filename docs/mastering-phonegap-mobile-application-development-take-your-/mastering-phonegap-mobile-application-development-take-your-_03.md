## ES2015 和 Browserify

`JavaScript` 迅速成为 Web 的*通用语言*，其语法和语义早就需要改进。`ECMAScript 2015` 引入了大量语法糖，使编写 `JavaScript` 更容易。它还提供了许多新的有用概念，简化了应用程序开发。我们在上一章简要介绍了 `ES2015`。我们将深入探讨 `ES2015`，因为熟悉新语法的样子很重要。

此外，重用自己的工作和他人的工作非常有用。但这需要你管理依赖关系并确保包含适当的库。手动做这件事很痛苦；但你可以利用 `Browserify` 等工具来协助。`Browserify` 不仅打包你的 `JavaScript` 文件及其依赖，还为浏览器提供类 `Node.js` 的环境。这意味着你可以重用 `Node.js` 使用的庞大 `JavaScript` 仓库的大部分。毕竟，如果别人已经完成了艰苦的工作，为什么要重复造轮子？

在本章中，我们将涵盖：

*   新的 `ES2015` 语法
*   使用 `Browserify`




本章中的多个代码片段也可以通过随本书代码包附带的交互式代码片段演练场来使用。启动演练场的说明在代码包中的 `README.md` 文件里。启动后，选择 `2: ES2015 and Browerify`，然后浏览并尝试相邻下拉菜单中的示例。如果适用，每个章节和代码片段都会指示你应该选择哪个示例。

虽然我们在上一章简要涉及了 `ES2015` (`ECMAScript 2015`)，但你可能还不太熟悉它。大多数浏览器（尤其是移动端）对 `ES2015` 要么完全支持，要么完全不支持。这很遗憾，因为语法和语义方面有巨大的改进，使得创建复杂的移动应用变得更容易。

所幸的是，正如你在上一章所见，有一些工具可以将这些功能转换回 `ES5`，而 `ES5` 在现代浏览器中得到了*非常好*的支持。这些工具将 `ES2015` 的语法和语义转换为其 `ES5` 的等价形式（在可能的情况下）。在无法完美等价的情况下，转换会尽可能接近，这仍然会给我们带来巨大的好处。

我们无法在本书中涵盖 `ES2015` 中的*每一项*新变化；但我们确实需要过一遍一些常见示例，特别是如果你不熟悉该语法的话。

### 注意

如果你是为旧设备开发，且打算使用 `ES2015` 代码，你将需要特别关注应用的性能特征。`ES2015` 代码不一定比 `ES5` 快，尤其是在经过转译时。遗憾的是，即使是 `ES2015` 的原生实现也不总是更快，因此你必须在开发效率和代码可读性的收益与应用的实际性能之间取得平衡。

## 块级作用域

正确理解变量作用域是 `ES5` 中开发者遇到的一个非常常见的陷阱。变量不是 `block-scoped`（块级作用域，这是大多数具有块概念的语言的典型特征），而是 `function-scoped`（函数级作用域）。变量定义会被提升到函数定义的顶部，这意味着很容易在变量实际赋值前就使用它们，也很容易重新定义变量（这在 `for` 循环中很典型）。因为这种提升从源代码中并不明显，所以很容易导致意外的惊喜。

另一方面，块级作用域变量允许我们指明变量仅在函数的特定部分有效。我们可以在需要时重用变量名（这对 `for` 循环有益），而无需担心不必要的副作用或意外。让我们看看它们在定义和赋值方面的区别：

| ES5 | ES2015 | 说明 |
| --- | --- | --- |
| `var x;` | `let x;` | `x` 已定义，但无值 |
| `var y = 5;` | `let y = 5;` | `y` 已定义且值为 `5` |
| N/A | `const z = 5;` | `z` 永远不能重新赋值 |
| `for(var i=0,l=5;i<l;i++)` | `for(let i=0,l=5;i<l;i++)` | 迭代 `5` 次 |

### 注意

本节的代码片段位于本书代码包中的 `snippets/02/ex1-block-scope/a` 和 `snippets/02/ex1-block-scope/b`。使用交互式代码片段演练场，在第一个下拉菜单中选择 `2: ES2015 and Browserify`，在第二个中选择示例 `1a` 和 `1b`。

```
// Snippet Example 1a
// Snippet Example 1a
var i = 10, // [1]
    x = 0;
console.log(i); // 10
for (var i = 0, l = 5; i < l; i++) { // [2]
  x += i;
}
console.log(x); // 10 (0+1+2+3+4)
console.log(i); // 5 [3]
console.log(l); // 5
```

注意，在 `[1]` 中定义的 `i` 的值被 `[2]` 中的 `for` 循环覆盖了（正如你在 `[3]` 中看到的）。根据代码的编写方式，这是一个惊喜，特别是对于不理解变量是函数级作用域的人来说。现在，让我们看看 `ES2015` 中的代码：

```
// Snippet Example 1b
let i = 10,
    x = 0;
console.log(i); // 10
for (let i = 0, l = 5; i < l; i++) {
  x += i;
}
console.log(x); // 10 (0+1+2+3+4)
console.log(i); // 10
console.log(l); // Error: Can't find variable: l
```

注意 `i` 不再被覆盖了——为循环的持续时间创建了一个新变量。此外，注意 `l` *完全*不可用；循环结束时它就超出了作用域。

防止变量多次定义是 `let` 和 `const` 的另一个重要特性。`ES5` 在变量被定义多次时不会报错，这导致了前面示例中的意外惊喜。而 `ES2015` 如果你尝试在同一作用域内重新定义变量，*会*报错。例如：

```
let i = 10;
let i = 20; // Error: Duplicate definition "i"
```

### 提示

重要的是要认识到 `var` 并没有消失；使用 `var` 定义变量完全有效。但是，如果你这样做，你需要知道这些变量按 `function-scope`（函数作用域）运行，且不继承任何 `ES2015` 行为。

## 箭头函数

JavaScript 一直严重依赖 `function` 关键字，这往往导致一些难以阅读且包含大量样板代码的代码。请看这个 `ES5` 片段：

```
// Snippet Example 2a
[1, 2, 3].map(function(val) {
  return val * 2;
}).forEach(function(val) {
  console.log(val);
});
// console displays:
// 2
// 4
// 6
```

### 注意

本节的代码片段位于本书代码包中的 `snippets/02/ex2-fat-arrow-functions/a` 和 `snippets/02/ex2-fat-arrow-functions/e`。使用交互式代码片段演练场，在第一个下拉菜单中选择 `2: ES2015 and Browserify`，在第二个下拉菜单中选择示例 `2a-2e`。

虽然这个片段很容易理解，但一直输入 `function` 和 `return` 有点烦人。使用 `ES2015`，我们可以改用以下箭头语法：

```
// Snippet Example 2b
[1, 2, 3].map(val => val * 2)
  .forEach(val => console.log(val));
// console displays the same output as before
```

这些箭头函数的形式根据所需参数的数量而不同。以下是一些示例：

```
// no parameters; note the empty parentheses
[1, 2, 3].map(() => 2); // [2, 2, 2]
```

```
// one parameter; no parentheses required
[1, 2, 3].map(val => val * 2); // [2, 4, 6]
```

```
// multiple parameters; parentheses required
[1, 2, 3].map((val, idx) => val * idx); // [0, 2, 6]
```

注意没有任何 `return` 关键字；箭头后面表达式的结果被假定为返回值。隐式 `return` 仅对单个表达式有效；如果你需要多条语句，必须使用大括号并提供 `return`，如下所示：

```
[1, 2, 3].map((val, idx) => {
  console.log(val);
  return val * idx;
});
```

箭头函数还具有*词法* `this` 值的概念。JavaScript 中 `this` 在任何时间点到底指向什么一直有点难以理解，尤其是在学习语言时。它仍然经常让经验丰富的开发者绊倒。当你做类似以下事情时，问题就出现了：

```
// Snippet Example 2c
var o = {
  a: [1, 2, 3],
  b: 5,
  doMul: function() {
    return this.a.map(function(val) {
      return val * this.b;
    });
  }
}
console.log(o.doMul());
```

虽然这看起来像一段完全无害的代码（我们期望控制台输出 `[5, 10, 15]`），但它实际上会抛出一个错误：`undefined is not an object (evaluating this.b)`。`doMul` 内部的 `this` 引用指向对象 `o`，但当 `this` 在映射函数内部使用时，它不指向 `o`。我们可以通过稍微修改代码来解决这个问题，如下所示：



# ES2015 特性：箭头函数、对象简写、默认参数、剩余参数和解构赋值

## 箭头函数与词法 `this`

```javascript
// Snippet Example 2d
var o = {
  ...,
  doMul: function() {
    return this.a.map(function(val) {
      return val * this.b;
    }, this); // map accepts a this parameter to apply to the callback
  }
}
```

对于不接受 `this` 参数的任何方法，我们也可以使用 `bind` 及其相关方法。但如果我们根本不需要担心这个问题呢？在 ES2015 中，我们可以这样写：

```javascript
// Snippet Example 2e
let o = {
  a: [1, 2, 3],
  b: 5,
  doMul() {
    return this.a.map(val => val * this.b);
  }
}
```

### 注意

ES2015 并没有消除 `this` 的各种怪癖，所以你仍然需要留意自己代码中 `this` 的任何误用。但词法 `this` 值确实有助于缓解这一特定痛点。

然而，因为箭头函数拥有自己的词法上下文，所以你不能在所有可能使用普通函数的地方都使用它们。如果函数需要通过 `bind`（或类似方式）提供许多不同的上下文，你最好改用普通函数。

## 更简洁的对象定义

JavaScript 中的对象定义相当冗长。例如，请看下面的代码片段：

```javascript
// Snippet Example 3a
function makePoint(x, y) {
  return {
    x: x,
    y: y
  };
}
console.log(makePoint(5, 10)); // {x: 5, y: 10}
```

### 注意

本节的代码片段位于本书代码包中的 `snippets/02/ex3-object-definitions/a` 和 `snippets/02/ex3-object-definitions/b`。使用交互式代码片段演练场，在第一个下拉菜单中选择 **2: ES2015 and Browserify**，在第二个下拉菜单中选择示例 **3a** 和 **3b**。

在前面的例子中，似乎有点多余地指定了两次 `x` 和 `y`，不是吗？虽然在这个例子中这并不算特别繁琐，但在 ES2015 中，我们可以写成如下形式，简洁得多：

```javascript
// Snippet Example 3b
function makePoint (x, y) {
  return {x, y};
}
console.log(makePoint(5,10)); // {x: 5, y: 10}
```

在 ES2015 中，键名是可选的。如果未提供，ES2015 会假定键名应与变量名匹配。在前面的例子中，生成的对象将有两个键，即 `x` 和 `y`，它们将拥有传递给该函数的值。

函数也可以使用这种简写来指定，正如你在上一节所见。不必输入 `myFunction: function() {…}`，ES2015 允许你指定 `myFunction() {…}`。

## 默认参数

编写带有可选参数的函数非常常见。当这些参数未提供时，会改用默认值。不幸的是，这种实现方式往往充满了问题和潜在的惊喜，并导致了许多 bug。在 ES5 中，默认参数通常按如下方式处理：

```javascript
// Snippet Example 4a
function mul(a, b, log) {
  var ans = a * b;
  if (log === true) {
    console.log(a, b, ans);
  }
}
mul(2, 4); // doesn't log anything
mul(4, 8, true); // logs 4 8 32
```

### 注意

本节的代码片段位于本书代码包中的 `snippets/02/ex4-default-arguments/a` 和 `snippets/02/ex4-default-arguments/b`。使用交互式代码片段演练场，在第一个下拉菜单中选择 **2: ES2015 and Browserify**，在第二个下拉菜单中选择示例 **4a** 和 **4b**。

虽然这在*技术上*可行，但远非理想，因为从代码快速阅读中并不明显 `log` 实际上是可选的。此外，如果变量检查不正确，很容易陷入令人惊讶的情况（例如，`if (log)` *不等同于* `if (log===true)`）。

### 注意

也经常看到以下模式：

```javascript
function repeat (a, times) {
  for (var i = 0; i < (times || 1) /*[1]*/; i++) {
    console.log(a);
  }
}
```

遗憾的是，如果我们用 `times` 设为零来调用此方法，这将导致错误行为。因为零是假值，[1] 处的表达式实际上会求值为 1，而不是 0。

如果能完全避免这团糟就好了。ES2015 让我们可以用以下方式编写前面的代码：

```javascript
// Snippet Example 4b
function mul(a, b, log = false) {
  var ans = a * b;
  if (log === true) { // [1]
    console.log(a, b, ans);
  }
}
```

这对 JavaScript 的真值概念没有任何作用（所以，我们仍应保留 `[1]` 处的检查）。然而，它确实让读者一眼就能看出 `log` 是可选的，以及如果未提供时将接收什么值。

## 可变参数（剩余参数）

JavaScript 中另一种常见模式是编写接受可变数量参数的方法。常见的例子是 `console.log` 方法——它接受多个值并将每个值记录到控制台。我们可以仿照它的例子编写一个简单的 `sum` 方法。首先让我们看看 ES5 中的写法：

```javascript
// Snippet Example 5a
function sum() {
  var args = [].slice.call(arguments);
  return args.reduce(
    function(prev, cur) {
      return prev + cur;
    }, 0);
}
console.log(sum(1, 2, 3, 4, 5)); // 15
```

### 注意

本节的代码片段位于本书代码包中的 `snippets/02/ex5-variable-arguments/a`...`d`。使用交互式代码片段演练场，在第一个下拉菜单中选择 **2: ES2015 and Browserify**，在第二个下拉菜单中选择示例 **5a…5d**。

当然，这并不理想——并不能立即看出该方法可以接受任意数量的参数。ES2015 让我们可以通过以下方式显式地表明这一点：

```javascript
// Snippet Example 5b
function sum(...items) {
  return items.reduce((prev, cur) => (prev + cur), 0);
}
```

差别不大，所以让我们看一个稍微复杂点的例子，还是先看 ES5 版本：

```javascript
// Snippet Example 5c
function interpolate(str) {
  var args = [].slice.call(arguments, 1 /*[1]*/);
  return args.reduce(function(prev, cur, idx) {
    return prev.replace(new RegExp(":" + (idx + 1), "g"), cur);
  }, str);
}
console.log(interpolate("My name is :1 and I say :2",
  "Bob", "Hello"));
  // My name is Bob and I say Hello
```

和之前一样，这个函数签名没有表明 `interpolate` 接受多个参数。此外，我们不得不稍微修改解析所有额外参数的代码——注意我们在 `[1]` 处调用 `slice` 时加了 `1`。这是因为我们在执行插值时需要避免包含第一个命名参数。

在 ES2015 中，我们可以改写如下：

```javascript
// Snippet Example 5d
function interpolate(str, ...items) {
  return items.reduce((prev, cur, idx) => prev.replace(
    new RegExp(":" + (idx + 1), "g"), cur), str);
}
```

`…` 操作符在 ES2015 中还可以以另一种形式使用，通过这样调用 `interpolate`：

```javascript
console.log(interpolate("My name is :1 and I say :2", ...["Bob", "Hello"]));
```

诚然，当我们硬编码数组时看起来有点傻。但如果我们要在 ES5 中写这个，不得不使用 `interpolate.apply(null, ["...", "Bob", "Hello"])` 来实现相同功能。当数组被传递给你时，这证明非常有用；你可以轻松调用一个接受可变数量参数的函数。

## 解构赋值与命名参数

JavaScript 中一种相当典型的模式是从对象中提取值。例如，经常看到函数接受一个修改函数行为的 `options` 对象。在 ES5 中，这需要相当多的工作。请看下面的片段——它在 **DOM**（**文档对象模型**）中搜索匹配给定查询的任何元素：



## ES2015 特性

## 解构赋值

```
// Snippet Example 6b
function findElements(options) {
  var first = false,
      query,
      els;
  if (options !== undefined && typeof options === "object") {
    if (options.first !== undefined) {
      first = options.first;
    }
    if (options.query !== undefined) {
      query = options.query;
    }
  }
  els = [].slice.call(document.querySelectorAll(query));
  if (first === true) {
    els = [els[0]];
  }
  return els;
}
console.log(findElements({
  query: "a",
  first: true
}).map(function(o) {
    return o.textContent;
  }));
```

### 注意

本节的代码片段位于本书代码包中的 `snippets/02/ex6-destructuring/a`...`d`。使用交互式代码片段演练场，在第一个下拉菜单中选择 `2: ES2015 and Browserify`，在第二个下拉菜单中选择示例 `6a … 6d`。

前几行专门用于提供适当的默认值并从 `options` 对象中提取值。这主要是样板代码；但如果不正确，就会产生 bug。如果我们能完全避免这些代码，那就好多了。

在 ES2015 中，我们可以使用一个称为 `destructuring`（解构）的新特性轻松提取这些值。它让我们能够编写如下代码：

```
// Snippet Example 6a
let {a, b} = {
  a: 5,
  b: 10	
};
console.log(a, b); // 5 10
```

我们可以安全地这样做，即使对象不包含我们请求的键之一——值将直接是 `undefined`。或者，我们可以指定默认值：

```
let {a, b = 10} = {
  a: 5
}; // a = 5, b = 10
```

我们还可以深入对象内部，如下所示：

```
let {a: {b, c=10}} = {
  a: {
    b: 5
  }
};
console.log(b, c); // 5, 10, a is undefined
```

同样的情况也适用于数组，如下面的片段所示：

```
let [state, capitol] = ["Illinois", "Springfield"];
```

如果项目数量超出你的关注范围，你可以跳过它们：

```
let [a, , c] = [1, 2, 3];
console.log (a, c); // 1 3
```

如果你想收集所有剩余的值，也可以使用 `...` 运算符：

```
let [a, ...b] = [1, 2, 3];
console.log(a, b); // 1, [2, 3]

let {c, ...d} = {
  c: 5,
  d: 10,
  e: 15
}
console.log(c, d); // 5, {d: 10, e: 15}
```

将所有这些结合起来，我们可以用 ES2015 重写原来的代码片段，如下所示：

```
// Snippet Example 6c
function findElements({query, first = false}) {
  let els = [].slice.call(document.querySelectorAll(query));
  if (first === true) {
    els = [els[0]];
  }
  return els;
}
console.log(findElements({
  query: "a",
  first: true
}).map(o => o.textContent));
```

虽然这可能不是显而易见的，但这也给我们带来了另一个特性：命名参数。它不如其他支持命名参数的语言那样完美，但已经足够好了。这也意味着我们的代码更加自文档化了。

我们也可以利用这一点来模拟多返回值：

```
// Snippet Example 6d
function doSomethingThatErrors() {
  let error = 5,
    message = "Element could not be found";
  return {
    error,
    message
  };
}
let {error, message} = doSomethingThatErrors();
console.log(error, message); // 5 Element could not be found
```

## 字符串插值

已经为 JavaScript 开发了许多模板和插值库，但 ES2015 现在提供了内置的字符串插值。

### 注意

本节的代码片段位于本书代码包中的 `snippets/02/ex7-string-interpolation/a`。使用交互式代码片段演练场，在第一个下拉菜单中选择 `2: ES2015 and Browserify`，在第二个下拉菜单中选择示例 `7`。

如果你还记得，我们已经在 `Variable Arguments`（可变参数）一节中编写了一个名为 `interpolate` 的插值方法。例如，`interpolate("My name is :1", "Bob")` 将返回 `My name is Bob`。在 ES2015 中，我们可以通过以下方式实现相同的功能：

```
let name = "Bob",
    str = `My name is ${name}`;
```

对于大多数插值库来说，功能到此为止。但 ES2015 允许我们计算任意表达式：

```
let x = 2,
    y = 4,
    str = `X=${x},Y=${y},X+Y=${x + y}`; // X=2,Y=4,X+Y=6
```

### 注意

`Anything` 只要是表达式就会生效，包括访问对象属性和方法。

作为额外的福利，这种新语法还允许多行字符串：

```
let str = `SELECT * FROM
             FROM aTable`;
```

当然，所有空白字符都会被包含在内，这意味着字符串实际上变成了 `"SELECT *\n FROM aTable"`。如果间距很重要，你需要采取必要的步骤来去除空白字符。

你还可以定义自己的 `interpolations`（插值处理器），这非常有用。让我们考虑一个常见的安全问题——SQL 注入。例如，一些开发人员在构建 SQL 语句时有点马虎，如下例所示：

```
var sql = "SELECT * FROM customers WHERE customer_name = \"" + name + "\"";
```

不幸的是，通过提供精心设计的客户名称，可以向该语句中注入任意 SQL 代码。现在，在你认为 ES2015 插值已将你从安全问题中拯救出来之前，请小心。以下结构同样不安全：

```
let sql = `SELECT * FROM customers WHERE customer_name = ${name}`;
```

### 提示

`永远不要编写像前面那样的代码！永远不要！`

相反，大多数处理 SQL 命令的库允许你编写 `prepared statements`（预处理语句）。这些语句由两部分组成：一个 SQL 模板和一个变量数组，这些变量被插入到模板中的适当位置。这听起来很像插值，确实如此。区别在于这些库使用数据库自己的代码来正确处理值，从而使 SQL 注入成为不可能。

在实践中，它看起来像下面这样：

```
var preparedStatement = {
  sql = "SELECT * FROM customers WHERE customer_name = ?",
  binds: ["Bob"]
};
```

以这种方式编写 SQL 语句并不太难；但有了 ES2015，我们 `can` 编写自己的插值处理器，使编写预处理语句变得更加容易：

```
function sql(s, ...binds) {
  let len = binds.length;
  return {
    sql: s.map((val, idx) => (`${val}${idx < len ? "?" : ""}`))
      .join(""),
    binds
  };
}

let name = "Bob";
let preparedStatement = /*[1]*/ sql`SELECT * FROM customers WHERE customer_name = ${name}`;
console.log(JSON.stringify(preparedStatement, null, 2));
// Console outputs
// {
//   sql: SELECT * FROM customers WHERE customer_name= ?
//   binds: ["Bob"]
// }
```

请看前面代码中的 `[1]`。这是 ES2015 指示调用 `sql` 方法以插值字符串的方式。在内部，这会转换为 `sql(["SELECT * FROM CUSTOMERS W` `HERE customer_name = ", ""], ["Bob"]);`。

## Promise 与 ES2016 初体验

在 JavaScript 中，人们经常需要编写异步代码，`especially` 是在使用 Cordova 插件时，或者需要发起 `XMLHttpRequest` 时。这意味着你通常最终会编写如下代码：

```
// Snippet Example 8a
function slowDiv(a, b, callback) {
  setTimeout(function() {
    if (b === 0) {
      callback("Can't divide by zero");
    } else {
      callback(undefined, a / b);
    }
  }, 2500 + Math.random() * 10000);
}
slowDiv(10, 5, function(err, result) {
  if (err !== undefined) {
    console.log(err);
    return;
  }
  console.log(result);
});
```

### 注意

本节的代码片段位于本书代码包中的 `snippets/02/ex8-promises/a` 和 `snippets/02/ex8-promises/d`。使用交互式代码片段演练场，在第一个下拉菜单中选择 `2: ES2015 and Browserify`，在第二个下拉菜单中选择示例 `8a-8d`。

前面的示例实现了一个名为 `slowDiv` 的非常简单的异步方法，它在一段随机的短时间后返回除法运算的结果。如果除数为零，则返回错误。




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

Promise 的完整入门指南超出了本章范围。如果你想了解更多关于 Promise 的信息，请参阅 [`developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 和 [`www.promisejs.org`](https://www.promisejs.org)。

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

如果你想了解如何在 ES2015 中模拟 `await`，请参阅 [`davidwalsh.name/async-generators`](http://davidwalsh.name/async-generators)。

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




# 模块

本节我们将介绍的最后一个 ES2015 新特性是模块。在 JavaScript 中实现模块化的方法有很多，但 ES2015 将模块化直接引入了语言本身。语法非常简单；但如果你不熟悉模块，可能需要一段时间才能完全消化吸收。

在 ES2015 中，模块是一个独立的 JavaScript 文件，其中有一个或多个方法或变量标记了 `export`。这是一个新关键字，允许你指明某个值或方法应当可供程序的其他部分使用。你可以选择使用 `export default` 提供默认导出。

相应地，你的代码通过 `import` 来使用模块。你可以选择用 `import *` 导入模块导出的所有内容，也可以按需导入特定部分。

让我们看一个例子。首先，定义一个简单的模块：

```
// math.js
export function add(a, b) {
  return a + b;
}
export function sub(a, b) {
  return a - b;
}
```

我们可以在另一个 JavaScript 文件中使用该模块，如下所示：

```
// another.js
import * from "math.js";
// 上面这行等同于下面这行
// import {add, sub} from "math.js"
console.log(add(2, 5));
```

现在，假设我们想创建一个高级数学库，但希望将简单的方法直接暴露给使用者。我们可以直接从另一个文件导出方法和值，如下所示：

```
// advancedMath.js
export * from "math.js"
export function mul(a, b) {
  return a * b;
}
`

这些内容并没有什么惊天动地的——大多数开发者都在使用类似的模式，但将其内置到语言中确实很不错。此外，它确实提供了更多灵活性，因为你可以按需选择要导入和导出的内容。

遗憾的是，你需要一个提供该特性支持的打包工具——Babel 默认不支持。我们将在本章后面的 *使用 Browserify* 一节中介绍这一点。

## 更多信息

你已经阅读了大量信息，但这些对于理解本书其余部分的代码至关重要。请花些时间熟悉本章提供的信息。然后，我建议你阅读一些我们未涉及的其他特性。以下网站提供了良好的切入点：

*   [`www.2ality.com`](http://www.2ality.com)
*   [`babeljs.io/docs/learn-es2015/`](https://babeljs.io/docs/learn-es2015/)

### 提示

并非每个 ES2015 特性都被我们使用的转译器（即 Babel）支持。关于 Babel 支持哪些特性的具体信息，请参阅 [`kangax.github.io/compat-table/es6/`](http://kangax.github.io/compat-table/es6/)。

为了测试你对所有 ES2015 特性的理解，或者如果你只是想尝试一下，Babel 提供了一个优秀的沙箱，你可以在其中编写 ES2015 代码，查看转译结果，并验证代码是否正常工作。你可以在线访问 [`babeljs.io/repl/`](https://babeljs.io/repl/)。

极少有应用程序需要完全自定义的代码。大多数应用程序都能从模块化和代码复用中受益，重要的是要善用模块化，以便更高效地构建应用。

许多开发者（甚至一些公司）都患有一种常见的综合症：非我发明综合症。患者极其犹豫使用自己未创建的代码、算法或任何其他资产。这种综合症的成因显而易见——它确保了一个人不会因第三方代码而承担责任，不会因不当授权资产而被起诉，等等；但这也意味着必须不断重复造轮子。这不可避免地导致算法更多漏洞、安全性更低，因为每个人都在尝试完全从零开始构建应用。

如果你读过其他关于移动应用开发的书籍，很可能已经接触过模块化自己的代码。与其拥有一个包含*所有内容*的大型单体 JavaScript 文件，不如分离代码，使其更易于理解（并使每个关注点清晰分离）。

然而，将这一理念扩展到更广泛的 JavaScript 世界极其有用；那里有大量采用宽松许可证的开源模块，可供你作为应用程序的基础。这意味着必须担心妥善遵守许可要求，但也使你能够组装经过充分测试的构建模块，以此作为应用程序的基石。

Browserify ([`browserify.org`](http://browserify.org)) 是一个工具，允许你使用庞大的 Node.js 包仓库。许多包原样即可在浏览器中正常工作，但 Node.js 拥有不同的全局环境。Browserify 将这个 Node.js 环境垫片化到浏览器中，这意味着几乎任何 Node.js 包都可以在浏览器环境中工作。大多数包会在文档中说明如果不支持浏览器。

### 注意

Browserify 还用于将应用的所有依赖打包成一个或多个大文件。这在生产环境中很有用，此时你可能想提供一个包含所有代码的压缩文件。

此外，既然我们可以使用 Node.js 包，不妨也使用 Node.js 包管理器 `npm`。我们已经在用它安装开发依赖，如 Cordova、Gulp 等。但我们也可以用 `npm` 来管理应用程序的依赖。

你应该花点时间浏览 [`www.npmjs.com`](https://www.npmjs.com)。应该很明显，那里有*海量*资源。利用 `npm` 作为资源能极大地加速你自己的开发。

## 修改我们的 Gulp 配置

遗憾的是，要想将 Browserify 和 ES2015 结合使用，我们需要修改 Gulp 配置。以前我们直接调用 `babel` 来转换代码，现在得稍微花点心思；必须调用 `browserify`，然后由 `browserify` 自行调用 `babel`。这是一个微妙的区别，但很重要。

首先，安装新的开发依赖：

```
$ npm --save-dev install vinyl-source-stream@1.1.0 vinyl-buffer@1.1.0 browserify@11.2.0 babelify@6.3.0
$ npm --save install babel@5.8.23
$ npm --save-dev uninstall gulp-babel
```

### 注意

我们安装的前两个模块将允许我们将 `browserify` 的输出转换为 Gulp 可处理的格式。

然后，我们需要在配置文件 (`gulp/config.js`) 顶部添加一些配置：

```
assets: { ...,   
     code: {src: "www/js/app/index.js", dest: "www/js/app"}
}
```

接下来，我们需要修改 `gulp/tasks/copy-code.js` 文件。首先，在顶部添加额外的 require 语句：

```
var ...,
    babelify = require("babelify"),
    browserify = require("browserify"),
    buffer = require("vinyl-buffer"),
    source = require("vinyl-source-stream");
```

然后，修改 copyCode 使其如下所示：




# Browserify 和 ES2015 集成

```javascript
function copyCode() {
  var isRelease = (settings.BUILD_MODE === "release");
  return browserify(paths.makeFullPath(config.assets.code.src,
                    paths.SRC), { // [1]
    debug: !isRelease, // [2]
    standalone: "app"
  }).transform(babelify.configure({ // [3]
      stage: 0, // 允许实验性功能 // [4]
    }))
    .bundle()
    .on("error", function(error) {
      this.emit("end"); // [5]
    }).on("error", notify.onError(function(error) {
      return "BABEL: " + error.message; // [6]
    }))
    .pipe(source("app.js")) // [7]
    .pipe(buffer()) // [8]
    .pipe(cordovaTasks.performSubstitutions())
    .pipe(isRelease ? gutil.noop() : sourcemaps.init({
      loadMaps: true
    })) // 从 browserify 文件加载 map
    .pipe(isRelease ? uglify({
      preserveComments: "some"
    }) : gutil.noop())
    .pipe(isRelease ? gutil.noop() : sourcemaps.write())
    .pipe(gulp.dest(paths.makeFullPath(config.assets.code.dest, 
          paths.DEST)))
    .pipe(browserSync.stream());
}
```

前面代码的后半部分看起来应该很熟悉——这就是上一章的内容，但上半部分是全新的。让我们更详细地了解一下。

首先，我们没有使用 `gulp.src`，而是直接调用了 `browserify`（`[1]`）。在上一章中，我们让 Gulp 将所有 JavaScript 文件合并成一个文件。而 `browserify` 会通过从主文件开始跟踪依赖树，自动发现所有需要打包在一起的文件。那些不是依赖的文件会自动被排除在最终输出之外。仅这一特性就非常不错。

在调用 `browserify` 时，我们通过设置 `debug` 属性（`[2]`）让它创建 sourcemap。我们只在开发时需要这样做，所以这取决于我们传递给 Gulp 的 `--mode` 设置。

我们还希望在测试时能全局访问应用程序的状态。默认情况下，`browserify` 会将所有内容打包成一个整齐的包，但我们无法直接访问应用程序可能创建的对象。向 `browserify` 传递 `standalone:"app"` 允许我们指定一个全局变量（`app`），通过它可以访问整个应用。这对生产代码不是必需的；但在开发调试时绝对有帮助。

接下来，我们使用 `babelify`（`[3]`）转换代码。`browserify` 会自动为我们调用 `babel`。我们还传递了 `stage: 0`（`[4]`），以便使用 `await` 和 `async` 等 ES2016 特性。

还添加了几个错误处理器，以便在 `browserify` 转换代码时遇到问题能收到通知。第一个（`[5]`）结束流（如果你想让 Gulp 监视代码，这很重要），第二个（`[6]`）生成错误消息。

最后，我们进入了 Gulp 的领域——前两个 `pipe` 方法是我们将 Gulp 引入的地方（`[7],[8]`）。因为我们没有让 Gulp 查找特定文件，所以使用了之前安装的两个 `vinyl` 模块，将 `browserify` 的输出转换为 Gulp 可处理的形式。从这一点开始，我们的代码与第 1 章《任务自动化》中的代码完全相同。

## 引入 Node.js 包

引入 Node.js 包非常简单。首先，你需要从 `npm` 找到一个喜欢的包。在示例中，我们将添加一个名为 `yasmf-h` 的模块。它不是一个复杂的包，本质上是第 1 章《任务自动化》中 `index.js` 里那个小模板方法的增强版。

要安装该包，请输入以下命令：

```bash
$ npm --save install yasmf-h
```

接下来，我们需要修改 `src/www/js/app/index.js` 以便使用该包：

```javascript
import 'babel/polyfill'; // [1]
import h from 'yasmf-h'; // [2]

export var app = {
  start() {
    var n = h.el("div",
      h.el("ul",
        h.el("li", "Some information about this app..."),
        h.el("li", "App name: {{{NAME}}}"),
        h.el("li", "App version: {{{VERSION}}}")
      )
    );
    h.renderTo(n, document.querySelector("#demo"));
  }
};

document.addEventListener("deviceready", app.start, false);
```

这与第 1 章《任务自动化》中的内容没有本质区别。注意这两条 `import` 语句。第一行代码（`[1]`）导入支持 Babel 所需的 `polyfill`，Babel 是我们一直在使用的 ES2015 转译器。第二行（`[2]`）导入我们安装的 `npm` 模块的代码，并将其赋值给变量 `h`。

我们还需要稍微修改 `src/www/index.html` 文件，即删除一行。请看下面的内容：

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="cordova.js" type="text/javascript"></script>
    <script src="./js/app/app.js" type="text/javascript"></script>
  </head>
  <body>
    <p>This is static content..., but below is dynamic content.</p>
    <div id="demo"></div>
  </body>
</html>
```

一切都一样，只是我们删除了以下行：

```html
<script src="./js/lib/browser-polyfill.js" type="text/javascript"></script>
```

我们不再需要在 `index.html` 中引入 ES2015 polyfill，因为已经在 `index.js` 中导入了它。`index.html` 中引入的脚本越少越好。

此时，你可以使用 `gulp build` 重新构建项目以应用新更改。它将完全像以前一样运行，但现在你正在使用一个*你没写的*第三方模块！

当然，在使用你没写的模块时，有几件事非常重要：

*   始终检查许可证。`npm` 包通常相当宽松，但最好复查一下。我们引入的包采用 MIT 许可证，要求仅在输出中某处包含原始版权。因为许可证包含在包的代码中，所以会被传递到我们的输出代码中。但确保正确授权项目中包含的任何代码取决于你。你还应该在应用的显眼部分（如 **关于** 或 **设置** 部分）列出你使用的包。

*   安装前审查包。查看代码或文档，看看是否有无法绕过的重大问题。如果包有测试，更好——你可以验证包是否真正工作。

本章涵盖了相当多的内容——大多与 ES2015 带来的新语法和特性相关。此外，我们还介绍了如何导入 `npm` 模块并在自己的代码中使用它们。

在下一章中，我们将致力于使用 Sass 构建模块化 CSS 文件。我们还将看到如何将其集成到构建过程中。

虽然 JavaScript 对应用程序功能至关重要，但 CSS 对应用程序外观至关重要。虽然 CSS 非常强大，但大多数移动浏览器支持的版本（CSS3）缺少许多能让开发更轻松的特性。CSS 也往往冗长；很容易最终得到又长又乱、难以理解和维护的样式表。

Sass（语法上很棒的样式表）旨在改进 CSS3，使其更易于编写和维护。它增加了变量、插值、计算、混入、模块化等特性。我们可以利用 Sass 让 CSS 更易读、更易写。

### 注意

你可以访问 `http://sass-lang.com` 了解更多关于 Sass 的信息。

在本章中，我们将重点关注以下内容：

*   学习 Sass
*   将 Sass 与 Gulp 集成




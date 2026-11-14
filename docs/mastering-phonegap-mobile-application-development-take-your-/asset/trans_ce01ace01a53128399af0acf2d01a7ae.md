
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


# ES2015 特性

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


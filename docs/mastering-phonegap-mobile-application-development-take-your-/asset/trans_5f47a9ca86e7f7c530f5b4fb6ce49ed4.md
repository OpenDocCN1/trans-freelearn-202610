

## 使用 Treo 添加数据

Treo 也支持添加数据，但仅提供 `put` 方法，因为如果试图创建一个已存在的对象，通常不希望报错——相反，大多数开发者希望对象被更新。对于多个对象或操作，Treo 也支持 `batch` 方法。

但首先，需要获取对象存储。这是通过使用对象存储名称调用 `store` 来完成的。然后可以根据需要调用 `put` 或 `batch`。让我们看一个例子：

```javascript
// 示例代码片段 4b
// 假设定义与前一个示例相同
let definitionStore = db.store("definition");
definitionStore.batch(definitions)
  .then(() => console.log("All entries added"))
  .catch((err) => console.log("[DB] An error was encountered " + err));
```

此时，代码量没有实质性的不同，不同之处在于 Treo 允许我们在这种情况下使用 Promise，而不是原生示例中的事件处理机制。

检索之前存储的对象非常容易，前提是你知道键。如果你只知道来自索引的值，事情会变得稍微难一点（虽然也不算太难）。我们稍后会讨论后一种情况。

### 注意

本节中的代码片段位于本书代码包的 `snippets/07/ex5-get-objects/*` 中。使用交互式代码片段演练场时，请选择 **7: IndexedDB** 和示例 **5a** 与 **5b**。

## 检索对象

与存储对象一样，你首先需要获取一个事务。然而，因为我们只需要一个只读事务，所以这变得容易得多。请看下面的例子：

```javascript
// 示例代码片段 5a
let req = db.transaction(["definition"]).objectStore("definition")
            .get("02124272");
```

传递给 `get` 的值是 `wordNetRef` 编号，它是对象的键。如果你使用的是自动生成的键，你需要指定它们（当然，你可能不会随手就知道键是什么）。

一旦找到对象，将调用 `success` 事件。事实上，如果指定键不存在对象，也会调用此事件，因此这是一个你需要自行检查的条件。如果发生其他错误，将触发 `error`。

让我们完成这个例子：

```javascript
// 示例代码片段 5a，续
req.onerror = function (evt) {
  console.log("[DB] An error occurred: ", evt.target.error);
};
req.onsuccess = function (evt) {
  let definition = evt.target.result;
  if (!definition) {
    console.log("Couldn't find the desired entry.");
  } else {
    console.log(JSON.stringify(definition, null, 2));
}
};
```

当触发 `success` 时，实际对象存储在 `evt.target.result` 中。如果这是 `undefined`，则 IndexedDB 无法为请求的键找到对象。如果不是 `undefined`，则你可以继续对对象进行任何所需操作，例如将其显示给用户或执行额外计算。

在 Treo 中，过程非常相似：

```javascript
// 示例代码片段 5b
let definitionStore = db.store("definition");
definitionStore.get("02124272")
  .then((result) => 
    console.log(result ? JSON.stringify(result, null, 2)
                       : "Couldn't find entry."))
  .catch((err) => {
    console.log("[DB] An error was encountered "+ err);
});
```

同样，就像原生版本一样，`undefined` 结果意味着找不到该对象。

## 删除对象

删除对象与使用 `get` 检索它们非常相似，只是我们使用 `delete` 代替。

### 注意

本节中的代码片段位于本书代码包的 `snippets/07/ex6-delete-objects/*` 中。使用交互式代码片段演练场时，请选择 **7: IndexedDB** 和示例 **6a** 与 **6b**。

让我们看一个例子：

```javascript
// 示例代码片段 6a
let req = db.transaction(["definition"],"readwrite").objectStore("definition").delete("02124272");
req.onerror = function (evt) {
  console.log("[DB] An error occurred: ", evt.target.error);
};
req.onsuccess = function () {
  console.log("Deleted the entry.");
};
```

重要的是要注意，即使找不到要删除的对象，也会调用 `success` 事件。如果发生其他错误，将触发 `error`。

让我们看看 Treo 的版本：

```javascript
// 示例代码片段 6b
let definitionStore = db.store("definition");
definitionStore.del("02124272")
  .then(() => console.log("Entry deleted."))
  .catch((err) => {
    console.log("[DB] An error was encountered "+ err);
});
```

Treo 使用 `del` 而不是 `delete` 来删除条目，除此之外，它与检索对象非常相似。

## 使用游标和索引

到目前为止，我们只使用存储时的唯一键来检索数据。但我们也可以使用创建的索引来定位对象，而无需知道对象的唯一键。

### 注意

本节中的代码片段位于本书代码包的 `snippets/07/ex7-cursors-indexes/*` 中。使用交互式代码片段演练场时，请选择 **7: IndexedDB** 和示例 **7a** 与 **7b**。

为此，我们在创建事务后从对象存储请求一个 `index`。一旦拥有索引，我们就可以调用 `get` 来检索索引中的对象。让我们看一个例子：

```javascript
// 示例代码片段 7a
let req = db.transaction(["definition"]).objectStore("definition")
            .index("lemmas").get("cat");
req.onerror = ...;
req.onsuccess = function (evt) {
  let definition = evt.target.result;
  if (definition) {
    console.log(JSON.stringify(definition, null, 2));
  } else {
    console.log("Couldn't find entry.");
  }
};
```

如果你查看几节前的 `definitions` 数组，你会发现一个问题：如果一个索引值对应多个对象怎么办？答案是 `get` 不在乎：它返回找到的第一个。只要索引是唯一的，这没问题；但如果索引不唯一，你将难以检索所有对象。游标是解决此问题的一种方式。它们允许你迭代多个值。你可以通过在索引或对象存储上调用 `getCursor` 来获取游标。只要你的 `success` 处理程序在游标上调用 `continue`，游标就会为索引或对象存储中的每个项目调用 `success`。例如：

```javascript
// 示例代码片段 7a，续
let req = db.transaction(["definition"]).objectStore("definition")
            .index("lemmas").openCursor();
req.onerror = ...;
req.onsuccess = function (evt) {
  let cursor = evt.target.result;
  if (cursor) {
    console.log(`key: ${cursor.key}, value: 
                ${JSON.stringify(cursor.value, null, 2)}`);
    cursor.continue(); // 获取下一个对象
  } else {
    console.log("No more entries.");
  }
};
```

前面的示例现在将列出 `lemmas` 索引中每个对象的每个定义。只要有匹配的对象，`cursor` 就会被定义。键和值可以通过分别引用 `key` 和 `value` 属性来获取。一旦没有更多项目，`cursor` 将变为 `undefined`。这也意味着如果索引中没有任何条目，`cursor` 最初就会是 `undefined`。

虽然迭代索引或存储中的每个项目很有用，但通常这并不是我们想要的。通常，我们需要过滤结果。为此，我们需要创建一个范围来限制结果。

`IDBKeyRange` 提供了几种可用于创建范围的方法。根据你的需求，有几个选项可用：

*   `only(key)`：游标将仅返回与键完全匹配的值。
*   `lowerBound(key [,exclusive=false])`：游标将返回包括键在内及键之后的所有值。如果 `exclusive` 为 `true`，则键本身将被排除。
*   `upperBound(key [,exclusive=false])`：游标将返回键及键之前的所有值。如果 `exclusive` 为 `true`，则键本身将被排除。
*   `bound(lowerKey, upperKey[,lowerExclusive=false [,upperExclusive=false]])`：这是 `lowerBound` 和 `upperBound` 的组合。


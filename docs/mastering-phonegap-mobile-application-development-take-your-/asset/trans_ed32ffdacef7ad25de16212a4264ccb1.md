
# Web SQL 数据库和 Cordova SQLite 存储插件

## 存储限制

当使用浏览器提供的 Web SQL 数据库支持时，我们会受到存储限制的约束。为了确保网站或应用不会占用计算机上的所有空间，浏览器会强制执行数据库大小的配额限制。这通常从 `5 MB` 开始。大多数浏览器允许扩展至 `50 MB`。

浏览器对 Web SQL 数据库实施的存储限制对移动应用来说是个问题，因为 `5` 甚至 `50 MB` 可能不足以满足应用的需求。值得庆幸的是，有一种方法可以绕过这一限制，也能绕过该标准已废弃且未来可能从浏览器中移除的事实：使用第三方插件。

## 第三方插件

有几个与 SQLite 交互的第三方插件可用，但我们将使用的这个插件旨在实现与所用规范相同的 API。这意味着，通常情况下，你可以在桌面浏览器和移动浏览器上使用相同的代码。其他一些插件提供截然不同的 API，这也可以，但不利于跨平台的代码复用。

我们将使用的插件名为 `Cordova SQLite Storage`，可从 <https://github.com/litehelpers/Cordova-sqlite-storage> 获取。你可以使用以下命令将其添加到 Cordova 项目中：

```
$ cordova plugin add io.litehelpers.cordova.sqlite
```

### 注意

如果你需要分发预填充的数据库，可以使用该插件的另一个版本，地址为 <https://github.com/litehelpers/cordova-sqlite-ext>。

## 创建和打开数据库

使用 Web SQL 数据库或 Cordova SQLite 存储插件创建和打开数据库很简单，但两者之间有几个差异是你需要注意的。在介绍这些差异之前，让我们先了解一下基础知识。

### 注意

本节的代码片段位于本书代码包中的 `snippets/08/ex1-create-database/`。使用交互式代码片段演练场时，选择 `8: Web SQL Database` 和示例 `1`。

### Web SQL 数据库语法

使用 Web SQL 数据库时，你可以通过以下方式打开数据库：

```
let db = window.openDatabase(name, version, description, quota);
```

让我们逐一了解这些参数：

*   `name`：这是数据库的文件名。通常以 `.db` 扩展名结尾。此参数为必填。
*   `version`：Web SQL 数据库支持版本化数据库。从技术上讲这是一个字符串参数，所以你可以使用 `1.0` 甚至 `"one"`。不过，我的建议是始终使用 `1`，稍后我们会说明原因。
*   `description`：这是数据库的简短描述，但主要供你自己参考。
*   `quota`：此参数指定你认为数据库需要多大，单位为字节。通常请求 `1`、`2` 或 `5` 兆字节。如果以后需要扩大容量，使用更大的 quota 打开数据库将请求额外空间。请记住，quota 只是一个*请求*，而非保证；用户完全可以不授予任何存储权限。

### Cordova SQLite 存储插件语法

使用 Cordova SQLite 存储插件时，首选语法略有不同：

```
window.sqlitePlugin.openDatabase(
    {name, location, createFromLocation,
     androidDatabaseImplementation, androidLockWorkaround});
```

差别不小，对吧？*从技术上讲*，你也可以使用第一种语法（只需添加 `sqlitePlugin`），但这已不再是该插件的首选语法。这是因为首选语法支持更多额外功能，而且 version、description 和 quota 实际上并不受支持，这意味着在使用浏览器实现时，没有理由使用除 `1` 以外的任何值作为数据库版本。

大多数参数都是可选的，但让我们快速过一遍所有参数：

*   `name`：与另一个版本一样，这是数据库的名称。
*   `location`：这是一个可选参数，指定数据库实际存储的位置。这仅影响 iOS。位置如下：
    *   `0`：将数据库存储在 `Documents/` 中；iTunes 可见，且由 iCloud 备份。这也是默认位置。
    *   `1`：将数据库存储在 `Library/` 中。由 iCloud 备份，但 iTunes 不可见。
    *   `2`：将数据库存储在 `Library/LocalDatabase/` 中。不由 iCloud 备份，iTunes 也不可见。
*   `createFromLocation`：这允许你随应用分发预填充的数据库。你的 `www` 文件夹中与之同名的数据库将被复制到最终位置。如果该位置已存在数据库，则不会再次复制。这仅在使用 <https://github.com/litehelpers/cordova-sqlite-ext> 处的插件时有效。
    *   `0`：不使用预填充数据库；创建的数据库将为空
    *   `1`：使用预填充数据库创建数据库
*   `androidDatabaseImplementation`：指定 Android 上应使用哪种 SQLite 实现。默认情况下，插件使用自己的实现而非系统提供的实现，但这偶尔会遇到问题（特别是在使用 Crosswalk 项目或 Ionic 时）。
    *   `2`：数据库使用系统 SQLite 实现而非插件自己的实现。为获得最佳兼容性，请使用此设置。
*   `androidLockWorkaround`：默认情况下未启用，但某些 Android 版本在意外关闭应用时会出现数据库锁定或数据丢失问题。这仅在 `androidDatabaseImplementation` 为 `2` 时才会发生。
    *   `1`：每次事务后关闭数据库。这以性能为代价修复了锁定和数据丢失问题。如果你将 `androidDatabaseImplementation` 设为 `2`，最好也包含此设置。

## 抽象差异：WebSQLDB 类

由于标准和插件之间的差异，通常最好将这些差异抽象化。让我们创建一个类来实现这一点：

```
// 示例片段 "WebSQLDB 类"；位于
// snippets/08/WebSQL/index.js
class WebSQLDB {
    constructor({name, version = "1", description, 
                 quota = (5 * 1024 * 1024),
                 location = 0, createFromLocation = 0,
                 androidDatabaseImplementation = 2,  
                 androidLockWorkaround = 1} = {}) {
        this.db = null;
        this.name = name;
        this.version = version;
        this.description = description;
        this.quota = quota;
        this.plugin = false;
        this.supportsReadOnlyTransactions = false;

        if (window.sqlitePlugin) {
            this.plugin = true;
            this.db = window.sqlitePlugin.openDatabase(
                {name, location, createFromLocation,
                 androidDatabaseImplementation,
                 androidLockWorkaround});
        } else {
            this.db = window.openDatabase(name, version, 
                description, quota);
        }

        if (!this.db) { // [1]
            throw new Error("Could not instantiate a database.");
        }

        if (this.db.readTransaction) { // [2]
            this.supportsReadOnlyTransactions = true;
        }
    }
}
```

请注意，前面的代码使用了大量默认参数。这些是建议的典型值，将它们设为默认值是合理的。如果你的应用需要不同的默认值，你随时可以将那些值设为默认值。

另外，请注意，我们只需检查返回值 `[1]` 即可判断打开数据库是否成功。如果它未设置为对象，则表示操作失败，我们可以适当地报错。最后几行 `[2]` 只是检查环境是否支持只读事务，我们将在下一节中深入探讨。

### 注意

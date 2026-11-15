

## Azure 表 API 集成

1. 首先，在 Azure 门户中创建表 API 并复制连接字符串。将其导出为环境变量。在基于 Linux 的系统中，可以运行此命令将连接字符串导出为环境变量：

```
export.AZURE_STORAGE_CONNECTION_STRING = <Your connection string>
```

2. 接下来，安装所需的依赖项。命令如下：

```
npm install azure-storage
```

3. 安装完成后，您可以在代码库中引入该模块并开始使用 `Table storage`，例如：

```
var azure = require('azure-storage');
var tableObject = azure.createTableService();
tableObject.createTableIfNotExists('demoTable', function(error, result, response) {
  if (!error) {
    // 表已存在或已创建
  }
  console.log(result);
});
```

这将使用连接字符串连接到您的 Azure 表存储。接下来，让我们继续将 Graph API 与 Node.js 集成。

# 将 Graph API 与 Node.js 集成

Cosmos DB 通过 `gremlin` 支持图数据库，而 `gremlin` 在 Node.js 中已支持很长时间。

我们可以使用以下命令安装 `gremlin` Node 模块：

```
npm install gremlin@latest
```

在生产环境中始终锁定依赖版本。

您可以在 Azure 门户中创建一个新的 Graph API 并复制连接详细信息。我将展示连接到该图数据库的示例代码。

我们已经安装了 Node 模块。让我们将配置详细信息放在一个文件中：

```
var config = {};

config.endpoint = "graph api end point";
config.primaryKey = "primary key";
config.database = "database name";
config.collection = "collection name";

module.exports = config;
```

现在，我们可以使用以下代码连接到 `Graph DB`：

```
var Gremlin = require('gremlin');
var config = require("./config");

var client = Gremlin.createClient(
    443,
    config.endpoint,
    {
        "session": false,
        "ssl": true,
        "user": `/dbs/${config.database}/colls/${config.collection}`,
        "password": config.primaryKey
    }
);
```

您可以使用 `node <filename.js>` 运行代码，如果连接字符串错误，它将抛出错误并退出。

您可以使用 `execute(query)` 函数在图数据库中执行查询，例如：

这段代码将返回图中顶点的数量：

```
client.execute("g.V().count()", {}, (err, results) => {
  if (err) return console.error(err);
  console.log("Result: %sn", JSON.stringify(results));
});
```

同样，您也可以执行其他查询。

这也涵盖了 Node.js 与 Graph DB 的集成。

# 总结

在本章中，我们涵盖了大量内容。我们使用现有代码库与著名的 MongoDB 进行了集成，以证明关于迁移或从传统 MongoDB 迁移到托管 MongoDB 的观点，且无需更改任何代码（当然，除了配置 URI）。

我们还学习了将 Node.js 与 `DocumentDB`、`Table` 和 `Graph DB` 集成。

在下一章中，我们将使用 Node.js 和 Cosmos DB 构建一个高性能链接编码和重定向服务。这类似于 `bit.ly` 服务。

# 使用 Cosmos DB 构建高性能链接缩短与追踪系统

Cosmos DB 是为其服务提供 SLA 的独特数据库之一。这种高性能系统对于实时运行的应用程序至关重要，例如群聊、实时视频通话或链接缩短服务。

在本章中，我们将使用 Cosmos DB 构建一个链接缩短服务。我们将在后续章节讨论选择哪种 API。我们将使用 Node.js 进行应用程序开发。

在本章中，我们将学习以下内容：

- 为什么需要 URL 缩短？
- 系统需求
- 容量估算
- 数据库设计
- URL 编码算法
- Node 应用开发
- 安全性与权限

## 为什么需要 URL 缩短？

URL 缩短用于为长 URL 创建较短的别名。当用户访问这些短 URL 时，会被重定向到原始 URL。无论何时使用，任何 URL 的较短版本都能节省大量空间，例如在打印或发推文时，因为推文有字符限制。

例如，如果我们通过 TinyURL 缩短此页面：

[https://packtpub.com](https://packtpub.com)

在这种情况下，我们将得到 [http://tinyurl.com/jlg8zpc](http://tinyurl.com/jlg8zpc)。

缩短后的 URL 尺寸几乎是实际 URL 的 1/3。

URL 缩短用于跨设备优化链接、追踪单个链接以分析受众和广告系列效果、隐藏联属原始 URL 等。

如果您以前没有使用过 [tinyurl.com](http://tinyurl.com/)，请尝试创建一个新的缩短 URL，并花些时间浏览其服务提供的不同选项。这将极大地帮助您更好地理解本章。

您也可以使用 [bit.ly](https://bitly.com/) 来更好地理解该概念。

让我们看看系统施加的需求。

## 系统需求

我们的 URL 缩短系统应满足特定需求。

以下是功能性需求：

- 给定一个 URL，我们的服务应生成其较短且唯一的别名
- 当用户访问较短的 URL 时，我们的服务应将他们重定向到原始链接

以下是非功能性需求：

- 系统应具有高可用性。这是必需的，因为如果我们的服务宕机，所有 URL 重定向都将开始失败。
- URL 重定向应实时发生，且延迟最小。
- 缩短的链接不应可被猜测（或预测）。

以下是扩展需求：

- 分析，例如，重定向发生了多少次？
- 我们的服务还应可通过 REST API 供其他服务访问

让我们计算系统上线所需的数据库大小和带宽。

## 容量估算

我们的系统将以读为主；与新的 URL 缩短相比，会有大量的重定向请求。假设读写比率为 100:1。

**流量估算**：如果我们假设每月会有 *5 亿* 个新的 URL 缩短，则在同一时期内预计会有 (*100 \* 5 亿 => 500 亿*) 次重定向。我们系统的**每秒查询率**（**QPS**）会是多少？

每秒新 URL 缩短数：

*5 亿 / (30 天 \* 24 小时 \* 3600 秒) ~= 200 URLs/s*

每秒 URL 重定向数：

*500 亿 / (30 天 \* 24 小时 \* 3600 秒) ~= 19K/s*

**存储估算**：既然我们预计每月有 5 亿个新 URL，如果我们保留这些对象五年，我们将存储的对象总数将达到 300 亿：

*5 亿 \* 5 年 \* 12 个月 = 300 亿*

假设我们存储的每个对象为 500 字节（仅为大概数字，稍后我们会深入探讨）；我们将需要 15 TB 的总存储空间：

*300 亿 \* 500 字节 = 15 TB*

**带宽估算**：对于写请求，因为我们每秒预计有 200 个新 URL，我们服务的总传入数据将为 100 KB/秒：

*200 \* 500 字节 = 100 KB/s*

对于读请求，因为我们每秒预计约有 19K 次 URL 重定向，我们服务的总传出数据将为 9 MB/秒：

*19K \* 500 字节 ~= 9 MB/s*

**内存估算**：如果我们想缓存一些频繁访问的热门 URL，需要多少内存来存储它们？如果我们遵循二八定律，即 20% 的 URL 产生 80% 的流量，我们希望缓存这 20% 的热门 URL。

因为我们每秒有 19K 个请求，每天将获得 17 亿个请求：

*19K \* 3600 秒 \* 24 小时 ~= 17 亿*

要缓存这些请求的 20%，我们需要 170 GB 内存：

*0.2 \* 17 亿 \* 500 字节 ~= 170 GB*

## 数据库设计

关于我们将要存储的数据性质，有几点观察：


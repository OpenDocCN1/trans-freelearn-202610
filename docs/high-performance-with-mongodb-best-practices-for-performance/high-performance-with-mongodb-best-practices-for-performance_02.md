# 2 为性能设计的模式设计
## 为性能设计的模式设计

MongoDB 的创建是为了弥合快速、水平可扩展的键值存储与传统关系型数据库之间的差距。要获得最佳性能，你需要了解 MongoDB 的优势并设计应用程序以利用这些优势。无论你处理的是大量读取、写入还是两者兼有，目标都是在可读性、可维护性和速度之间取得平衡。MongoDB 灵活的模式设计提供了独特的机会，但需要深思熟虑的规划以避免常见陷阱。

在本章中，我们将介绍为高性能设计 MongoDB 模式的基本原则。我们将首先介绍 MongoDB 中模式设计的关键原则并澄清一些常见误解。然后，我们将探讨 MongoDB 的文档模型如何使构建灵活且高效的模式变得更容易。之后，我们将回顾可能损害性能或使数据更难管理的常见错误和反模式。最后，我们将分享实用的模式设计模式，帮助你优化读取和写入。为了将所有内容整合起来，我们将把这些想法应用到示例应用中，以便你可以看到它们在实践中的运作方式。

本章将涵盖以下主题：

- 模式设计的核心理念
- MongoDB 模式模型的优势
- 使用模式验证确保数据完整性
- 按优势组织的模式设计模式

### 了解模式设计的核心理念

MongoDB 中的模式设计由应用程序如何访问和修改数据驱动；没有单一的 *正确* 结构适用于每种用例。这与关系模型不同，在关系模型中，规范化是默认的起点，而非规范化是性能调优步骤。在 MongoDB 中，你首先了解工作负载，然后设计模式以匹配。

在本节中，我们将介绍诸如数据共置、读写权衡、文档大小和常见陷阱等关键原则。这些准则将帮助你构建既高效又与应用程序行为一致的数据模型。

### 没有唯一正确的方式

这里是关于模式设计的最重要的一点，你可能没有意识到：*没有单一正确的方式来构建你的文档。* 是的，你没看错！每个用例都需要读取和写入效率之间的独特平衡。关键是首先考虑应用程序的工作负载；*经常一起检索的数据应存储在一起以避免昂贵的查找。*

这与你可能学到的在关系数据库中管理数据的方式根本不同。在该模型中，数据规范化是自始即被期望的做法。只有当性能无法跟上完全规范化的数据模型时，你才会根据数据的用途选择性地对部分数据进行非规范化。在 MongoDB 中，我们从问题，*这些数据将如何被使用？* 开始，并创建最适合我们访问模式的数据模型。

想想城市规划。一些城镇将住宅、商业和工业区隔离到不同的区域。但在设计良好的城市中，区域并非严格按类型组织，而是作为社区，居民的日常必需品触手可及。同样，MongoDB 赋予你根据应用程序实际使用方式组织数据的灵活性，而非按任意分类。

### 数据共置

一旦你确定了应用程序将处理哪些实体，主要决策就是是否嵌入相关数据或将其单独存储并引用。你会反复问自己一个对象是否是一个应独立存储在自己的集合中的独立实体，还是另一个对象的属性，应作为该对象的一部分存储。同样，对于对象的每个属性，你需要考虑是否直接嵌入它或单独存储。

这就是 *一起使用的数据应存储在一起* 原则发挥作用的地方。乍一看它可能听起来很简单，但在理解其影响时它非常强大。以下是它在实践中的体现：

- **嵌入始终一起检索的数据**：这减少了数据库往返次数并保持读取高效。例如，如果订单行项目通常一起访问，则将订单行项目嵌入订单文档是有意义的。
- **分离不常访问的数据**：将大但很少使用的字段保留在主文档之外。这有助于防止大多数查询不需要它们时的开销。将仅偶尔需要的较大属性存储在文档中会增加磁盘 I/O 并浪费 RAM。例如，如果大多数用户页面只显示小缩略图，则在主用户文档中存储用户的全尺寸图像是不高效的。

最糟糕的错误可能是将 MongoDB 当作关系数据库对待，将数据在多个集合中规范化，然后为每个查询重新连接。另一方面，如果过度嵌入导致文档膨胀到极端程度，也可能不是最优的。你最终需要的缓存、**每秒输入/输出操作数（IOPS）** 和网络带宽将显著高于按应用程序需求更合理地构建数据的情况。

### 读写权衡

在设计模式时，你可能会面临优化读取与写入之间的冲突需求。这并非 MongoDB 独有，但文档模型为解决这些冲突提供了更多选项。以下是平衡模式设计中读取和写入性能时需要考虑的几个常见因素：

- **索引**：索引加快读取速度但会增加写入开销并使用其他资源。在集合中添加多个索引可以显著提高查询性能，但会增加写入操作的成本。这是一个经典的权衡，但如果索引为你的应用程序正确设计，则值得这样做。
- **严格的服务等级协议（SLA）**：优先考虑延迟要求最严格的操作。有时你优化写入，有时优化读取，很少两者同等优化。例如，订单处理系统可能优先考虑快速写入，而报表系统可能优化读取效率。
- **减少 I/O**：努力最小化数据读取和写入量。你访问的每个数据块都有成本。如果能消除不必要的读取或写入，所有操作都会变得更便宜、更快。

这些考虑因素也会影响是否使用大文档或小文档建模数据，这会影响应用程序读取、写入和更新数据的效率。

### 小文档与大文档

了解 MongoDB 如何在磁盘上存储文档对于优化性能至关重要。MongoDB 的 WiredTiger 存储引擎，在第 7 章《存储引擎》中更详细地介绍，使用面向页面的架构，其中文档存储在按需加载到内存的页面上。

这就是原因：

- **最小页面大小：** WiredTiger 的最小未压缩页面大小为 32 KB。小于此阈值（未压缩小于 32 KB）的文档与其他文档共享一个页面，允许在一次页面读取中检索多个小文档。
- **相关文档碎片化：** 如果相关数据分散在多个文档中，你的应用程序最终需要发出多个查询来检索完整的数据集。此外，这些文档可能位于不同的页面上，需要更多 I/O 操作来检索所有相关数据。
- **写入放大：** 当大文档被更新时，即使只有小的更改，在检查点期间整个文档仍必须写入磁盘。文档越大，就越可能导致不必要的更大写入。

理解这一点引出了我们下一个重要原则：*高读取速度的工作负载受益于嵌入式文档模式。* 这是因为相关数据存储在一个地方，一个页面中。另一方面，对于高写入速度的工作负载，即频繁更新数据的小部分，多文档模式可能更高效。

### 常见误区

让我们打破一些关于 MongoDB 模式设计的常见误解：

- **非规范化：** 对数据进行非规范化并不总是意味着 *更多存储* 或 *数据重复*。在某些情况下，将相关数据嵌入一处会降低总体空间使用并减少所需索引的数量。例如，将行项目嵌入订单文档中消除了在订单和行项目集合中都需要订单 ID 索引的需求，并有助于减少总数据大小。嵌入很少或从不更改的频繁访问的领域数据也会减少额外的查找并提高整体性能。



-   **动态模式：** MongoDB 提供灵活性，但你不必使用它。你可以通过应用层或通过 MongoDB 自身的模式验证功能，甚至两者兼用，来强制执行严格的模式。灵活的模式是一个选项，而不是要求。
-   **有意设计：** 如果你没有有意识地设计数据结构，那它就不是真正的模式。有人把 JSON 传给你直接存入数据库，这不是模式设计；这只是存储负载。真正的模式设计反映了应用程序的具体需求和要求。
-   **索引并非自动：** 这并不是一个常见的误解，但确实存在。每个集合都会自动在必需的 _id 字段上创建唯一索引，该字段是每条记录的主键，如果你在插入文档时未提供它，它将被自动生成。但模式设计的重要部分是确定你将通过哪些次要属性进行查询，并确保创建所有必要的索引以高效支持这些查询。你将在 *第 3 章，索引* 中了解更多关于索引的内容。

还记得关系型数据库从业者会争论 *是否每个表都需要代理键或自然键？* 吗？现在，我们有了更有趣的讨论，关于 *是否嵌入或引用相关数据*，这些讨论直接影响应用程序性能！

## MongoDB 模式设计的关键优势

与关系型数据库相比，MongoDB 的文档模型支持简化数据访问、降低查询复杂度、提升读写密集型工作负载的性能，并更易适应不断变化的应用需求的模式。MongoDB 不强制执行僵化的结构，而是允许开发者以反映真实用例和访问模式的方式建模数据。这种灵活性是 MongoDB 模式设计方法的核心，使开发者能够构建针对速度和可扩展性优化的应用程序。本节将介绍 MongoDB 模式设计的关键优势，包括处理一对多关系、嵌入弱实体、支持动态属性、利用缓存和快照、针对常见用例进行优化，以及实现无缝的模式演进。

## 一对多关系

MongoDB 中的一对多关系被视为一等公民，数组、对象和对象数组都享有完整的查询、更新和索引功能。这种能力深植于 MongoDB 文档模型的基础之中，而非事后添加。

这使得 MongoDB 非常适合存储结构化、重复的数据，例如博客文章中的评论、产品的标签或商品的变体。例如，在博客平台中，博客文章文档可以将其评论包含在数组中，使应用程序能够通过一次查询检索文章及其评论。通过将相关数据存储在一起，MongoDB 消除了昂贵的连接需求，降低了查询复杂度，并最小化了磁盘 I/O 操作，从而提升整体性能。

## 嵌入弱实体

弱实体（依赖父实体提供上下文的项目）是嵌入到 MongoDB 中的理想候选。例如，电子商务平台中的订单明细行固有地依赖于其父订单。将这些明细行嵌入到订单文档中可确保所有相关数据一起存储，无需单独查找或连接。没有明细行的订单价值不大，而将它们保持在一起可使开发者高效地查询和更新订单数据。这种方法通过减少 I/O 操作和查询复杂度来提升性能，同时使数据检索更快并简化应用程序逻辑。

## 动态属性

关系型数据库在建模字段名称或属性数量事先未知时常遇到困难。这些挑战通常涉及添加新列的昂贵模式迁移，或需要以笨重、效率低下格式存储属性。MongoDB 则原生拥抱动态数据建模。例如，产品目录可以包含可变属性，如服装的“颜色”或车辆的“发动机尺寸”，而无需僵化的定义或结构更改。文档可以轻松地将属性名称作为键存储并对应值，或使用键值对数组，两者都支持高效的索引和查询。这消除了运营开销，并确保即使数据演进时查询也更快。

## 缓存和快照

在传统数据库中，数据规范化会阻止冗余，但 MongoDB 允许选择性重复以优化频繁访问模式。例如，将客户的收货地址直接存储在每个订单文档中可确保快速检索此信息，而无需查询单独的集合。同样，将用户最近的少量操作，如帖子或评论，存储在其父文档中可减少重复查询并加快对频繁请求数据的访问。这种数据重复的使用可作为有效的缓存或时间点快照，减少 I/O 开销，并确保在高用户流量的应用程序中查询更快。

## 常见用例的优化

前面提到的一些示例允许你针对每分钟发生数千次的操作优化系统，例如用户查看其订单或朋友的帖子，同时让你不必担心可能根本不会发生或极少发生的“百年难遇”的操作（用户多久更改一次姓名，该操作真的需要即时完成吗？）。这种优化使开发者能够专注于关键工作流程，例如检索用户帖子或订单详情，同时降低罕见操作的优先级，例如用户更改账户信息。MongoDB 的模式灵活性使开发者能够专注于优化这些频繁操作，减少不必要的处理开销，并确保日常交易平稳、快速的性能。

## 模式演进

你是否曾尝试在生产环境中向大型关系型数据库表添加列？借助 MongoDB 的动态模式，你可以立即开始使用新字段，同时根据需求懒惰地更新旧文档或根本不更新。应用程序应优雅地处理文档中的新字段或新形状。这可以确保向后兼容，并在模式更新期间实现平稳过渡。MongoDB 应用程序中的良好数据访问层理解模式可以随时间演进，而无需进行完整的数据转换。

MongoDB 的模式设计优势为现代应用程序提供灵活性和效率，使开发者能够构建量身定制的数据模型以最大化现实世界的性能。无论是处理一对多关系、嵌入弱实体、支持动态属性、利用缓存和快照、优化高频操作，还是适应模式演进，MongoDB 都使开发者能够创建适应变化需求的高性能应用程序。通过简化查询、降低延迟和简化数据访问模式，MongoDB 提供了可扩展、现代工作负载所需的强大功能和多功能性。其文档模型不仅仅是一种便利；它是现实世界应用程序性能优化的基石。

## 模式验证

虽然大部分模式设计关注数据如何被结构化和访问，但一个经常被忽视却至关重要的方面是模式验证。仅仅因为 MongoDB 灵活并不意味着你应该允许任何数据结构被存储。模式验证是维护数据完整性的重要工具，且在性能影响方面几乎是免费的。在插入或更新时验证文档的性能成本与写入的总成本相比可以忽略不计，并且它提供了几个实际好处：

-   **提前捕获错误：** 验证可防止不正确的或格式错误的数据进入数据库
-   **强制执行业务规则：** 你可以直接在数据模型中定义诸如最小值或有效模式等要求
-   **记录你的模式：** 验证规则可作为数据模型的实时文档
-   **平滑演进：** 随着模式的演进，你可以轻松调整验证规则

虽然 MongoDB 提供了极大的灵活性，但模式验证可确保这种灵活性不会以数据完整性为代价。通过为数据模型定义明确的规则，你不仅可以防止错误并强制执行业务逻辑，还为可维护、不断演进的模式奠定了基础。现在让我们来看常见的模式设计错误，其中许多错误可以通过深思熟虑的验证以及对数据结构和用例模式的透彻理解来避免。

## 常见的模式设计错误

MongoDB 灵活的文档模型在谨慎使用时提供了强大的优势。同样能够实现高性能和开发敏捷性的特性，如果应用不当，也可能导致效率低下或瓶颈。常见的模式设计错误会削弱性能、增加复杂性，或抵消 MongoDB 模型的优势。了解这些陷阱将帮助你做出更明智的设计选择，避免日后进行昂贵的重写。

## 过度规范化

这通常发生在具有关系数据库背景的开发人员将相同的规范化原则应用于 MongoDB 时。以下是你可能过度规范化的一些迹象：

-   你的代码频繁使用 $lookup 操作（MongoDB 的 JOIN 版本）
-   你有多个集合表示通常一起访问的实体
-   读查询需要从多个集合中拼凑数据
-   你需要多文档事务来更新存储在单独文档中的相关字段，而这些字段本可以存储在一起并在无需事务的情况下以原子方式更新

如果你在 MongoDB 中的集合数量与关系数据库中相同，那么你很可能过度规范化了你的模式。

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_73_0.png)

## 过度规范化的代价

我正在审查一个应用程序，开发人员为用户偏好创建了一个单独的集合，为用户资料创建了另一个，为用户设置创建了另一个……你明白了。每次用户页面加载都需要七次不同的查询！我们将经常访问的字段合并到单个用户文档中，页面加载时间从 600 ms 降至 100 ms 以下。

## 过度嵌入

这是相反的问题：当开发人员对 MongoDB 的文档模型感到兴奋并试图将太多内容放入单个文档中时。以下是你可能过度嵌入的一些迹象：

-   文档大小不断增长且无限制
-   你有非常大的文档
-   你在经常访问的文档中存储了很少访问的数据
-   你有无限增长的数组（如热门帖子的评论）
-   编写查询以仅返回文档的特定部分变得复杂
-   对深度嵌入字段的更新需要复杂的更新操作符
-   为深度嵌套字段创建有效索引变得具有挑战性

如果你的文档接近 16 MB 的文档大小限制，那就是你必须重新考虑模式设计的强烈迹象。

## 其他常见反模式

除了过度规范化和过度嵌入之外，还有几种模式设计模式经常导致问题，我们称之为反模式。识别并避免这些陷阱可以帮助你构建更高效、可维护和可扩展的应用程序：

-   **对所有操作都需要事务：** 设计始终需要多文档事务的模式会抵消文档模型的优势。通过适当的嵌入，大多数操作可以在没有事务的情况下实现原子性。
-   **糟糕的索引策略：** 不知道如何为查询模式正确索引会导致全集合扫描和性能低下。
-   **对嵌入数据的复杂更新：** 如果你无法轻松更新深度嵌套字段，你的模式可能过于复杂或过度嵌入。
-   **查询效率低下：** 如果你无法轻松查询文档中仅相关的部分，请重新考虑你的模式设计。
-   **将 MongoDB 当作关系数据库对待：** 将相关数据分散到各个集合中，并在每次查询时使用 $lookup 重新连接，会抵消文档模型的优势。过度使用 $lookup 会减慢查询速度，可以通过战略性嵌入或反规范化来避免。
-   **无界数组增长：** 嵌入可能无限增长的数组（如病毒式帖子的评论）可能导致文档大小问题和性能低下。
-   **忽略写入模式：** 在写入密集型应用程序中仅优化读取可能导致争用和性能瓶颈。
-   **过度索引：** 只是以防万一在每个字段上创建索引会增加写入开销，并不一定能提高查询性能。
-   **在文档中存储大型二进制数据：** 虽然 MongoDB 可以存储二进制数据，但非常大的文件（>16 MB）应使用 GridFS 或外部存储，并在文档中引用。

现在你已经熟悉模式设计中的常见陷阱，让我们专注于优化读取、写入、查询、存储等效率的模式。

## 按收益划分的模式设计模式

现在我们已经介绍了基础知识，让我们探索一套实用的模式设计模式，每种模式都按其提供的特定收益组织，例如提高读取性能、写入效率或查询灵活性。这些模式不仅仅是理论性的；它们基于真实世界的用例和常见挑战。

在本节中，我们将使用名为 **Socialite** 的社交媒体应用程序的示例来说明模式设计模式。Socialite 包含三个主要数据存储：

-   **内容存储：** 包含帖子、媒体和评论
-   **用户图：** 管理用户以及用户之间的关系（关注者、正在关注）
-   **时间线缓存：** 针对用户登录时快速显示相关内容进行了优化

这种分离使 Socialite 能够针对其特定的访问模式优化每个存储。当我们探索不同的模式设计模式时，我们将参考这些组件来展示一些实际应用。但接下来，让我们讨论一些 MongoDB 特定的模式优势。

## 读取性能优化模式

在本节中，我们将查看有利于优化读取性能的模式设计模式。这些包括嵌入模式，允许经常访问的相关数据一起存储；扩展引用模式，在灵活性和性能之间平衡嵌入和引用；以及子集模式，有助于仅检索文档中最相关的部分，而无需加载不必要的数据。

## 嵌入模式

在 MongoDB 中，嵌入模式涉及将相关数据包含在同一文档中，而不是对其进行规范化并从另一个集合中引用它。在下面的示例中，该模式用于 users 集合中的示例文档，以存储分层和相关信息：

```
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  username: "jane_smith",
  name: "Jane Smith",
  email: "jane.smith@example.com",
  profile: {
    bio: "Travel enthusiast and photographer",
    location: "San Francisco, CA",
    birthdate: ISODate("1988-04-12T00:00:00Z"),
    avatar: "https://socialite.com/images/avatars/jane_smith.jpg"
  },
  preferences: {
    theme: "dark",
    emailNotifications: true,
    privacySettings: {
      profileVisibility: "friends",
      activityStatus: "everyone"
    }
  }
}
```

让我们回顾一下这份文档，以了解嵌入模式是如何在此应用的：

-   **作为子文档的个人资料数据：** profile 字段作为子文档嵌入，包含 bio、location、birthdate 和 avatar 等详细信息。不是将此信息存储在单独的集合中并引用它，所有用户个人资料相关数据都作为一个连贯的单元一起存在。这使数据保持本地化，以便快速访问和检索。
-   **作为子文档的偏好数据：** 同样，preferences 字段包含用户特定的设置，例如 theme、emailNotifications 和嵌套的 privacySettings。通过将这些偏好直接嵌入到主文档中，它避免了单独集合和连接的复杂性，使得在一个操作中查询和更新用户设置变得更容易。
-   **嵌套的隐私设置：** privacySettings 是 preferences 中的一个子文档，它存储 profileVisibility 和 activityStatus，展示了更深层次的嵌入。这显示了 MongoDB 在保持逻辑结构的同时创建多级嵌入的灵活性。

通过对 profile 和 preferences 子文档使用嵌入模式，该设计实现了数据反规范化，减少了对昂贵连接的需求，并确保了数据本地化，以优化读取和写入性能。该模式在相关数据经常一起访问的场景中效果良好，消除了对多个查询的需求。

## 扩展引用模式

当我们在 MongoDB 中需要引用相关集合中的另一个文档，同时在查询时为了方便和性能而包含其某些关键字段时，就会使用扩展引用模式。这种方法有助于避免不必要的查找，尤其是在经常访问的字段 readily available 时。

以下示例是 posts 集合中的一个文档，其中包含对作者和作者相关数据的引用：

```
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d6f47"),
  content: "Just visited the Golden Gate Bridge! #travels #sanfrancisco",
  postDate: ISODate("2023-04-07T09:15:30Z"),
  media: ["https://socialite.com/images/posts/golden_gate.jpg"],
  likes: 42,
  comments: 7,
  author: {

```

```
_id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),  // Reference to complete user
user
  username: "jane_smith",    // Duplicated for display
  name: "Jane Smith",        // Duplicated for display
  avatar: "https://socialite.com/images/avatars/jane_smith.jpg" // Duplicated for display
  }
}
```

在此示例中，posts 文档通过包含作者子文档利用了扩展引用模式：

- **嵌入式引用：** 作者子文档包含一个 _id 字段，它是对存储在 users 集合中其他位置的完整 Author/User 文档的引用。这使得可以将帖子链接回其作者。
- **用于显示的重复关键字段：** 而不是仅存储用户的 _id 值，posts 文档中直接包含了 username、name 和 avatar 等额外的作者相关详细信息。这些字段是从引用的 users 文档中复制的，以提高性能并允许在无需执行额外查询或查找操作的情况下显示作者详细信息。
- **优化的读取操作：** 通过嵌入 username、name 和 avatar，posts 文档可确保常见查询（例如显示帖子内容以及作者的基本信息）能够在无需额外数据库调用的情况下无缝处理。这降低了延迟并提高了频繁显示帖子及其作者的应用程序的效率。

使用扩展引用模式在这里在规范化和非规范化之间取得了平衡。虽然 _id 为更新或更深入的数据检索提供了指向原始 Author/User 文档的明确链接，但 username 和 avatar 等重复字段可确保帖子与基本作者信息一起快速呈现。此模式在社交媒体平台等读密集型应用程序中特别有用。

## 子集模式

子集模式用于在 MongoDB 中将频繁访问的数据与不频繁访问的数据分开存储，优化常见查询的读取，同时将更详细或不太需要的相关信息保留在相关文档中。

以下是一个媒体帖子的示例数据，该数据存储在两个单独的文档中：

// Document 1: Frequently accessed media post data
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d6f51"),
  postType: "video",
  title: "My Trip to Yosemite",
  authorId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  thumbnailUrl: "https://socialite.com/thumbnails/yosemite-trip.jpg",
  duration: "3:42",
  viewCount: 1287,
  likeCount: 328,
  commentCount: 57
}

// Document 2: Infrequently accessed media post data
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d6f61"),
  postId: ObjectId("5f4e5b6d3c1e2a1b3c4d6f51"),
  highResVideo: "https://socialite.com/videos/yosemite-trip-hd.mp4",
  rawMetadata: {
    codec: "H.264",
    resolution: "1920x1080",
    fps: 60,
    bitrate: "12Mbps"
  },
  processingHistory: [
    { timestamp: ISODate("2023-04-05T14:20:00Z"), action: "uploaded" },
    { timestamp: ISODate("2023-04-05T14:25:12Z"), action: "processed" },
    { timestamp: ISODate("2023-04-05T14:30:45Z"), action: "published" }
  ]
}

让我们回顾此示例以了解子集模式在此处的应用方式。子集模式旨在将数据集拆分为两个单独的文档（一个存储频繁访问的数据，另一个保存不频繁访问的数据），以优化常见查询的性能，同时在必要时仍可访问详细信息。

第一个文档（Document 1）包含有关媒体帖子的高度查询字段，包括以下内容：

- `postType`：媒体类型（例如 "video"）
- `title`：媒体帖子的名称
- 参与度指标，例如 `viewCount`、`likeCount` 和 `commentCount`
- `authorId`：对作者资料的引用
- 轻量级媒体信息，例如 `thumbnailUrl` 和 `duration`

此文档专为呈现帖子 feed、显示热门内容或加载媒体帖子摘要等用例而设计。通过保持此文档小巧并专注于频繁访问的字段，对其的查询速度快且轻量。

第二个文档（Document 2）存储日常查询中很少需要的详细数据，例如以下内容：

- 完整的视频 URL，存储在 `highResVideo` 中以用于流式传输或下载原始视频文件。
- `rawMetadata`，包含有关媒体的技术细节，例如其编解码器、分辨率、FPS 和比特率，这些对于后端处理相关但不适用于面向用户的查询。
- 一个 `processingHistory` 数组，记录媒体文件的处理工作流，包括上传、处理和发布等操作的 时间戳。
- 通过 `postId` 对第一个文档的引用，建立了两者之间的关系。这使得应用程序仅在必要时（例如准备视频进行流式传输或访问历史处理数据时）获取详细文档。

此文档更大且更详细。它单独存储，以确保常见查询（例如在 feed 中列出帖子）不会因加载不必要的技术或历史信息而负担过重。

使用子集模式允许您将数据拆分为两个单独的文档：一个包含频繁访问的字段，第二个文档单独存储大且不频繁访问的信息。这种方法在频繁查询的效率与在需要时检索详细数据的能力之间取得了平衡，使其成为数据集的不同部分具有显著不同访问模式的场景中的理想解决方案。

## 写入性能优化模式

在本节中，我们将介绍有助于优化写入性能的模式设计。这些包括文档版本模式，允许您通过在同一文档或单独的集合中存储以前的版本来有效地管理频繁更新文档的更改；分桶模式，将多个相关记录分组到单个文档中，以减少写入次数并提高写入密集型工作负载的效率；以及键前缀模式，通过设计 _id 字段的值来避免创建二级索引。

## 文档版本模式

而不是更新现有文档并存储更改的内容，而是创建新版本并维护指向当前版本的指针。当需要始终保留实体更改的完整历史记录时，这特别有用。

以下是在 Socialite 应用中功能发布配置的示例，显示了包含两个不同版本的两个文档：

// Current version (in main collection)
{
  _id: "feature_story_sharing",
  name: "Story Sharing Feature",
  isEnabled: true,
  userPercentage: 75,
  configuration: {
    maxDurationSeconds: 30,
    allowedFilters: ["sepia", "noir", "vintage"],
    maxFileSize: 15728640
  },
  version: 3,
  lastUpdated: ISODate("2023-04-12T16:30:00Z")
}

// History collection (previous versions)
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d7f20"),
  featureId: "feature_story_sharing",
  name: "Story Sharing Feature",
  isEnabled: true,
  userPercentage: 50,  // Was 50%, now 75%
  configuration: {
    maxDurationSeconds: 30,
    allowedFilters: ["sepia", "noir"],  // "vintage" filter added later
    maxFileSize: 15728640
  },
  version: 2,
  updatedAt: ISODate("2023-04-10T09:15:00Z")
}

让我们回顾此示例以了解文档版本模式在此处的应用方式。文档版本模式用于通过将当前版本存储在主集合中并将文档的以前版本存储在单独的历史集合中来跟踪文档随时间的变化：

- **当前版本文档（主集合）**：第一个文档表示功能的活动配置，存储以下内容：
  - 功能的当前状态（isEnabled: true，userPercentage: 75）
  - 最新配置设置（allowedFilters 现在包含 "vintage"）
  - 当前版本号（version: 3）和时间戳（lastUpdated），指示上次修改的时间

此文档存储在主集合中，主集合专注于当前版本，为需要访问或修改最新功能配置的应用程序提供服务。由于它代表最新数据，因此优化了快速读取和更新。

- **以前版本文档（历史集合）**：第二个文档提供了功能配置早期版本的快照，包括以下内容：
  - 历史设置，例如 userPercentage: 50 和 allowedFilters，在添加 "vintage" 过滤器之前



-   version: 2 字段清楚地表明这是该功能的早期版本
-   updatedAt 时间戳记录了该版本被新版本替换的时间

该文档存储在专门用于维护文档版本的独立历史集合中。将旧版本单独存储可确保主集合保持轻量，同时仍能访问历史数据以满足审计、调试或比较分析等用例的需求。

文档版本控制模式通过将功能配置的当前版本与其历史版本分离，避免了记录“发生了什么变化”所需的第二次写入。此模式最适合需要维护审计跟踪、随时间跟踪更改或支持回滚的应用程序，例如配置管理系统、内容发布平台，或任何需要在不影响活动工作流性能的情况下需要历史数据的系统。

## 分桶模式

对于时间序列或高容量数据，如果由于某种原因无法使用特殊的时间序列集合，可以使用分桶模式将相关数据点分组到“桶”中以减少文档数量。

以下是 Socialite 应用中跟踪用户每日活动的 timeline 缓存示例：

```
// With bucketing (optimized for fast timeline displays)
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5f2f"),
  userId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  date: ISODate("2023-04-10T00:00:00Z"),
  activities: [
    {
    type: "post",
    postId: ObjectId("5f4e5b6d3c1e2a1b3c4d6f4h"),
    timestamp: ISODate("2023-04-10T10:00:00Z"),
    content: "Just posted a new photo!",
    mediaUrl: "https://socialite.com/images/photo123.jpg",
    likes: 42
    },
    {
    type: "follow",
    timestamp: ISODate("2023-04-10T10:15:12Z"),
    targetUser: {
        id: ObjectId("5f4e5b6d3c1e2a1b3c4d6f7j"),
        username: "travel_adventures"
    }
},
{
    type: "like",
    timestamp: ISODate("2023-04-10T10:25:30Z"),
    postId: ObjectId("5f4e5b6d3c1e2a1b3c4d6f8k"),
    postAuthor: "mountain_explorer",
    postSnippet: "Summit day at Mt. Rainier..."
}

    // ... more activities within this day
],
    activityCount: 27
}
```

让我们回顾一下这个示例，以了解分桶模式在此处是如何应用的：

-   **分组活动：** 而不是将每个用户活动（如帖子、点赞和关注）作为单独的文档存储，特定日期（date: ISODate("2023-04-10T00:00:00Z")）用户的全部活动都被分组到 activities 数组下的单个文档中。数组中的每个条目代表一个活动，使得在该时间范围内跟踪相关操作变得容易。
-   **聚合元数据：** 文档包含 activityCount 字段，提供存储在桶内的活动数量的聚合摘要，便于快速参考。

在此示例中，分桶模式通过将用户的每日活动分组到单个文档中应用，而不是为每个活动创建单独的文档。此模式大幅减少了写入操作，并使显示用户的时间线极快，因为一次查询即可检索整天的活动。分桶模式非常适合涉及基于时间或逻辑分组的数据的用例，例如活动日志、事件跟踪、物联网传感器读数或时间序列数据，其中记录存在自然聚类且查询分组记录很常见。

## 键前缀模式

键前缀模式涉及向字段（通常是 _id 字段）添加有意义的前缀或标识符，以分组相关数据、实现高效查询并减少对二级索引的需求。键前缀模式最适合具有许多小型相关文档的工作负载，这些文档需要在不使用额外索引的情况下一起高效检索。

以下来自 Socialite 应用的示例包含有关用户通知的数据文档：

```json
{
  _id: "user_5f4e5b6d3c1e2a1b3c4d5e6f",  // Main user document
  username: "jane_smith",
  email: "jane.smith@example.com"
  // ... other user fields
}

{
  _id: "user_5f4e5b6d3c1e2a1b3c4d5e6f_notif_001",  // Notification with prefix
  type: "like",
  message: "Alex liked your post",
  timestamp: ISODate("2023-04-10T10:30:00Z"),
  read: false
}

{
  _id: "user_5f4e5b6d3c1e2a1b3c4d5e6f_notif_002",  // Another notification
  type: "comment",
  message: "Sarah commented on your post",
  timestamp: ISODate("2023-04-10T11:15:00Z"),
  read: false
}
```

让我们回顾一下这个示例，以了解键前缀模式是如何应用的。在这里，该模式用于通过使用用户标识符为 _id 加前缀来结构化 _id 字段，从而将通知与特定用户关联：

-   **主文档前缀：** 主用户文档具有 _id 值（user_5f4e5b6d3c1e2a1b3c4d5e6f），其中前缀 user_ 清楚地将其标识为用户记录。此结构化的 _id 值有助于逻辑地组织文档并简化针对特定类型记录的查询。
-   **相关通知：** 与用户相关的通知使用相同的前缀（user_5f4e5b6d3c1e2a1b3c4d5e6f），后跟额外的后缀（_notif_001、_notif_002）。这确保所有通知都直接与用户关联，并且可以使用 _id 高效查询。

使用 _id 字段的默认索引，您可以使用范围查询高效检索用户的全部通知。此外，如果 user_ 之后的前缀部分是对象 ID，通知也会自然按时间排序。

## 查询和分析优化模式

在本节中，我们将查看有利于查询和分析优化的模式设计。这些包括计算模式，该模式预先计算并存储派生或聚合数据以降低查询执行期间即时计算的成本；模式版本控制模式，该模式跟踪文档结构随时间的变化以确保跨版本的兼容性并促进对不断变化的数据集的分析；以及多态模式，该模式允许通过利用灵活的模式在统一集合中存储不同类型的相关数据，从而在无需多个集合的情况下实现高效查询和分析。

### 计算模式

使用计算模式，您可以预先计算并存储聚合结果，以避免在读取操作期间进行昂贵的计算。这包括在操作发生时递增计数器，以便总数始终反映当前指标。

以下是 Socialite 数据库中包含帖子分析的示例文档：

```
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d6f7k"),
  content: "Check out my new photography portfolio!",
  postDate: ISODate("2023-04-10T14:30:00Z"),
  // Pre-computed metrics
  engagementMetrics: {
  totalViews: 1432,
  uniqueViewers: 1053,
  likeCount: 287,
  commentCount: 43,
  shareCount: 18,
  avgTimeViewed: 12.5,  // in seconds
  conversionRate: 0.086  // % of viewers who took action
},

  // Demographics of engagers (pre-computed)
  audienceBreakdown: {
  genderDistribution: {
    male: 0.48,
    female: 0.51,
    other: 0.01
  },
  ageGroups: {
    "18-24": 0.32,
    "25-34": 0.45,
    "35-44": 0.15,
    "45+": 0.08
  }
}
}
```

让我们回顾一下这个示例，以了解计算模式是如何应用的。在这里，该模式用于将预先计算的参与度指标和受众人口统计细分与主要帖子内容一起存储：

-   **参与度指标**：文档包含预先计算的 engagementMetrics 字段，其中包含 totalViews、uniqueViewers、likeCount、commentCount、shareCount、avgTimeViewed 和 conversionRate 等聚合数据。不是为每个查询动态计算这些指标，而是将它们直接存储在文档中以便即时检索。
-   **受众人口统计**：audienceBreakdown 字段提供有关观众性别分布和年龄组百分比的预先计算数据。这消除了在查询时执行分析计算或运行昂贵的聚合管道的需求。



通过预先计算这些数据并将其嵌入到文档中，与参与度统计和受众人口统计相关查询可以快速执行，而无需访问原始事件日志或其他相关数据集。

该模式消除了在读取操作期间进行复杂聚合查询的需求。请注意，这通过增加一次额外的写入，提高了本应只是读取或写入的成本；为了避免此成本，您可以将额外的写入排队作为异步写入执行，而不是使其在原始操作期间成为必需。

## 模式版本控制模式

在应用模式版本控制模式时，您会包含一个版本字段来跟踪随时间的模式更改，从而在模式演进期间实现平滑过渡。

以下示例包含来自 Socialite 数据库的两个示例用户文档：

```
// New document with schema version 3
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  schemaVersion: 3,
  username: "jane_smith",
  name: { // In schema v3, we split name into first/last
    first: "Jane",
    last: "Smith"
  },
  email: "jane.smith@example.com",
  // ... other fields
}

// Older document with schema version 2
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  schemaVersion: 2,
  username: "john_doe",
  fullName: "John Doe", // In schema v2, we used a single name field
  email: "john.doe@example.com",
  // ... other fields
}
```

让我们回顾此示例以了解模式版本控制模式在此处是如何应用的：

- **版本标识：** `schemaVersion` 字段明确指示每个文档符合的版本，使应用程序级逻辑能够在查询、更新或读取期间处理模式的变化。
- **模式演进：** 在示例中，两个文档与不同的模式版本相关联。对于版本 3，名称存储在两个字段中（first 和 last），而在版本 2 中，名称存储为单个 fullName 字段。

该模式允许您的应用程序优雅地处理具有不同模式版本的文档。除非需要进行模式验证或应用程序的某些部分无法仅通过使用字段的缺失作为模式版本的指示来满足，否则可能不需要使用 schemaVersion 字段。

## 多态模式

多态模式将相似但不完全相同的实体存储在同一集合中以实现灵活查询。

以下示例包含来自 Socialite 数据库的文档，其中包含活动源数据，包括 post、following 和 group join 活动：

```
// Post activity
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  activityType: "post",
  userId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  timestamp: ISODate("2023-04-10T15:30:00Z"),
  content: "Just posted a new photo!",
  mediaUrl: "https://socialite.com/images/photo123.jpg"
}

// Follow activity
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7b"),
  activityType: "follow",
  userId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  timestamp: ISODate("2023-04-10T16:45:00Z"),
  targetUserId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e8c"),
  targetUsername: "alex_wong"
}
```

```
// Group join activity
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e8d"),
  activityType: "group_join",
  userId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  timestamp: ISODate("2023-04-10T17:15:00Z"),
  groupId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e9e"),
  groupName: "Photography Enthusiasts"
}
```

让我们回顾此示例以了解多态模式在此处是如何应用的：

- **共享集合：** 所有活动都存储在同一个集合中。每个文档代表一种不同的活动类型，但共享诸如 `_id`、`activityType`、`userId`、`timestamp` 等通用字段，以及特定于活动的附加字段。
- **类型特定字段：** `activityType` 字段指定活动的类型（`post`、`follow` 或 `group_join`）。根据此值，每个文档中包含类型特定的字段。例如，post 活动包含 `content` 和 `mediaUrl`。

现在，而不是为每种活动类型维护单独的集合，查询可以使用 `activityType` 作为过滤器来定位单个集合。

该模式允许在保持类型特定字段的同时高效地跨不同类型的活动进行查询。

## 用于存储优化的归档模式

在本节中，我们将介绍归档模式，该模式有利于存储优化。此模式通过将不常访问或历史数据移动到为长期存储优化的单独集合或数据库来帮助降低活动存储成本。

要应用归档模式，您可以将旧数据或不常访问的数据移动到单独的集合，以提高主集合的性能。

以下示例包含来自两个集合的消息文档：一个用于活跃且经常访问的最近消息，另一个用于旧消息文档的长期存储：

```
// Active messages collection
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d7f5f"),
  conversationId: ObjectId("5f4e5b6d3c1e2a1b3c4d7f6f"),
  sender: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  recipient: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  content: "Are we still meeting tomorrow?",
  timestamp: ISODate("2023-04-10T14:30:00Z"),
  read: true
}

// Archived messages collection (older than 90 days)
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d7f7f"),
  conversationId: ObjectId("5f4e5b6d3c1e2a1b3c4d7f6f"),
  sender: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  recipient: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  content: "Hey, how's it going?",
  timestamp: ISODate("2023-01-05T10:15:00Z"),
  read: true,
  archivedAt: ISODate("2023-04-05T00:00:00Z")
}
```

让我们回顾此示例以了解归档模式在此处是如何应用的：

- **活动消息集合：** 第一个文档是活动集合的一部分，该集合存储经常访问或更新的最近数据，例如用户之间交换的消息。此文档代表于 2023-04-10 发送的消息，并保留在活动集合中，因为它被视为正在进行交互的一部分。
- **归档消息集合：** 第二个文档是归档消息集合的一部分，该集合存储不再是活跃交互一部分的旧消息。此文档代表于 2023-01-05 发送的消息，在超过 90 天阈值后于 2023-04-05 被归档。archivedAt 字段捕获该消息被移动到归档时的时间戳。

通过将数据分离到活动和归档集合中，该模式在保留历史数据的同时保持活动集合精简。您可以将数据归档到同一集群中的另一个集合、另一个集群或 Atlas 在线归档中。

## 实际应用：Socialite 应用

在实际应用中，实现最佳性能通常需要不仅仅是采用单一的模式设计模式；它涉及组合多个模式。MongoDB 的灵活性使开发人员能够做到这一点！本节探讨了如何组合模式设计模式来增强性能，并通过 Socialite 应用场景的实际示例来说明。

### 场景 1：用户资料和活动源

在以下场景中，Socialite 应用需要高效地显示用户的资料以及其最近的活动源。传统方法是将用户数据、帖子、评论、点赞和关注存储在单独的集合中，然后每次查看个人资料页面时在这些集合之间执行复杂的连接。此方法可能很慢且资源密集，尤其是在数据量增长时。然而，通过 MongoDB，我们可以通过使用模式设计模式的组合来优化此过程：计算模式和子集模式。

以下示例演示了此方法。它显示了 users 集合中的单个文档：

```
// User document
{
  _id: ObjectId("5f3d7eb8c242d20df83d3a8c"),
  username: "jane_smith",
  name: "Jane Smith",
  bio: "Photographer and travel enthusiast",
  profileImage: "https://socialite.com/images/profiles/jane_smith.jpg",
  followerCount: 1245,  // Computed pattern
  followingCount: 362,  // Computed pattern

  // Subset pattern - most recent activities
  recentActivities: [
    {
      type: "post",
      timestamp: ISODate("2023-04-07T10:22:15Z"),
      content: "Just posted photos from my Japan trip!",
      imageUrl: "https://socialite.com/images/posts/japan123.jpg",
      likeCount: 48
    },


```
{
  type: "like",
  timestamp: ISODate("2023-04-07T09:15:30Z"),
  targetPost: {
    id: ObjectId("5f3d7eb8c242d20df83d3a94"),
    author: "travel_adventures",
    snippet: "My day at Mount Fuji..."
  }
}
```
// 3-5 条更近期的活动
]
}

让我们回顾这个示例，了解计算模式和子集模式是如何在此应用的：

- **Computed pattern:**
  - Fields such as followerCount and followingCount are calculated ahead of time and stored directly in the user document
  - It reduces the need for on-the-fly aggregation or queries to count followers/following during profile views
- **Subset pattern:**
  - It embeds an array of recentActivities directly in the user document
  - It includes only the most recent posts, likes, or actions, optimized for quick access during profile page loads
  - More comprehensive activity history is deferred to a separate collection for less frequent queries

这种设计通过将最近的活动直接包含在用户文档中来优化个人资料页面的加载。完整的活动历史存储在单独的集合中，仅在用户查看更多活动时才会被查询。

## 场景 2：聊天系统

在以下场景中，Socialite 应用需要实现一个消息系统，能够高效地处理实时聊天和用户消息历史的访问。我们可能会想把所有消息存储在单个集合中，并反复查询以获取实时更新和历史数据，但随着消息量的增长，这会导致性能瓶颈。然而，通过 MongoDB，我们可以通过结合子集、桶化和归档模式来优化这一点。

以下示例演示了这种方法，并包含两个文档，一个来自活动消息集合，另一个来自归档消息集合：

```
```
// Active messages collection - current conversation state (for fast access)
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d7f6r"),
  participants: [
    ObjectId("5f3d7eb8c242d20df83d3a8c"),  // jane_smith
    ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a")   // john_doe
  ],
  lastMessage: {
    sender: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
    content: "See you tomorrow!",
    timestamp: ISODate("2023-04-12T18:45:22Z")
  },
  unreadCount: {
    "5f3d7eb8c242d20df83d3a8c": 0,
    "5f4e5b6d3c1e2a1b3c4d5e7a": 1
  },

  // Recent messages (subset pattern)
  recentMessages: [
    {
      sender: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
      content: "See you tomorrow!",
      timestamp: ISODate("2023-04-12T18:45:22Z"),
      read: false
    },
    {
      sender: ObjectId("5f3d7eb8c242d20df83d3a8c"),
      content: "Yes, 3 PM works for me.",
      timestamp: ISODate("2023-04-12T18:44:15Z"),
      read: true
    }
  ]
}
```
// 10-15 条最近的消息
]
}

// Archived messages collection - for older messages, using bucketing pattern
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d8f1t"),
  conversationId: ObjectId("5f4e5b6d3c1e2a1b3c4d7f6r"),
  date: ISODate("2023-04-11T00:00:00Z"),  // One bucket per day
  messages: [
    {
      sender: ObjectId("5f3d7eb8c242d20df83d3a8c"),
      content: "How about meeting tomorrow?",
      timestamp: ISODate("2023-04-11T15:30:12Z"),
      read: true
    }
    // More messages from this day
  ]
}
```

让我们回顾这个示例，了解子集、归档和桶化模式是如何在此应用的：

- **Subset pattern:**
  - The recentMessages field in the first document from the active messages collection stores the most recent 10–15 messages directly within the document
  - It enables quick access to the latest messages without the need for a separate query to a larger data store
- **Archive pattern:**
  - The second document uses the archive pattern to store older messages. The conversationId field links the bucket to a specific conversation, ensuring logical grouping by conversationId. The date field represents the day covered by the bucket, allowing for chronological organization of messages.
- This pattern ensures efficient storage and retrieval of older, less frequently accessed messages and reduces overhead when querying or analyzing historical conversations, as queries can target specific buckets based on date and conversationId.
- **Bucketing pattern:**
  - Older messages are grouped into daily buckets in the messages array in a separate collection with one document per conversation per day
  - It organizes historical messages efficiently to reduce the overhead of querying massive datasets and improves scalability for long-running conversations.

这种方法允许快速访问最近的消息，同时通过组织和高效查询历史消息的归档来保持可扩展性。

## Summary

通过将您的模式与数据使用的实际模式对齐（如何读取、写入或更新文档），您可以最大限度地减少 I/O 和资源争用。MongoDB 的灵活结构可以帮助，但只有在深思熟虑地使用、结合正确的嵌入、引用和索引时才有效。

这里有很多重要的收获。在设计模式之前，至关重要的是了解您的工作负载并考虑读写模式。确保您以文档而非表格的方式思考。拥抱文档模型的灵活性，而不是强加关系模式。记住为常见操作设计并优化具有最严格性能要求的<|message|> 操作。使用真实的数据量进行测试：适用于 100 个文档的内容可能在 1000 万个文档时失败。最后，始终要有意为之：深思熟虑的模式设计始终优于任意的数据存储。

MongoDB 中的模式设计既是一门艺术，也是一门科学。通过了解应用程序的工作负载并平衡读写效率，您可以创建既高效又可维护的模式。

MongoDB 在快速、水平可扩展的键值存储和功能丰富的数据库之间架起了桥梁。为了充分发挥其威力，请设计您的模式以利用文档模型的优势：嵌入相关数据、避免不必要的查找，并针对特定的访问模式进行优化。在下一章中，我们将基于这些模式设计原则，探索进一步增强应用程序性能的索引策略。

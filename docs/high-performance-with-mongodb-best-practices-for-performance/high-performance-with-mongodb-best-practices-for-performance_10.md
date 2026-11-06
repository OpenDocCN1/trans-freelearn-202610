# 10 客户端库

MongoDB 的客户端库由官方语言驱动程序和 **对象文档映射器 (ODMs)** 组成，作为开发者与数据库之间的接口。这些库抽象了低级数据库交互，为每种编程语言提供符合惯用语、高性能的体验。通过封装网络通信、查询优化和连接管理的复杂性，客户端库让你能够专注于编写清晰、可维护的应用程序代码，而非与数据库实现细节搏斗。

本章重点介绍理解、配置和优化 MongoDB 客户端库以实现最佳性能和可靠性。我们将探讨驱动程序和 ODM 的作用，深入研究实用的配置最佳实践，并说明如何有效利用其功能构建健壮、可扩展的应用程序，充分利用 MongoDB 的能力。

本章涵盖的关键主题包括以下内容：

- 定义并解释 MongoDB 驱动程序在连接应用程序与数据库中的基本作用
- MongoDB 驱动程序的关键特性，包括通过共享规范实现一致性和符合惯用语的体验，以及连接管理和查询效率等性能优化技术
- 驱动程序的高级特性，例如错误处理、读写关注点和监控能力
- ODM 在简化应用程序开发中的优势
- ODM 的概述，包括模式强制、查询 API 和关系管理
- 高级主题，例如使用客户端库的异步模式和网络性能优化

### 什么是驱动程序？

MongoDB 驱动程序是软件库，促进应用程序代码与 MongoDB 集群之间的通信。不是直接使用低级网络协议、**二进制 JSON (BSON)** 序列化或线协议细节，你可以使用这些驱动程序通过直观的、特定于语言的 API 与 MongoDB 交互。驱动程序将高级操作转换为数据库命令，使你能够执行 **创建、读取、更新和删除 (CRUD)** 操作、管理索引、执行聚合管道，并在不编写原始数据库命令的情况下实现其他 MongoDB 功能。

MongoDB 为最流行的编程语言提供官方驱动程序，包括 C#、Go、Java、Node.js、PHP、Python、Ruby 和 Rust。每个驱动程序都遵循 MongoDB 规范存储库中维护的共享规范集 https://github.com/mongodb/specifications，确保一致性和功能对等，同时保留特定于语言的惯用语，例如方法名称大小写约定。这种结构化方法在多样化的技术栈中提供可靠性，同时对使用其首选语言的开发者来说仍然感觉自然。

MongoDB 客户端库的有效使用直接影响应用程序性能。配置良好的驱动程序可以最小化延迟、最大化吞吐量并帮助保持应用程序可扩展性。理解连接池、重试能力、读取偏好和读写关注点使你能够为特定工作负载和可靠性要求微调数据库操作。

随着我们深入本章，我们将探讨这些库的内部工作原理，检查配置最佳实践，并演示如何在实际场景中利用其功能。无论你是在构建简单的 Web 应用程序还是复杂的分布式系统，了解 MongoDB 的客户端库对于创建高效、健壮的应用程序以充分利用数据库功能至关重要。

### MongoDB 驱动程序的工作原理

当你使用 MongoDB 驱动程序时，你利用的是精心设计的接口来处理以下等复杂任务：

- **优化数据库访问**：驱动程序管理连接池以高效重用数据库连接。不是为每个操作创建新连接，驱动程序维护一个活动连接池，减少延迟并提高性能。这包括根据需要建立、维护和关闭连接，并且通常允许配置池大小和连接超时参数。
- **序列化与反序列化数据：** MongoDB 驱动程序在将数据发送到数据库之前处理将应用程序数据序列化为 BSON 格式，然后将 BSON 响应反序列化回原生数据结构。这种自动转换确保数据在应用程序和 MongoDB 服务器之间正确传输和解释。
- **自动重试逻辑：** 为了确保可靠性，驱动程序会自动处理网络故障的重试逻辑。如果数据库操作因临时网络问题而失败，驱动程序可以重试操作，通常基于可配置的重试策略，确保数据完整性和应用程序稳定性。
- **自动操作路由：** 在副本集中，驱动程序智能地将操作路由到适当的服务器。驱动程序了解集群拓扑，可以将读写操作定向到正确的节点，包括处理故障转移并根据读取偏好配置将读取路由到从节点。
- **监控服务器变化：** 驱动程序持续监控副本集中服务器拓扑变化。如果服务器宕机或添加新服务器，驱动程序会更新其对集群的视图并相应调整路由决策。这种动态适应确保应用程序连续性和在不断变化环境中的正常运行。
- **多文档 ACID 事务：** 它们提供 API 来启动、管理和提交或中止多文档 ACID 事务，确保多个操作之间的原子性和一致性（在 *第 9 章，事务* 中更详细地介绍）。
- **变更流用于实时数据变更通知：** 它们允许应用程序订阅变更流，实现数据修改、删除或插入的实时通知，这对于构建事件驱动架构至关重要（在 *第 8 章，变更流* 中更详细地介绍）。
- **聚合管道用于复杂数据处理：** 应用程序可以定义和执行复杂的聚合管道，在 MongoDB 中转换、过滤和分组数据，实现复杂分析（在 *第 4 章，聚合* 中更详细地介绍）。
- **GridFS 用于存储和检索大文件：** 驱动程序实现 GridFS 规范，允许应用程序通过将大文件拆分为块来存储和检索，克服文档大小限制。
- **客户端字段级加密以增强安全性：** 它们使应用程序能够在数据发送到数据库之前加密特定字段，保护传输中和静态的敏感数据。
- **Atlas Search 和 Vector Search：** 驱动程序公开便捷的 API，使应用程序能够利用 Atlas Search 和 Vector Search，提供强大的全文搜索和向量搜索功能，增强应用程序的搜索和分析功能。




In essence, MongoDB drivers are more than simple connectors; they are sophisticated tools designed to streamline database interactions and optimize performance. By handling tasks such as connection management, BSON serialization, and intelligent routing, drivers allow you to focus on application logic rather than low-level database mechanics. Having this understanding of how drivers function enables us to build robust, scalable MongoDB applications and lays the foundation for the effective use of higher-level abstractions such as ODMs.

## Key features of MongoDB drivers

MongoDB drivers offer a wide range of features that ensure both functionality and efficiency. Understanding these features is vital for building robust applications. In this section, we’ll delve into the key aspects that make these drivers powerful. We’ll explore how drivers ensure consistency and reliability through shared specifications, offer an idiomatic experience tailored to each language, optimize for performance, and provide support for advanced features such as transactions and change streams.

## Consistency and reliability through shared specifications

One of the defining characteristics of official MongoDB drivers is their adherence to common specifications. By following core specifications, all drivers support the same fundamental features, such as connection pooling, retryable reads or writes, and wire protocol interactions, regardless of the programming language. This consistency is particularly valuable for organizations working in multi-language environments, as developers can expect similar behavior and capabilities across different technology stacks.

When MongoDB introduces new features, these specifications are updated, ensuring that all drivers implement the functionality in a reliable and consistent manner. This specification-driven approach reduces bugs and unexpected behaviors across different language drivers.

## Community-maintained MongoDB drivers

Aside from MongoDB’s officially maintained drivers, there are community-maintained drivers developed using the shared specifications. These drivers may not implement all specifications or offer all features the official drivers do; however, this should not discourage you from trying them in environments where official drivers don’t exist.

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_232_0.png)

## Idiomatic experience

Although MongoDB drivers share common specifications, each driver is designed to feel natural within its programming language ecosystem. Take the following examples:

- The Node.js driver leverages JavaScript promises and async/await patterns
- The Python driver follows PEP guidelines and Pythonic principles
- The Java driver provides strong typing and integrates with Java’s concurrency tools
- The C# driver embraces .NET idioms and LINQ-style queries

This idiomatic design means that you can work with MongoDB using patterns and practices you’re already familiar with, reducing the learning curve and improving productivity. Each driver’s API is designed to align with the conventions of its host language, making database interactions feel like a natural extension of the language itself.

## Performance optimization

MongoDB drivers are meticulously engineered for performance. They minimize latency and maximize throughput by incorporating numerous optimizations, such as the following:

- **Connection pooling**: Reusing existing database connections instead of creating new ones for each operation, reducing connection overhead and lowering latency.
- **Intelligent operation routing**: Drivers make intelligent server selections by continuously monitoring network latency and server state, dynamically routing read and write operations to the most optimal nodes in a replica set or sharded cluster, ensuring high availability and minimal latency.
- **Network data compression**: Efficiently compress data before it is transmitted across the network, employing algorithms such as Snappy, zlib, or zstd, which reduces the amount of data transferred, thus lowering bandwidth usage and speeding up data transmission. This is particularly beneficial for applications dealing with large, compressible documents or high network loads.
- **Bulk database operations**: Allow multiple actions, such as inserts, updates, or deletes, to be grouped into a single request, which minimizes the number of network round-trips and dramatically reduces overall network overhead. This is especially beneficial for improving the performance of batch data processing tasks.

These optimizations are particularly important for high-throughput applications where database performance directly impacts user experience. The drivers handle these optimizations behind the scenes, allowing developers to focus on application logic rather than performance tuning.

## What are object-document mappers (ODMs)?

Beyond the official drivers, ODMs offer an additional abstraction layer that bridges MongoDB’s document-based data model with object-oriented programming paradigms. Similar to **object-relational mappers (ORMs)** in SQL databases, ODMs allow you to work with native language objects instead of raw BSON documents. Popular ODMs include EF Core for C#, Symfony for PHP, Django for Python, Mongoose for JavaScript, Mongoid for Ruby, and Prisma for TypeScript.

ODMs provide valuable features, including the following:

- Schema enforcement and validation
- Convenient query-building APIs
- Relationship management between documents
- Middleware and life cycle hooks
- Type safety and IDE autocompletion

Similar to the value ORMs bring to developers working with SQL databases, ODMs provide a meaningful abstraction layer between MongoDB’s document-based data model and an application’s object-oriented programming paradigm.

## Understanding ODMs

At their core, ODMs translate between two worlds: MongoDB’s flexible document model and the structured, class-based approach of object-oriented programming. This translation layer offers significant advantages for developers seeking to maintain a clean separation of concerns within their applications.

To illustrate the contrast between using MongoDB’s native driver directly and leveraging an ODM, the following examples compare retrieving a user document, the first using the PyMongo driver, while the second demonstrates the same operation using MongoEngine:

```python
from pymongo import MongoClient

client = MongoClient()
db = client.my_database
users = db.users
user = users.find_one({"email": "john@example.com"})
print(f"Name: {user['first_name']} {user['last_name']}")
```

While PyMongo gives you full control over queries and data structures, it leaves it to the developer to enforce schema constraints, handle serialization logic, and manually parse or transform results.

```python
### With MongoEngine ODM
from mongoengine import Document, StringField, connect

connect('my_database')

class User(Document):
    first_name = StringField(required=True, max_length=50)
    last_name = StringField(required=True, max_length=50)
    email = StringField(required=True, unique=True)

user = User.objects.get(email="john@example.com")
print(f"Name: {user.first_name} {user.last_name}")
```

With MongoEngine, the schema is declared once using Python classes, making your domain model explicit and easier to reason about. Queries feel more natural in an object-oriented context, and developers benefit from built-in validation, better code completion in IDEs, and reduced risk of runtime key errors. The result is code that can be easier to read, test, and evolve over time.

The key differentiator between a driver and an ODM is that the driver is operation-based, whereas the ODM is state-based. Drivers primarily focus on CRUD operations, or other database operations that you need to perform directly.

ODMs, on the other hand, rely heavily on the state. You load an object into a context, manipulate that object using language primitives, then persist those changes. It is the job of the ODM to translate the current state of the object into the necessary database commands to persist the object’s state.

While MongoDB’s native drivers provide the foundation for database communication and interaction, ODMs build upon them to offer higher-level abstractions that align more naturally with application code patterns. This approach reduces boilerplate, enhances code organization, and improves maintainability by representing database entities as classes with well-defined behaviors.

## Key features of ODMs

ODMs offer numerous benefits that simplify database interactions and enhance application development. In this section, we’ll explore the key advantages of ODMs, specifically focusing on schema enforcement and data validation for structured data, the use of intuitive query

用于可读数据库交互的 API、用于定义和处理数据关联的关系管理、用于在各个阶段执行自定义逻辑的中间件和生命周期钩子，最后是类型安全和 IDE 集成，这促进了更健壮的开发工作流程并实现错误检测。

## 模式强制执行和数据验证

尽管 MongoDB 在设计上具有灵活的模式，但许多应用程序受益于结构一致性。正如我们在 *第 2 章，面向性能的模式设计* 中所看到的，MongoDB 提供了可选的模式验证功能，可在服务器端强制执行。然而，对于客户端验证，ODM 允许开发人员定义明确的模式，以确保文档符合预期模式。

MongoDB 的固有灵活性允许数据以类似 JSON 的文档形式存储，而无需严格的模式。虽然这种方法提供了很强的适应性，但如果数据结构未得到管理，也可能导致不一致。诸如 Mongoose 之类的 ODM 通过使开发人员能够定义模式来提供解决方案，这些模式充当蓝图，以确保每个文档都符合特定的结构和规则，具体方法如下：

- 定义必填字段和默认值
- 指定数据类型和约束
- 实现自定义验证逻辑
- 在保存或检索数据时应用转换

这种程度的控制在保留 MongoDB 所知灵活性的同时提供了安全性和可预测性，例如以下示例展示了 Mongoose 如何帮助强制执行数据类型和验证规则，确保 MongoDB 集合中的数据完整性：

```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  firstName: { type: String, required: true, trim: true },
  lastName: { type: String, required: true, trim: true },
  email: {
    type: String,
    required: true,
    unique: true,
    validate: {
      validator: function(v) {
return /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v);
},
    message: 'Please enter a valid email'
  }
},
});
const user = mongoose.model('User', userSchema);
```

在前面的示例中，定义了一个 Mongoose 模式，列出了用户记录字段处理的几条规则。用户的名字、姓氏和电子邮件都是必填项。对于前两个字段，多余的空白将被自动修剪，并且电子邮件地址的唯一性和构成将被强制执行。电子邮件字段上的自定义验证器在验证失败时会显示自定义消息，这比简单地无法修改文档提供了更好的开发人员体验。

## 直观的查询 API

ODM 提供语言惯用的方式来构建数据库查询，使其更具可读性和可维护性。示例包括在 C# 中使用 LINQ，或在 C#、Java 和 PHP 中使用 Builders API。

为了说明 ODM 中查询构建的直观性，此使用 Mongoid 的 Ruby 代码示例演示了如何使用方法链和特定于语言的语法构建复杂的数据库查询，以查找特定类别和价格范围内有库存的产品，然后对它们进行排序：

```
class Product
  include Mongoid::Document
  field :name, type: String
  field :price, type: Float
  field :category, type: String
  field :in_stock, type: Boolean
end

### Find all in-stock products in the "electronics" category
### priced between $100 and $500, sorted by price
results = Product.where(:category => "electronics")
  .and(:in_stock => true)
  .and(:price.gte => 100, :price.lte => 500)
  .order_by(:price => :asc)
```

大多数 ODM 提供一系列功能，使使用 MongoDB 更直观高效，包括以下内容：

- 用于复杂查询构建的流畅 API
- 与 MongoDB 查询功能匹配的过滤运算符
- 投影和排序机制
- 分页助手
- 聚合框架抽象

因此，通过提供语言原生语法和语言惯用功能（如方法链），ODM 大大简化并增强了查询构建过程，使其对开发人员来说更易于接近且更高效。

## 关系管理

MongoDB 的文档模型固有地支持嵌入文档和引用。ODM 通过显式关系定义扩展了这一点。

为了说明 ODM 如何促进关系定义，以下使用 TypeScript 的 Prisma 模式示例演示了如何在不同实体之间指定一对一、一对多和多对多关系，例如作者、书籍和流派，以结构化的方式有效建模复杂的数据关联：

```
model Author {
  id    String  @id @default(auto()) @map("_id") @db.ObjectId
  name  String
  email String  @unique
  books Book[]  // One-to-many relationship
  bio   Bio?    // One-to-one relationship
}

model Book {
  id       String  @id @default(auto()) @map("_id") @db.ObjectId
  title    String
  isbn     String  @unique
  authorId String  @db.ObjectId
  author   Author  @relation(fields: [authorId], references: [id])
  genres   Genre[] // Many-to-many relationship
}

model Genre {
  id     String   @id @default(auto()) @map("_id") @db.ObjectId
  name   String   @unique
  books  Book[]
}

model Bio {
  id       String   @id @default(auto()) @map("_id") @db.ObjectId
  text     String
  authorId String   @unique @db.ObjectId
  author   Author   @relation(fields: [authorId], references: [id])
}
```

为了确保数据符合特定模式，ODM 提供了以下功能，使开发人员能够为文档结构和验证定义和强制执行规则：

- 一对一、一对多和多对多关系
- 嵌入文档模式
- 具有自动填充的引用模式
- 级联操作（保存、更新和删除）

通过为各种关系配置提供显式定义，ODM 使开发人员能够在 MongoDB 的文档结构内有效建模复杂的数据关联，弥合灵活数据库模型与结构化应用程序设计之间的差距。

## 中间件和生命周期钩子

大多数 ODM 提供钩子，在文档的访问或修改生命周期的特定点执行代码。以下示例演示了在 'save' 操作之前运行的 Mongoose 中间件，在将用户密码存储到数据库之前自动对其进行哈希处理：

```
userSchema.pre('save', async function(next) {
  // Only hash the password if it's modified or new
  if (!this.isModified('password')) return next();

  try {
    this.salt = await bcrypt.genSalt(10);
    this.password = await bcrypt.hash(this.password, this.salt);
next();
} catch (error) {
  next(error);
}
});

userSchema.post('save', function(doc) {
  console.log(`User ${doc.email} has been saved`);
});
```

ODM 通常在文档生命周期内提供各种钩子，允许开发人员在战略点执行自定义逻辑，例如以下常见示例：

- 验证前/后
- 保存前/后
- 更新前/后
- 删除前/后

这些钩子支持复杂行为，例如自动时间戳、日志记录、加密和更改跟踪。

## 类型安全和 IDE 集成

现代 ODM 利用语言特性提供编译时安全性和改进的开发人员体验。以下使用 Typegoose（Mongoose 的库）的 TypeScript 示例演示了类装饰器如何定义模式及其属性，从而在开发期间启用自动完成和类型检查，减少错误并提高开发人员生产力：

```ts
class User {
  @prop({ required: true })
  public firstName!: string;

  @prop({ required: true })
  public lastName!: string;

  @prop({ required: true, unique: true })
  public email!: string;

@prop({ default: Date.now })
public registeredAt?: Date;

public get fullName(): string {
  return `${this.firstName} ${this.lastName}`;
}
}

const UserModel = getModelForClass(User);

// Autocomplete and type checking works:
const newUser = new UserModel({
  firstName: "Jane",
  lastName: "Doe",
  email: "jane@example.com"
});

// TypeScript would catch this error:
// newUser.nonExistentField = "value"; // Error!
```

与语言工具和类型系统的无缝集成通过利用代码完成和类型检查等功能为开发人员提供许多优势，从而提高生产力并减少错误：

- 文档属性的自动完成
- 查询操作的<|message|>类型检查
- 重构支持
- 通过类型进行文档

最终，先进 ODM 提供的类型安全与紧密 IDE 集成的结合带来更高效、无错误的开发流程，使开发人员能够专注于构建功能而不是调试基本的数据处理问题。

## 对开发人员生产力的影响

ODM 在简化开发流程和提高开发人员生产力方面发挥着关键作用。通过提供与熟悉编码实践一致的抽象和工具，ODM 减少了管理低级数据库细节所花费的时间，使开发人员能够专注于构建应用程序功能。以下概述了 ODM 如何实现显著的生产力提升：

-   **减少样板代码：** 诸如数据验证、类型转换和关系管理等常见数据库操作可以被自动化，从而消除了开发人员编写重复冗长的代码的需求，节省了时间并降低了在这些重复任务中出错的可能性。
-   **改进代码组织：** ODM 通过将数据库实体表示为类或对象来促进更结构化的方法，按业务概念对代码进行逻辑组织，从而简化开发和维护，并使数据库交互与应用程序架构保持一致。
-   **增强可读性：** ODM 使用语言惯用的语法和直观的查询 API，使数据库操作更易读易懂，因为它们遵循熟悉的编程模式，而不需要了解特定的 MongoDB 命令，从而提高代码清晰度和团队协作。
-   **更早捕获错误：** 通过使用模式强制和类型检查在开发时而非运行时捕获与数据相关的错误，ODM 可以防止无效数据进入数据库。这减少了后期调试的需求，节省了时间并提高了整体代码质量。
-   **简化复杂操作：** ODM 通过提供高级抽象和方法，使诸如关系管理、嵌套文档处理和条件更新等复杂数据库操作变得更简单，使开发人员能够用更少的代码和更低的认知负担完成复杂任务。

通过自动化例行任务、简化复杂操作并提供更清晰的代码结构，ODM 可显著提高开发人员生产力，使他们能够将更多精力集中在功能开发上，而减少对数据库底层实现的关注。

## When to use ODMs

ODM 在许多开发场景中提供显著优势，但并非对每个项目都是必需的。是否采用 ODM 取决于多个因素，包括应用程序的复杂性、开发团队的经验以及对数据完整性和一致性的具体要求。以下情况是 ODM 特别有价值且对开发项目有益的：

-   从关系数据库迁移到 MongoDB 的团队会发现 ODM 有助于适应非关系数据模型，这得益于它们提供的熟悉面向对象模式。
-   当一致的数据结构是优先事项时，ODM 使定义和强制执行模式变得更容易，有助于防止数据不一致。
-   具有复杂领域模型的应用程序，在其中管理实体之间的关系至关重要，可以从 ODM 提供的结构中获得很大益处。
-   对于受益于清晰关注点分离的代码库，ODM 通过将数据库交互抽象为不同的层来提供帮助。
-   寻求在开发早期减少与数据库相关错误的开发团队会欣赏 ODM 提供的内置类型检查和验证功能。

然而，ODM 确实引入了一个抽象层，可能影响性能或限制对某些 MongoDB 特定功能的访问。对于具有极端性能要求或使用高级 MongoDB 功能的应用程序，在性能关键部分采用混合方法或直接驱动程序使用可能是合适的。

最终，使用 ODM 的决定应基于对项目复杂性、团队对 MongoDB 的熟悉程度以及对结构化数据处理和生产力提升需求的平衡评估。

## What are application frameworks?

应用程序框架为构建软件应用程序提供结构化基础，提供一组预构建的组件和库，用于标准化开发模式和实践。通过抽象常见任务，例如处理 HTTP 请求、管理数据库操作和渲染视图，这些框架使开发人员能够专注于实现业务逻辑，而不必为每个项目重新发明低级功能。

## The value of application frameworks

诸如 Ruby on Rails、Django、Spring、ASP.NET MVC 和 Laravel 等应用程序框架通过提供结构化的软件构建方法显著提升开发工作。它们提供众多优势，可增强开发过程和最终产品。以下是应用程序框架为开发团队及其解决方案带来的关键优势的分解：

-   1. 标准化：框架在项目中强制执行一致的结构和编码风格，使团队更易于协作和维护代码库。这种标准化降低了新团队成员的学习曲线，并确保在整个开发生命周期中遵循最佳实践。
-   2.  **效率：** 通过提供可重用组件和库，框架加速开发时间。开发人员可以快速集成身份验证、路由和数据验证等功能，而无需编写大量样板代码，从而使团队更快地交付功能。
-   3.  **可扩展性：** 设计良好的框架可处理扩展应用程序的复杂性。它们提供内置的负载均衡、缓存和数据库连接管理支持，使应用程序能够随着用户需求的增加无缝增长。
-   4.  **社区支持：** 流行的框架拥有庞大且活跃的社区，贡献插件、扩展和全面的文档。这些丰富的资源帮助开发人员快速解决问题并了解技术的最新进展。

本质上，应用程序框架为开发人员提供结构化、高效且可扩展的基础，促进标准化、生产力，并在大多数情况下提供强大的社区支持。

## Leveraging ODMs and ORMs in frameworks

大多数应用程序框架与 ODM 或 ORM 集成以简化数据建模和模式管理。这些集成提供一致的数据库操作方法，使开发人员能够使用其原生编程语言模式处理数据。

以下是一个代码示例，说明如何将 ODM，特别是 Mongoose，集成到流行的 Web 应用程序框架 Express.js 中。这演示了应用程序路由层与 ODM 提供的数据建模之间的无缝连接和交互，简化了在 Web 应用程序上下文中的数据库操作：

```
const express = require('express');
const mongoose = require('mongoose');
const app = express();

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/myapp');

// Define a schema using Mongoose ODM
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  created: { type: Date, default: Date.now }
});
```

```
// Create a model from the schema
const User = mongoose.model('User', userSchema);

// Use the model in an Express route
app.post('/users', async (req, res) => {
  try {
    const user = new User(req.body);
    await user.save();
    res.status(201).send(user);
  } catch (error) {
    res.status(400).send(error);
  }
});
```

前面的示例演示了使用 Mongoose 将 MongoDB 与基本 Express 应用程序集成的过程。它连接到本地 MongoDB 实例，定义了用户模式，其中包含必需的 name 和 email 字段（后者还标记为唯一），并提供带有默认值 created 时间戳。Mongoose 模型从模式创建以与数据库交互，并通过自定义路由公开，允许创建新用户的基本 REST API。

通过在应用程序内统一模式和标准，框架增强了开发人员生产力、代码可维护性和项目可扩展性。它们还促进协作工作流程，使团队能够无缝集成新功能，同时受益于既定的最佳实践。

## Popular MongoDB-compatible frameworks

多个应用程序框架提供与 MongoDB 的出色集成，为构建现代应用程序提供稳健高效的环境。这些框架各自带来针对特定编程语言和开发范式量身定制的功能和优势。以下是一些与 MongoDB 无缝协作的流行框架：

-   **Spring Data MongoDB (Java)**：提供一种便捷的、基于注解的方法来在 Spring 应用程序中管理 MongoDB 文档。它提供存储库抽象、对象映射和事务支持，使其成为企业 Java 应用程序的稳健选择。



- **Entity Framework (C#)**: 为 .NET 开发者提供熟悉的数据访问范式，利用存储库模式和代码优先建模。MongoDB 提供程序允许 .NET 开发者在使用文档数据库时继续使用他们熟悉的相同的 Entity Framework 抽象。
- **Ruby on Rails (Ruby)**: 以 Mongoid 作为其 ODM，Rails 遵循约定优于配置的原则，简化设置并自动化许多常规数据操作。其资源管道和脚手架工具使其在快速开发基于 MongoDB 的应用程序时特别高效。
- **Quarkus (Java)**: 专为云原生和容器化环境设计，Quarkus 通过响应式编程模型优化性能，该模型与 MongoDB 的异步驱动程序集成良好。其较低的内存占用使其非常适合微服务架构。
- **Django (Python)**: Django 提供了一个全面的框架，可以与 MongoEngine 集成，使开发者能够在熟悉且结构化的 Python 生态系统中利用 MongoDB 的灵活性。
- **Flask (Python)**: Flask 是一个轻量级替代方案，可很好地与 PyMongo 或 ODM 扩展配合使用，为开发者在使用 MongoDB 时提供更多灵活性和控制权。
- **FastAPI (Python)**: 一个用于使用 Python 构建 API 的现代高性能框架，FastAPI 非常适合创建由 MongoDB 支持的 RESTful 服务。它通过 Motor 或 Beanie 支持异步数据库操作，具有出色的性能特性。
- **Laravel (PHP)**: 一个流行的 PHP 框架，包含 artisan 命令和脚手架工具以简化数据库管理。其 Eloquent ORM 可通过诸如 jenssegers/mongodb 的包扩展以实现 MongoDB 集成。
- **Symfony (PHP)**: 另一个广泛使用的 PHP 框架，提供强大的数据库管理工具。Symfony 与 Doctrine MongoDB ODM 集成以操作 MongoDB。

总体而言，这些框架为开发者提供了在首选编程语言和架构模式中使用 MongoDB 的多种选择，促进高效且可扩展的应用程序开发。

## 使用框架与 MongoDB 时的最佳实践

在利用应用程序框架与 MongoDB 时，请考虑以下重要实践：

- **MongoClient 重用：** MongoClient 实例维护到所有集群成员的连接池以及监控连接。重用此实例很重要，以防止不必要的连接重复，这通常通过单例模式或框架的 IoC 容器的正确配置来实现。
- **高效的模式设计：** 设计模式以在优化查询性能的同时最小化冗余。根据应用程序访问模式，适当使用嵌入式文档和引用来平衡数据规范化和非规范化，并避免潜在的 N+1 问题。
- **避免 N+1 问题：** 注意 N+1 查询问题，其中执行多个单独查询来获取相关数据。使用诸如预加载、MongoDB 聚合管道或批量操作等技术来减少数据库往返次数。
- **缓存策略：** 实现适当的缓存层以减少数据库负载并提高应用程序性能。大多数框架提供内置缓存解决方案，可配置为与 MongoDB 查询结果配合使用。
- **延迟加载与预加载：** 根据应用程序的具体需求在延迟加载和预加载之间进行选择。延迟加载将相关数据的加载推迟到访问时，而预加载则提前获取所有相关数据。每种方法在性能和内存使用方面都有权衡。
- **生命周期回调：** 利用框架的 ODM 提供的生命周期钩子在文档生命周期的各个点执行自定义逻辑。这些回调允许您实现一致的业务规则，例如数据验证、转换或触发事件。
- **性能监控：** 对框架代码进行监控以监控 MongoDB 操作性能。了解查询模式和执行时间有助于识别瓶颈和优化机会。

应用程序框架通过提供结构化、可重用的组件来显著增强 MongoDB 开发，从而简化常见任务。它们与 ODM 的集成提供了一种自然的开发体验，其中数据库操作与惯用的编程模式保持一致。通过选择与语言专业知识和项目要求相符的框架，开发团队可以更高效地构建可扩展、可维护的 MongoDB 应用程序。

正如我们将在下一节中探讨的，了解流行 ODM 和框架的具体模式和陷阱将帮助您利用 MongoDB 的功能，同时保持干净、高性能的代码。

## 超出基础

在本节中，我们将探讨高级技术和策略，这些技术和策略可以在使用客户端库时显著提高 MongoDB 应用程序的性能、弹性和可扩展性。这些实践代表了基本功能与能够处理现实世界挑战的生产就绪应用程序之间的区别。

### 异步和非阻塞模式

现代应用程序通常需要在保持响应性的同时处理多个并发操作。异步编程模式对于实现这一点至关重要，尤其是在处理可能 otherwise 阻塞应用程序执行的数据库操作时。

MongoDB 客户端库在各种编程语言中为异步操作提供强大的支持：

- **Promises 和 async/await：** JavaScript 和 Node.js 驱动程序利用 promises 和 async/await 语法实现简洁、可读的异步代码。这消除了复杂的回调链，同时保持非阻塞执行。
- **事件驱动框架：** 许多 MongoDB 驱动程序可无缝集成到事件驱动架构中，允许应用程序将业务逻辑与 I/O 操作解耦。这种方法使应用程序在等待数据库响应时仍能继续处理。
- **语言特定实现：** 不同语言的驱动程序根据语言惯例实现异步模式，例如 Java 的 CompletableFuture、Python 的 asyncio，或 C# 和 Node.js 的 async/await。

通过有效实现异步模式，您的应用程序即使在高负载下也能保持响应性，如以下 Python 示例所示：

```python
import asyncio
from pymongo import AsyncMongoClient
async def main():
    uri = ""
    client = AsyncMongoClient(uri)
    try:
        database = client.get_database("sample_mflix")
        movies = database.get_collection("movies")

        # Query for a movie that has the title 'Back to the Future'
        query = { "title": "Back to the Future" }
        movie = await movies.find_one(query)
        print(movie)

        await client.close()
    except Exception as e:
        raise Exception("Unable to find the document due to the following error: ", e)

### Run the async function
asyncio.run(main())
```

在 MongoDB 客户端库中实现异步模式使您能够构建高度响应且性能良好的应用程序。通过利用诸如 Promises、async/await 或如前述示例中所示的语言特定并发功能等构造，开发人员可以确保数据库操作不会阻塞应用程序的主线程。

并发执行结合非阻塞 I/O 可防止应用程序冻结或变得无响应，最终在高负载下带来更顺畅、更高效的用户体验。

### 暴露和处理故障条件

为了管理瞬态故障和网络问题，您应该实现强大的错误处理。MongoDB 驱动程序提供可重试读取和写入的机制，可在临时故障情况下自动重试操作：

- **可重试写入：** MongoDB 驱动程序支持对因瞬态网络问题或副本集选举而失败的写入操作的自动重试。此功能默认启用，但可使用 retryWrites=false 连接字符串选项禁用。
- **可重试读取：** 同样，读取操作默认配置为在某些故障时自动重试。您可以使用 retryReads=false 选项禁用此行为。

尽管这些自动重试非常强大，但它们应与应用程序代码中的适当错误处理模式相结合，以优雅地处理数据库异常，例如以下 Node.js 代码片段所示，该片段说明了在执行插入操作时如何捕获和记录特定错误条件：

```
try {
    const result = await collection.insertOne(document);
    console.log(`Document inserted with _id: ${result.insertedId}`);
} catch (err) {
```



```
if (err.code === 11000) {
    console.error('Duplicate key error');
} else if (err instanceof MongoNetworkError) {
    console.error('Network error occurred');
} else {
    console.error('An error occurred:', err);
}
}
```

如果检测到重复键错误，则满足一个失败条件，而网络问题会触发另一个失败条件。try-catch 块使应用程序能够对这些条件保持弹性，这些条件应通过适当的业务逻辑来处理。

一些 MongoDB 驱动程序，例如 Node.js 驱动程序，现在已实现 **客户端操作超时 (CSOT)**。该功能通过配置 timeoutMS 值引入了更可预测的操作生命周期控制机制：

```
const docs = await collection.find({}, {timeoutMS: 1000}).toArray();
```

此示例确保游标的初始化和所有文档的检索将在 1 秒（或 1,000 毫秒）内完成，如果超出此时间限制则会抛出错误。此错误需要在应用程序中处理；然而，通过确保控制在 1 秒内返回给应用程序，可以实现对操作生命周期的更严格控制。

除了确保操作不会超过 timeoutMS 阈值之外，当配置了 CSOT 且启用了可重试读取时，操作将继续重试直到 timeoutMS 耗尽。这为可能因持续时间较长的瞬态网络错误而失败并需要额外重试才能成功的操作提供了额外的弹性层。

通过实现对失败和重试的稳健处理，开发人员可以确保其 MongoDB 应用程序即使在面临瞬态问题或网络中断时也能保持稳定和可靠。

## 连接管理

为了高效管理数据库连接，请使用驱动程序的连接池功能。与为每次数据库操作创建新连接不同，连接池有助于维护一组可重用的连接，从而显著提高性能。重要的是根据应用程序的并发需求和 MongoDB 服务器上可用的资源来配置连接池大小。

以下示例演示了如何使用 PyMongo 驱动程序在 Python 中初始化 MongoDB 客户端，设置连接池的特定参数，例如最大和最小池大小，以及连接的最大空闲时间：

```python
client = MongoClient('mongodb://localhost:27017/',
                    maxPoolSize=50,
                    minPoolSize=10,
                    maxIdleTimeMS=30000)
```

连接管理将在第 11 章，管理连接和网络性能中更详细地讨论；然而，前面的示例展示了如何在 MongoClient 实例旁边创建连接池。在这种情况下，池将始终有 10 个连接处于打开和可用状态，并在需要时可扩展到 50 个连接。每个连接在未使用时将等待 30 秒（maxIdleTimeMS 的值）后自动关闭。

有效的连接管理是应用程序性能和资源利用的基础。以下列表涵盖了可通过 MongoClient 选项或连接字符串直接设置的一些概念和可调参数的介绍：

- **连接池：** 连接池维护一组可重用的数据库连接，消除了反复建立新连接的开销。根据工作负载特征配置连接池大小。太小会导致连接等待时间，而太大则会浪费资源。
- **监控连接健康：** 实现健康检查以识别并替换失败的连接。大多数 MongoDB 驱动程序会自动处理此问题，但可能受益于调整超时和重试逻辑。
- **连接选项：** MongoDB 驱动程序提供众多可为特定用例调整的连接字符串选项：
  - `maxPoolSize`: 池中维护的最大连接数
  - `minPoolSize`: 需维护的最小连接数
  - `maxIdleTimeMS`: 连接在关闭前保持空闲的时间
  - `connectTimeoutMS`: 建立连接时等待的最大时间

有效配置和管理 MongoDB 驱动程序的连接池及其选项对于保持应用程序性能、稳定性和高效的资源利用至关重要。优化池大小、连接超时和空闲时间等设置可确保您的应用程序优雅地扩展并快速响应用户请求，而强大的监控和生命周期管理可防止连接问题影响可用性。

为了说明如何微调连接池，请考虑以下场景及其相应的建议：

| 场景 | 建议 |
|---|---|
| 应用程序端操作时间缓慢，但未在数据库服务器日志或实时面板中反映。 | 使用 connectTimeoutMS 确保驱动程序在连接阶段不会无限期等待。将 connectTimeoutMS 设置为大于您到集合成员的最长网络延迟的值。

例如，如果某个成员的延迟为 10,000 毫秒，将 connectTimeoutMS 设置为 5000（毫秒）会阻止驱动程序连接到该成员。 |
| 配置错误的防火墙错误地关闭了套接字连接，且驱动程序无法检测到连接未正确关闭。 | 使用 socketTimeoutMS 确保套接字始终关闭。将 socketTimeoutMS 设置为驱动程序运行的最慢操作长度的两到三倍。 |
| 服务器日志或实时面板显示应用程序花费太多时间创建新连接。 | 启动时可用连接不足。通过设置 minPoolSize 在池中分配连接。将 minPoolSize 设置为您希望在启动时可用的连接数。MongoClient 实例确保连接数始终存在。 |
| 数据库负载较低，且任何时候活跃连接数都很少。应用程序同时执行的 fewer operations 比预期少。 | 增加 maxPoolSize，或增加应用程序或您使用的框架中的活动线程数。 |
| 数据库 CPU 使用率高于预期。服务器日志或实时面板显示连接尝试次数比预期多。 | 减小 maxPoolSize 值或减少应用程序中的线程数。这可以降低负载和响应时间。 |

*表 10.1：与连接相关的性能问题及推荐的 MongoDB 驱动程序设置*

通过微调这些参数，您可以显著提高应用程序与 MongoDB 交互时的效率和可靠性。

## 读写关注点和读取偏好

如 *第 5 章，复制* 中所述，MongoDB 提供了可配置的读取偏好、写入关注点和读取关注点，可让您精细控制操作如何与副本集交互。这些设置可直接通过客户端库进行配置，在平衡一致性、可用性和性能方面发挥着至关重要的作用：

- **读取偏好：** 这些决定哪些副本集成员处理读取操作：
  - `primary`: 这是默认设置。所有读取都转到主节点。
  - `primaryPreferred`: 首先尝试主节点，如果不可用则使用从节点。
  - `secondary`: 仅从从节点读取。
  - `secondaryPreferred`: 首先尝试从节点，如果不可用则使用主节点。
  - `nearest`: 从网络延迟最低的节点读取。
- **写入关注点：** 这些指定写入操作所需的确认级别：
  - `w:1`: 主节点确认写入。
  - `w: "majority"`: 默认设置。写入必须在副本集的大多数成员上持久化。
  - `w:0`: 无需确认（即发即弃 – 不推荐）。
  - `wtimeout`: 选项用于指定时间限制，以防止写入操作无限期等待。
- **读取关注点：** 这些控制从副本集和分片集群读取的数据的一致性和隔离属性：
  - `local`: 默认设置。返回实例中的最新数据。
  - `available`: 与 local 保证类似。它在分片集群中使用以实现最低延迟读取，但以一致性为代价，因为可用的读取关注点在从分片集合读取时可能会返回孤立文档。
  - `majority`: 返回已被副本集大多数成员确认的数据。读取操作返回的文档即使在发生故障时也是持久的。
  - `linearizable`: 返回反映在读取操作开始之前已完成的所有成功的大多数确认写入的数据。查询可能等待并发执行的写入传播到副本集的大多数成员后才返回结果。




-   **快照：** 使用快照读关注的查询会返回在最近过去的某个特定时间点上跨分片显示的大多数已提交数据。快照读关注仅在事务以大多数写关注提交时才提供其保证。

通过有效使用写关注和读关注，您可以根据需要调整一致性和可用性保证的级别，例如等待更强的一致性保证或放宽一致性要求以提供更高的可用性。

## 压缩和网络性能

优化应用程序与 MongoDB 之间的数据传输可以显著影响性能，尤其是在处理大型数据集或高吞吐量需求时：

-   **网络压缩：** MongoDB 驱动程序支持多种压缩算法：
    -   **Snappy：** 在压缩比和速度之间提供了良好的平衡
    -   **zlib：** 压缩比更高但更耗 CPU
    -   **zstd：** 新型算法，压缩效果出色且速度良好

-   **最小化网络延迟：** 在可能的情况下，将应用程序和数据库部署在同一区域或数据中心。考虑针对地理分布的应用程序采用边缘部署或缓存策略。

-   **批量操作：** 在插入、更新或查询多个文档时，使用批量操作以最小化网络开销，如下所示：

```
// Instead of multiple single inserts
const bulkOp = collection.initializeUnorderedBulkOp();
for (const doc of documents) {
  bulkOp.insert(doc);
}
await bulkOp.execute();
```

-   **投影：** 请求您需要的字段，而不是检索整个文档，如下所示：

```
// Only retrieve necessary fields
const result = await collection.findOne(
  { _id: documentId },
  { projection: { name: 1, email: 1 } }
);
```

有效管理数据传输通过网络压缩、通过战略部署最小化延迟、通过批处理优化操作以及利用投影减少数据检索，所有这些都在提升 MongoDB 应用程序整体性能方面发挥着至关重要的作用。通过关注这些领域，开发人员可以显著提高数据交换的效率和速度，确保最终用户获得更顺畅、更具响应性的应用程序体验。

## 摘要

本章全面探讨了 MongoDB 的客户端库，包括驱动程序和 ODM，它们充当应用程序与数据库之间的关键接口。我们首先定义了 MongoDB 驱动程序在促进特定语言的高性能通信中的基本作用，并探讨了关键特性，例如通过共享规范实现一致性、针对不同语言量身定制的惯用体验、性能优化技术（如连接管理）以及高级功能（如事务支持）。然后我们深入探讨了 ODM 的世界，强调了它们通过模式强制、直观的查询 API 和关系管理简化开发的好处，并检查了流行的解决方案，如 Mongoose、Mongoid、Prisma、Spring Data 和 Django。

我们讨论了最佳实践的重要性，包括连接和错误管理、查询优化、读/写关注以及监控，并探讨了应用程序框架如何与 MongoDB 集成并利用 ODM 以提高生产力。最后，我们超越了基础知识，解决了异步模式、网络性能优化以及处理故障和重试等高级主题。掌握这些方面使开发人员能够构建的不仅是功能性的，而且是高性能、可扩展且具有弹性的 MongoDB 应用程序，充分利用数据库在实际部署中的能力。

在下一章中，我们将基于您对客户端库的了解，深入探讨连接管理和网络性能，这些是决定应用程序在幕后与 MongoDB 通信有效性的关键因素。

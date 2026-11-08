
## 使用订阅实现 JWT 身份验证

本节介绍如何通过 WebSocket 实现身份验证。有关更多信息，请参阅 https://www.apollographql.com/docs/apollo-server/data/subscriptions/#operation-context 的文档。

到目前为止，我们的订阅尚未安全，因此我们需要阻止未经过身份验证的用户订阅。这意味着只有在用户经过身份验证时才允许 WebSocket 连接。

我们可以以与查询和变更相同的方式保护我们的订阅。我们只需在用户每次通过 WebSocket 连接时向我们的订阅传递一个上下文参数即可！

当我们创建 SubscriptionServer 实例时，可以使用在每次 WebSocket 连接之前执行的 onConnect 函数。此函数接受 ConnectionParams 类型的对象作为其参数之一。如果它返回一个对象，则该对象会作为上下文传递给解析器。

使用 ConnectionParams，我们可以获取客户端发送的 JWT。在 onConnect 内部，我们将从 JWT 中提取经过身份验证的用户并将其添加到上下文中：

- 1. 打开 server/src/index.ts 文件并添加以下导入：

```
import { ApolloServer, AuthenticationError } from 'apollo-server-express';
import { SubscriptionServer, ConnectionParams } from 'subscriptions-transport-ws';
```

我们将 AuthenticationError 添加到来自 apollo-server-express 的现有导入声明中，并将 ConnectionParams 添加到来自 subscriptions-transport-ws 的现有导入声明中。

- 2. 接下来，将 onConnect 函数添加到传递给 SubscriptionServer.create() 方法的对象中，并提取可以从传递给 onConnect 的参数中检索到的令牌：

```
const subscriptionServer = SubscriptionServer.create(
{
  schema, execute, subscribe, onConnect:
    (connectionParams: ConnectionParams) => {
const token = connectionParams.get('authToken')
    || '';
    if (token != '') {
        const authUser = getAuthUser(token.split(' ')[1]);
        return {
            authUser: authUser
        }
    }
    throw new AuthenticationError('User is not authenticated');
    }
},
    { server: httpServer, path: server.graphqlPath }
);
```

我们现在已经学会如何保护我们的 WebSocket 连接，以防止未经过身份验证的用户订阅实时事件。

### 摘要

在本章中，我们学习了如何为服务器应用程序添加实时支持，以便在新评论和点赞添加到用户帖子时立即将它们从服务器推送到客户端。为此，我们使用了 Apollo Server 中的 GraphQL 订阅。

接下来，我们实现了 JWT 身份验证，以防止未经过身份验证的用户订阅这些实时订阅。

现在我们拥有了一个功能齐全的 GraphQL API，其中包含实时订阅和 JWT 身份验证，在下一章中我们将开始实现 Angular 前端来构建社交网络应用程序的用户界面。

## 第 2 部分：使用实时支持构建 Angular 前端

在本部分中，我们将使用 Angular 和 Apollo 实现应用程序的前端。我们将在各个模块和组件之间添加路由和导航。

我们将了解如何使用 Apollo Client 将前端与后端集成，Apollo Client 旨在向服务器发送 GraphQL 查询和变更以获取和写入数据，然后我们将实现身份验证系统。

之后，我们将通过添加上传用户照片和封面图片以及个人简介的功能来实现个人资料组件的功能。

我们将添加必要的代码来获取与个人资料 URL 对应的用户并在页面上渲染其信息，然后我们将学习如何发送查询以接收分页帖子和评论相关数据，以及发送变更以向帖子添加评论和点赞。

本节包含以下章节：

- 第 6 章，Angular 应用程序架构和路由
- 第 7 章，添加用户搜索功能
- 第 8 章，保护路由和测试身份验证
- 第 9 章，上传图片和添加帖子
- 第 10 章，获取帖子并添加评论和点赞

## 6 Angular 应用程序架构和路由

在之前的章节中，我们使用 GraphQL 和 Apollo Server 开发了后端，并且还使用 Apollo Studio 与我们的 GraphQL API 通信。我们现在可以开始开发前端应用程序来使用 API 并向用户展示用户界面以与后端通信。

在接下来的章节中，我们将重点关注通过 Apollo Client 将前端应用程序与后端 API 连接起来并发送查询以获取和变更数据，但在开始之前，我们需要使用基本布局构建一个 Angular 前端项目。

在本章中，我们将首先安装 **Angular CLI** 并使用最新版本的 Angular 创建一个新项目。之后，我们将利用 Angular CLI 创建构成应用程序用户界面的 **模块**、**服务** 和 **组件**，并向您介绍 **依赖注入**。

在创建项目所需的各种工件之后，我们将向您介绍 **Angular Router** 并演示如何实现路由和导航以开发单页应用程序。之后，我们将指导您使用当前的 **Angular Ivy** 运行时进行调试，这将允许您从控制台访问应用程序组件、执行方法，甚至触发变更检测。最后，我们将在项目中安装和配置 **Angular Material**，以提供用于开发应用程序用户界面的组件。

以下主题将被涵盖：

- 安装 Angular CLI
- 初始化 Angular 项目
- 理解应用程序架构
- 理解服务和依赖注入
- 创建模块、服务和组件
- 理解 Angular 路由
- 理解并添加路由
- 添加导航
- 调试 Angular 应用程序
- 添加 Angular Material

### 技术要求

为了按照本章中的说明操作，您必须在本地开发机器上安装 Node.js 和 npm。如果您尚未安装，请参阅本书的 *第 1 章，应用程序架构和开发环境* 以获取安装说明。

您还必须完成之前的章节，以便开发将在本章开始实现的前端使用的 GraphQL API。

您还应熟悉以下技术：

- JavaScript/TypeScript
- HTML

您可以在 https://github.com/PacktPublishing/Full-Stack-App-Development-with-Angular-and-GraphQL/tree/main/Chapter06 找到本章的源代码。

不需要 TypeScript 专业知识，但了解类和装饰器等概念会有所帮助。

TypeScript 类的示例如下：

```typescript
class Product {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  getName() {
    return this.name;
  }
}

let product1 = new Product("product1");
```

如果您之前使用过基于类的面向对象编程语言，您应该熟悉这种语法，但即使是 JavaScript 的最新版本，例如 ECMAScript 6 或 JavaScript 2015，也允许开发人员使用这种语法定义类。

我们只需使用 class 关键字，后跟类名和类体来定义类。

类的成员被添加到用大括号包围的类体中。在这种情况下，我们有一个名为 name 的字符串类型属性、一个构造函数和一个名为 getName() 的方法。

最后，我们使用 new 关键字创建 Product 类的实例，这将创建一个具有 Product 类型的新对象并调用类的构造函数来初始化它。

类可以被视为创建用户定义类型的机制，从而让您轻松表示现实世界中的对象。

在 TypeScript 中，装饰器是一种特殊结构，允许您动态地修改类、方法、访问器、属性和参数。

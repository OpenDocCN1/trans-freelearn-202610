
```
const app: Application = express();
const server : ApolloServer =
    new ApolloServer({schema});
await server.start();
server.applyMiddleware({
    app,
    path: '/graphql'
});
app.listen(PORT, () => {
    console.log('Server is running at
                http://localhost:${PORT}');
});
}
startApolloServer();
```

我们使用 import 语句导入 schema 并将其传递给 Apollo Server。如果再次运行服务器，你应该能够像之前那样与 Apollo Studio 交互。

现在，让我们为我们的小型社交网络应用添加类型。

6. 进入 src/graphql/schema.graphql 文件并按如下方式添加 User 类型：

```
type User {
    id: ID!
    fullName: String!
    bio: String
    email: String!
    username: String!
    password: String!
    image: String
    coverImage: String
    postsCount: Int!
    createdAt: String!
}
```

7. 接下来，按如下方式添加 Post 类型：

```
type Post {
  id: ID!
  text: String
  image: String
  author: User!
  commentsCount: Int!
  likesCount: Int!
  likedByAuthUser: Boolean
  latestComment: Comment
  latestLike: String
  createdAt: String!
}
```

8. 按如下方式添加 Comment 类型：

```
type Comment {
  id: ID!
  Comment: String!
  Author: User!
  post: ID!
  createdAt: String!
}
```

9. 按如下方式添加 Like 类型：

```
type Like {
  id: ID!
  user: User!
  post: ID!
  createdAt: String!
}
```

10. 按如下方式添加 Notification 类型：

```
type Notification {
  id: ID!
  text: String!
```

```
postId: ID!
createdAt: String!
}
```

11. 然后，我们需要按如下方式添加查询：

```
type Query {
  message: String!
  getUser(userId: ID!): User
  getPostsByUserId(userId: ID!, offset: Int, limit:
    Int): [Post]
  getFeed(offset: Int, limit: Int): [Post]
  getNotificationsByUserId(userId: ID!, offset: Int,
    limit: Int): [Notification]
  getCommentsByPostId(postId: ID!, offset: Int, limit:
    Int): [Comment]
  getLikesByPostId(postId: ID!, offset: Int, limit:
    Int): [Like]
  searchUsers(searchQuery: String): [User]
}
```

12. 最后，按如下方式添加变更：

```
type Mutation {
  post(text: String, image: String): Post
  removePost(id: ID!): ID
  comment(comment: String!, postId: ID!): Comment
  removeComment(id: ID!): Comment
  like(postId: ID!): Like
  removeLike(postId: ID!): Like
  removeNotification(id: ID!): ID
}
```

这是我们将在后续章节中不断改进的初始 schema。在继续实现 GraphQL API 之前，让我们学习如何使用 Apollo Server 的模拟功能（如果你计划遵循 **GraphQL优先开发** 流程，这是一个重要步骤）。

## 使用 GraphQL API 模拟

模拟允许前端开发者在后端开发完成之前开始构建 UI。它还让我们可以在不等待缓慢的数据库操作或运行完整的 GraphQL 服务器的情况下测试 UI。

社区提供了许多工具，可以轻松地基于 GraphQL schema 和查询进行请求模拟，例如 graphql-tools、graphql-faker 以及 Apollo Server 的内置模拟功能。

由于我们已经在使用 Apollo Server，让我们在实现解析器之前使用其模拟功能来模拟我们的 GraphQL 请求。

打开 src/index.ts 文件并按如下方式向 Apollo Server 添加 mocks: true：

```
const server : ApolloServer = new ApolloServer({schema ,
  mocks: true});
```

Apollo Server 的内置模拟非常基础。它只是查看类型定义并返回与对应字段相同类型的值。

前往 Apollo Studio 并添加以下查询：

```
query {
  searchUsers(searchQuery:"a") {
    id fullName email postsCount
  }
  getPostsByUserId(userId:"1") {
    id text author { id fullName } commentsCount
    latestComment { id comment }
    createdAt
  }
}
```

以下截图显示了查询结果：

![](img/c798729e4c42765fcb1c98950164d0ba_57_0.png)

图 2.3 – 使用 Apollo Studio 的查询结果

在这里，我们可以看到，默认情况下，每个预期返回列表值的查询只返回两个值，并且每个字符串值字段都返回 Hello World。这提供了结果的正确结构，但离真实情况还很远。

幸运的是，我们可以在 Apollo Server alongside 使用其他工具，例如 casual，这是一个用于 JavaScript 的假数据生成器，它允许我们在实现所需解析器之前从 GraphQL 查询中返回更真实和随机的值。

返回到你的终端，并在 server/ 文件夹的根目录下运行以下命令：

```
npm install casual
```

接下来，返回 src/index.ts 文件并添加以下导入：

```
import casual from 'casual';
```

接下来，按如下方式添加一个 mocks 对象：

```
const mocks = {};
```

接下来，通过在 mocks 对象内部添加以下代码来模拟 User 类型：

```
User: () => ({
    id: casual.uuid,
    fullName: casual.full_name,
    bio: casual.text,
    email: casual.email,
    username: casual.username,
    password: casual.password,
    image: 'https://picsum.photos/seed/picsum/200/300',
    coverImage:
      'https://picsum.photos/seed/picsum/200/300',
    postsCount: () => casual.integer(0)
  }),
```

接下来，按如下方式模拟 Post 类型：

```
Post: () => ({
    id: casual.uuid,
    text: casual.text,
    image: 'https://picsum.photos/seed/picsum/200/300',
    commentsCount: () => casual.integer(0),
    likesCount: () => casual.integer(0),
    latestLike: casual.first_name,
    createdAt: () => casual.date()
  }),
```

接下来，按如下方式模拟 Comment 类型：

```
Comment: () => ({
    id: casual.uuid,
    comment: casual.text,
    post: casual.uuid,
    createdAt: () => casual.date()
  }),
```

接下来，按如下方式模拟 Like 类型：

```
Like: () => ({
  id: casual.uuid,
  post: casual.uuid
})
```

对于 schema 中每个类型的每个字段，我们可以使用 casual 为其分配常量值或随机值，casual 为最常用的数据类型提供了生成器。你可以在 https://github.com/boolean/casual 找到可用属性和方法的列表。

默认情况下，我们的模拟解析器对查询只返回两个结果。我们可以按如下方式更改此默认行为。在同一个 mocks 对象中，你需要添加以下对象：

```
Query: () => ({
  getPostsByUserId: () =>
    [...new Array(casual.integer(10, 100))],
  getFeed: () => [...new Array(casual.integer(10, 100))],
  getNotificationsByUserId: () =>
    [...new Array(casual.integer(10, 100))],
  getCommentsByPostId: () =>
    [...new Array(casual.integer(10, 100))],
  getLikesByPostId: () =>
    [...new Array(casual.integer(10, 100))],
  searchUsers: () =>
    [...new Array(casual.integer(10, 100))]
})
```

现在，我们的查询将返回 10 到 100 条假条目。有关更多详细信息，请参阅 https://www.graphql-tools.com/docs/mocking#using-lists-in-mocks。

按如下方式更新传递给 ApolloServer 的对象：

```
const server: ApolloServer = new ApolloServer({schema,
  mocks, mockEntireSchema: false});
```

如果你与开发团队一起工作，前端开发者可以开始构建前端，因为他们可以与 GraphQL API 交互并获取生成的数据，而无需等待后端开发者实现解析器。出现的一个问题是，在现实场景中应链接的条目在我们的模拟 API 中并未链接——让我们学习如何更改这一点！

## 链接模拟数据

帖子应属于用户，因此应通过 user 字段链接到用户。然而，该字段的随机生成值不保证是为用户标识符生成的其中一个值。我们可以通过在 src/index.ts 文件中模拟对象之前定义两个数组来轻松解决此问题，如下所示：

```
let postsIds: string[] = [];
let usersIds: string[] = [];
```

这些数组将允许我们跟踪用户和帖子生成的标识符，以确保 Post、Comment 和 Like 类型的 user 和 post 字段接收这些值中的一个。这样，我们生成的数据将更真实，更类似于现实世界的示例。

接下来，将 mock User 对象上 id 字段的生成器更改为以下函数：

```
id: () => {let uuid = casual.uuid; usersIds.push(uuid);
        return uuid},
```

这确保生成的 ID 被添加到 usersIds 数组中。

接下来，你需要将模拟的 Post 类型的 id 字段更改为以下函数：

```
id: () => {let uuid = casual.uuid; postsIds.push(uuid);
        return uuid},
```

同样，这确保生成的帖子 ID 被添加到 postsIds 数组中。

接下来，按如下方式向模拟的 Post 对象添加 author 字段：

```
author: casual.random_element(usersIds),
```

接下来，按如下方式更改模拟的 Comment 对象的 author 字段：

```
author: casual.random_element(usersIds),
```

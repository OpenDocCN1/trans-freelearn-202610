## 5 使用 Apollo Server 添加实时支持

在上一章中，我们了解了如何向我们的 **GraphQL** API 添加身份验证和图片上传。我们还了解了如何保护端点免受非认证用户的访问。

传统上，在 Web 应用程序中，客户端向服务器发送 HTTP 请求并接收响应。之后，连接会被关闭。因此，如果服务器上的数据发生变化，客户端没有机制知道这一点。

如今，当代应用程序实时运行，这意味着后端服务器可以在数据发生变化时随时提醒客户端，即使它们没有发送另一个请求，因为连接保持打开状态，以便服务器在更新可用时进行传输。

在本章中，我们将为服务器应用程序添加实时支持，这将使我们能够在数据可用时立即将最新数据从服务器传达给客户端。为此，我们将利用 Apollo Server 的 GraphQL subscriptions。



We will address the following topics:
- Understanding GraphQL subscriptions
- Implementing GraphQL subscriptions with Apollo Server
- Implementing JSON Web Token (JWT) authentication with subscriptions

## 技术要求

To accomplish the steps in this chapter, you must have completed the previous chapter's steps. Refer to *第 4 章，使用 Apollo Server 实现身份验证和图像上传*, of this book for instructions.

You also need to be familiar with the following technologies:
- TypeScript
- Node.js
- GraphQL basics

You can find the code source for this chapter in the `chapter5` folder of our GitHub repository or via this link: at https://github.com/PacktPublishing/Full-Stack-App-Development-with-Angular-and-GraphQL/tree/main/Chapter05.

## 理解 GraphQL 订阅

When interacting with a GraphQL server, you'll often get and write data using queries and mutations. In this situation, the client must send a query or mutation to the server in order to obtain a response.

**Subscriptions** are another form of data communication in GraphQL, in which the server provides realtime data to listening clients.

In order to do that, the client opens a two-way communication with the server, which can be done with protocols such as WebSocket instead of HTTP. WebSocket can be used to make two-way communication between the user's browser and a server.

Subscriptions are similar to queries. However, they let subscribing clients receive fresh or updated data whenever a certain server event happens. You may configure Apollo Server to use a subscription-specific endpoint that is distinct from the default endpoint for queries and mutations.

We will be able to add realtime capabilities to our social network application thanks to GraphQL subscriptions. We may create them in the same way that we define queries, with a group of fields indicating the form of data required by the client, but under a Subscription type in our schema.

Now that we've defined GraphQL subscriptions, we'll see how to use them with Apollo Server to provide realtime capabilities to our social network application.

## 实现 GraphQL 订阅

After explaining GraphQL subscriptions, let's look at how we can use Apollo Server to implement them in our application. Because subscriptions utilize WebSocket rather than HTTP, we must use a second GraphQL endpoint created for subscriptions that uses the WebSocket protocol rather than HTTP:

1. Begin by installing the packages listed as follows:
2. Add the following imports to the server/src/index.ts file:
3. Following that, we must launch two servers, HTTP and WebSocket. As a result, we must create http.Server by providing our Express app to the createServer() function in the body of the startApolloServer() function, as seen here:

```
npm install subscriptions-transport-ws graphql-subscriptions
```

```
import { createServer } from 'http';
import { execute, subscribe } from 'graphql';
import { SubscriptionServer } from 'subscriptions-transport-ws';
```

```
async function startApolloServer() {
  const PORT = 8080;
  const app: Application = express();
  // [...]
  const httpServer = createServer(app);
```

4. Next, create a SubscriptionServer instance beneath the ApolloServer instance:

```
const subscriptionServer = SubscriptionServer.create(
  { schema, execute, subscribe },
  { server: httpServer, path: server.graphqlPath }
);
```

5. Then, in the ApolloServer constructor, add a plugin to close the SubscriptionServer instance.

First, add the plugins array to the constructor, as follows:

```
const server: ApolloServer = new ApolloServer({ schema,
context: ({req}) => {
  // [...]
  return ctx;
}, plugins: [] });
```

Then, inside the plugins array, add the following code:

```
{
  async serverWillStart() {
    return {
      async drainServer() {
        subscriptionServer.close();
      }
    };
  }
}
```

6. Update the listen() call from app.listen() to httpServer.listen():

```
httpServer.listen(PORT, () => {
  console.log('Server is running at http://localhost:${PORT}');
});
```

This will allow us to launch both servers. Check out this commit for the updates we've made in this step: https://git.io/JghrI.

7. Next, we need to update the schema, open the server/src/graphql/schema.graphql file, and add the following type:

```
type Subscription {
  onPostCommented: Comment
  onPostLiked: Like
}
```

We added a Subscription type that defines the top-level fields that clients can subscribe to.

The onPostCommented field will update its value whenever a new comment is created on the backend and send the comment to subscribing clients.

Before continuing, you need to regenerate the resolvers' types using the npm run codegen command from the root of your monorepo project.

Next, we need to implement the resolvers for these two fields. Resolvers for these fields are different from the resolvers for the query and mutation fields. They are not functions but objects that should define a subscribe function. This function must return an object of the AsyncIterator type, a standard interface for iterating over asynchronous results:

1. Open the server/src/graphql/resolvers.ts file and start by adding the following code:

```
import { PubSub } from 'graphql-subscriptions';
const pubsub = new PubSub();
```

We imported the PubSub interface and created an instance that will allow you to publish events to a particular label and listen for events associated with a particular label.

Apollo Server adopts a publish-subscribe mechanism to track events for updating active subscriptions.

2. Next, add a Subscription object inside the resolvers object, as follows:

```
Subscription: {
  onPostCommented: {
    subscribe: () =>
      pubsub.asyncIterator(['ON_POST_COMMENTED'])
  },
  onPostLiked: {
    subscribe: () =>
      pubsub.asyncIterator(['ON_POST_LIKED'])
  }
}
```

For each field, we added a subscribe() function and mapped the corresponding event using the pubsub.asyncIterator() method.

3. Next, you need to publish some events with the publish() method of the PubSub object.

Update the comment mutation, as follows:

```
comment: async (_, args, { orm, authUser }:
    Context) => {
    // [...] 
    pubsub.publish('ON_POST_COMMENTED',
    { onPostCommented: savedComment });
    return savedComment as unknown as Comment;
}
```

Then, update the like mutation, as follows:

```
like: async (_, args, { orm, authUser }: Context) => {
    // [...] 
    pubsub.publish('ON_POST_LIKED',
    { onPostLiked: savedLike });
    return savedLike as unknown as Like;
}
```

Apollo Server uses the payloads of the ON_POST_COMMENTED and ON_POST_LIKED events to push updated values for the onPostCommented and onPostLiked fields respectively.

4. We can write the GraphQL query for listening for subscriptions in the same way we write other types of queries. Head over to http://localhost:8080/graphql and write the following subscription in the left panel of Apollo Studio:

```
subscription {
    onPostCommented {
        comment
        post {
            id
        }
    }
}
```

After running this subscription, we will listen for comments on posts over WebSocket. In Apollo Studio, this will not return data immediately like queries and mutations. Instead, we'll get data every time a comment is added.

If you want to see some data pushed to an onPostCommented subscription, you simply need to create a comment.

You can start listening for new likes using the following subscription:

```
subscription {
  onPostLiked {
    id
    post {
      id
      likesCount
    }
  }
}
```

This is an example of a response I received after liking the post with the 1 ID:

![](img/c798729e4c42765fcb1c98950164d0ba_137_0.png)

Figure 5.1 – 订阅的响应

Now that we have implemented subscriptions to add realtime features to our social application, let's see how to secure these subscriptions using JWT authentication.



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

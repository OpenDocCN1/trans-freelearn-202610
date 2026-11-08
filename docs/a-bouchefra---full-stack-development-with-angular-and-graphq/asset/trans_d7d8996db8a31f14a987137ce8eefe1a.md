
这是我收到的响应：

![](img/c798729e4c42765fcb1c98950164d0ba_125_0.png)

图 4.4 - post mutation 的响应

你可以看到作者与我们使用 signIn mutation 登录时使用的已认证用户相同。它已成功从我们通过 authorization header 传递的 token 中检索出来。这表明我们的实现按预期工作。

接下来，让我们在这篇文章上创建一些评论和点赞：

```graphql
mutation CreateComments {
    c1: comment(comment: "Comment on post with id #2",
        postId: 2 ) {
        id comment author { id fullName } post { id }
    }
    c2: comment(comment: "Comment on post with id #3",
        postId: 3 ) {
        id comment author { id fullName } post { id }
    }
    c3: comment(comment: "Comment on post with id #4",
        postId: 4 ) {
        id comment author { id fullName } post { id }
    }
}
```

在响应中，你在 selection set 中收到的 post ID 应与你传递给 mutation 的 postId 参数相同。这意味着我们在 resolver 中正确地检索了关系。此外，selection set 中的作者应与已认证用户相同。

让我们以同样的方式创建点赞：

```graphql
mutation CreateLikes {
  like1: like(postId: 1) {
    id user { fullName } post { id }
  }
  like2: like(postId: 2) {
    id user { fullName } post { id }
  }
  like3: like(postId: 3) {
    id user { fullName } post { id }
  }
}
```

我们已保护了 GraphQL 端点，仅允许已认证用户与 API 的大多数查询和 mutation 交互。我们还实现了 post、comment、like 和 removeLike mutation。接下来，我们将了解如何使用 Apollo Server 实现图片上传，以便用户上传其个人资料的照片和封面图片。

## 实现图片上传

在 GraphQL API 中实现身份验证并保护查询和 mutation 之后，现在让我们实现图片上传功能，该功能将用于上传用户的个人资料照片和封面图片。

我们将了解如何使用 Apollo Server 实现文件上传并将图片流式传输到存储服务，例如 Amazon S3 或 DigitalOcean Spaces 对象存储。我们将使用后者服务，但如果你更喜欢 Amazon S3 或任何 S3 兼容的对象存储服务，也可以随意使用。

在此之前，请确保为你选择的 S3 兼容服务注册，创建 S3 存储桶，并记下以下信息：

- S3_ACCESS_KEY_ID
- S3_SECRET_ACCESS_KEY
- S3_ENDPOINT
- S3_BUCKET

使用服务器 URL、存储桶名称（或 Space 名称）、访问密钥和密钥，你可以将任何 S3 兼容的客户端或库连接到你的 DigitalOcean Space。Space 等同于存储桶。

DigitalOcean Spaces API 与 Amazon S3 REST API 互操作，因此我们将使用 AWS SDK 与 Spaces 交互：

- 1. 打开 server/src/graphql/schema.graphql 文件并在顶部添加以下行：
- 2. 然后，添加以下类型：
- 3. 接下来，添加以下 mutation：
- 4. 接下来，你需要返回到 Terminal 并从 server 文件夹运行以下命令：

```
scalar Upload
```

```
type File {
    url: String
    filename: String
    mimetype: String
    encoding: String
}
```

```
type Mutation {
    [...]
    uploadFile(file: Upload!): File!
}
```

```
npm i graphql-upload aws-sdk
npm i --save-dev @types/graphql-upload
```

这将安装 graphql-upload 及其类型，以及 aws-sdk 包。

- 5. 接下来，打开 server/src/index.ts 文件并首先添加此导入：

```
import { graphqlUploadExpress } from 'graphql-upload';
```

然后，按如下方式使用中间件：

```
async function startApolloServer() {
  const PORT = 8080;
  const app: Application = express();
  app.use(cors());
  app.use(graphqlUploadExpress());
  // [...]
```

确保在调用 server.applyMiddleware() 方法之前使用 graphqlUploadExpress 中间件。

我们现在已使用 Apollo Server 和 Express 启用了文件上传；接下来，我们需要实现 uploadFile resolver。

在此之前，打开 .env 文件并使用你自己的 S3 服务凭据添加以下变量：

```
S3_ACCESS_KEY_ID=<YOUR_ACCESS_KEY>
S3_SECRET_ACCESS_KEY=< YOUR_SECRET_KEY >
S3_ENDPOINT=<YOUR_S3_ENDPOINT>
S3_BUCKET=<YOUR_S3_BUCKET>
```

此外，请确保重新生成你的 resolver 类型（使用从 Lerna 项目根目录运行的 npm run codegen）。

- 6. Upload scalar 的实现由 graphql-upload 包中的 GraphQLUpload 导出提供，因此你需要将其添加到 resolver 的映射中以启用它。打开 server/src/graphql/resolvers.ts 文件并添加以下导入：

```
import { GraphQLUpload } from 'graphql-upload';
```

然后，将其添加到 resolvers 对象中：

```
const resolvers: Resolvers = {
  Upload: GraphQLUpload,
  Query: {...},
  Mutation: {...}
};
```

接下来，在顶部添加以下代码：

```
import AWS from 'aws-sdk';
const spacesEndpoint = new AWS.Endpoint(process.env.S3_ENDPOINT as string);
const s3 = new AWS.S3({
  endpoint: spacesEndpoint,
  accessKeyId: process.env.S3_ACCESS_KEY_ID,
  secretAccessKey: process.env.S3_SECRET_ACCESS_KEY
});
```

最后，按如下方式添加 uploadFile mutation：

```
uploadFile: async (_, { file }) => {
  const { createReadStream, filename, mimetype, encoding } = await file;
  const fileStream = createReadStream();
  const uploadParams = { Bucket: process.env.S3_BUCKET as string, Key: filename, Body: fileStream, ACL: "public-read" };
  const result = await s3.upload(uploadParams).promise();
  console.log(result);
  const url = result.Location;
  return { url, filename, mimetype, encoding };
}
```

我们解构作为参数传递给 resolver 的 file promise。这样，我们就可以获取读取流和其他信息，例如文件名、MIME 类型和编码，这些信息是使用 AWS SDK 的 upload() 方法将文件保存到 S3 服务所需的。接下来，我们获取已保存文件的 URL 并连同其他信息一起返回。

## 练习

我们将使用 uploadFile mutation 为帖子上传图片。对于上传用户的照片和封面图片，我们还需要将上传的图片链接到相应的用户。为此，我们需要添加两个 setUserPhoto 和 setUserCover mutation。

按照相同的步骤将 `setUserPhoto` 和 `setUserCover` mutation 添加到 schema。两个 mutation 都应具有 `Upload` 类型的 `file` 参数，并应返回 `User` 对象。

接下来，重新生成 resolver 的类型，然后实现所需的 resolver。

最后，使用 `@auth` 指令保护 mutation 免受未认证用户的访问。

你可以在 https://git.io/JgQiC 找到此练习的解决方案。

作为第二个练习，添加 `setUserBio` mutation（它接受 `String` 类型的 `bio` 参数并返回更新后的 `User` 对象），重新生成 resolver 的类型，并实现 mutation 的 resolver。

你可以在 https://git.io/JgQzA 的提交中找到此练习的解决方案。

## 总结

在本章中，我们了解了如何使用 JWT 向 GraphQL API 添加身份验证和图片上传，并保护端点免受未认证用户的访问。

本章到此结束！在下一章中，我们将继续构建后端应用程序的旅程，通过实现实时订阅。

## 5 使用 Apollo Server 添加实时支持

在上一章中，我们了解了如何向我们的 **GraphQL** API 添加身份验证和图片上传。我们还了解了如何保护端点免受非认证用户的访问。

传统上，在 Web 应用程序中，客户端向服务器发送 HTTP 请求并接收响应。之后，连接会被关闭。因此，如果服务器上的数据发生变化，客户端没有机制知道这一点。

如今，当代应用程序实时运行，这意味着后端服务器可以在数据发生变化时随时提醒客户端，即使它们没有发送另一个请求，因为连接保持打开状态，以便服务器在更新可用时进行传输。

在本章中，我们将为服务器应用程序添加实时支持，这将使我们能够在数据可用时立即将最新数据从服务器传达给客户端。为此，我们将利用 Apollo Server 的 GraphQL subscriptions。

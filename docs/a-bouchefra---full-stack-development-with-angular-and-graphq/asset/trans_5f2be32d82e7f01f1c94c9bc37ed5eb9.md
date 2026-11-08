
```
providers: [],
    bootstrap: [AppComponent]
})

export class AppModule { }
```

在 GraphQL 模块中，我们使用来自 apollo-angular/http 的 HttpLink 服务将客户端链接到运行在 http://localhost:8080/graphql 的 GraphQL 服务器。我们还使用来自 @apollo/client/core 的 InMemoryCache 来创建内存缓存，用于存储本地和远程数据。HttpLink 服务使用 HttpClient 与服务器通信，因此我们在应用程序根模块中还导入了来自 @angular/common/http 的 HttpClientModule。

Apollo 提供了 Apollo Link 接口，它允许你管理网络层，在该层中可以插入链接来控制客户端与 GraphQL 服务器之间的数据流，或查询的发送方式。客户端的网络行为可以描述为按顺序运行的链接链。

HttpLink 就是这些链接中的一个示例。默认情况下，Apollo Client 使用 Apollo Link 的 HttpLink 通过 HTTP 将 GraphQL 操作发送到远程服务器。此默认链接会自动创建，可满足我们的初始用例，但稍后，我们将定义更高级的链接并指定其执行顺序，以扩展默认的网络行为。

阅读 https://www.apollographql.com/docs/react/api/link/introduction/ 了解有关 Apollo Link 的更多信息，并查看社区开发的自定义链接列表 https://www.apollographql.com/docs/react/api/link/community-links/。

内存缓存是 Apollo Client 存储从 GraphQL 服务器获取的数据的位置。你可以在 https://apollo-angular.com/docs/caching/configuration/ 找到更多信息。

现在我们已将 Apollo Client 添加到应用程序中并建立了与 GraphQL 服务器的连接，我们将在下一节中了解如何导入构建应用程序 UI 所需的 Angular Material 组件。

## 导入 Angular Material 组件

每个 Angular Material 组件都包含在其自己的模块中。这些组件通常在整个应用程序的多个位置使用，这就是为什么我们必须从共享模块导入并重新导出它们，以避免在每个模块中都导入它们：

- 1. 打开 shared/shared.module.ts 文件并添加以下导入：

```typescript
import { MatToolbarModule } from '@angular/material/toolbar';
import { MatButtonModule } from '@angular/material/button';
import { MatIconModule } from '@angular/material/icon';
import { MatInputModule } from '@angular/material/input';
import { MatCardModule } from '@angular/material/card';
import { MatFormFieldModule } from '@angular/material/form-field';
import { MatTooltipModule } from '@angular/material/tooltip';
import { MatSnackBarModule } from '@angular/material/snack-bar';
import { MatDialogModule } from '@angular/material/dialog';
import { MatProgressBarModule } from '@angular/material/progress-bar';
import { MatBadgeModule } from '@angular/material/badge';
```

组件模块的名称不言自明；例如，MatToolbarModule 包含 Material Toolbar 组件，将用于构建应用程序的页眉，但你也可以查阅官方文档 https://material.angular.io/ 了解有关每个可用 UI 组件的更多信息。

- 2. 接下来，我们需要按如下方式将这些组件添加到共享模块的 imports 和 exports 数组中。

首先，将导入的模块添加到 matModules 数组中：

```typescript
const matModules = [
  MatToolbarModule,
  MatButtonModule,
  MatIconModule,
  MatInputModule,
  MatCardModule,
  MatFormFieldModule,
  MatTooltipModule,
  MatSnackBarModule,
  MatDialogModule,
  MatProgressBarModule,
  MatBadgeModule
];
```

接下来，按如下方式将它们添加到共享模块的 imports 和 exports 数组中：

```typescript
@NgModule({
  declarations: [],
  imports: [
    CommonModule, ReactiveFormsModule, ...matModules
  ],
  exports: [ReactiveFormsModule, ...matModules]
})
export class SharedModule { }
```

由于这些 Angular Material 组件需要同时添加到共享模块的 imports 和 exports 数组中，我们只将它们添加到 matModules 数组一次，然后使用 Spread 运算符将其包含在内。查看 https://basarat.gitbook.io/typescript/future-javascript/spread-operator。

在导入并重新导出必要的 Material UI 组件后，我们将了解如何添加用户注册和身份验证功能，包括如何使用 UI 组件构建注册和登录界面。

## 注册和登录用户

在本节中，我们将了解如何使用 Apollo Client 和 Angular 的 Apollo 库为 Angular 前端添加用户身份验证。

### 定义 GraphQL 文档

让我们首先定义用户注册、登录和搜索所需的 GraphQL 文档（查询和变更）：

- 1. 创建 shared/constants/auth.ts 文件（确保在 shared/ 文件夹内创建 constants/ 文件夹）并首先添加以下导入和两个字符串常量，其中包含访问令牌和已认证用户的键，这些键将用于在用户浏览器的本地存储中存储相应信息：
- 2. 添加并导出发送到后端 API 用于注册用户的 register 变更：

```typescript
import { gql } from 'apollo-angular';
export const ACCESS_TOKEN: string = 'accessToken';
export const AUTH_USER: string = 'authUser';
```

```typescript
export const REGISTER_MUTATION = gql`
mutation register($fullName: String!, $username: String!, $email: String!, $password: String!){
  register(fullName:$fullName, username:$username, email:$email, password:$password){
    token, user { id fullName bio email username image coverImage postsCount createdAt }
  }
}
`;
```

我们使用 gql 函数包装查询和变更字符串，使其被解析为 GraphQL 文档。它是一个模板字面量标签，用于将查询、变更、订阅和片段传递给 Apollo Client。它返回 DocumentNode 类型的对象，即解析后 GraphQL 查询字符串的对象表示形式。参见 https://git.io/JikfN。

我们使用了 mutation 操作类型，因为我们需要在 GraphQL 服务器上调用一个变更。我们将其命名为 register，并使用 GraphQL 变量提供了必要的参数（因为我们需要动态传递用户的全名、用户名、电子邮件和密码以便为他们注册账户）。

在我们的变更体中，我们只是请求了 register 字段，该字段我们之前已添加到 GraphQL 服务器模式的 Mutation 类型中，并且由于相应的解析器将返回包含身份验证令牌和保存在数据库中的用户的对象，因此我们指定了一个选择集来请求所需字段。

查看 https://graphql.org/learn/queries/ 了解有关操作类型的更多信息。

- 3. 添加并导出用于身份验证用户的登录变更：

```ts
export const LOGIN_MUTATION = gql`
mutation signIn($email: String!, $password: String!){
  signIn(email:$email, password:$password){
    token, user { id fullName bio email username image coverImage postsCount createdAt }
  }
}
`;
```

再次，我们使用了 gql 模板字面量来构建将发送到服务器以验证用户的 GraphQL 文档。我们使用了 mutation 类型来指示服务器调用顶级 Mutation 类型上的 signIn 字段，并使用变量传递动态参数，例如名称和密码，这些参数是验证用户所必需的。

由于 signIn 变更字段引用了一个包含令牌字符串和用户对象的对象，我们使用选择集来请求客户端所需的所有信息，包括 JWT 和已认证用户。

- 4. 创建 shared/constants/user.ts 文件并按如下方式导入 gql 标签：

```ts
import { gql } from 'apollo-angular';
```

然后，添加并导出用于按 ID 获取用户的用户查询：

```typescript
export const USER_QUERY = gql`
query getUser($userId: ID!){
  getUser(userId:$userId){
    id fullName bio email username image coverImage postsCount createdAt }
}
`;
```

在这里，我们使用 query 操作类型并将用户 ID 作为参数传递，以要求 API 服务器返回具有该 ID 的现有用户。服务器上的 Query 类型包含我们正在查询的 getUser 字段，并且由于它返回一个用户对象，因此我们在客户端提供一个选择集来请求我们感兴趣的字段。

- 5. 添加并导出以下用于搜索用户的查询：

```typescript
export const SEARCH_USERS_QUERY = gql`
query searchUsers($searchQuery: String!){
  searchUsers(searchQuery:$searchQuery){
    id fullName bio username image
  }
}
`;
```

在这里，我们使用 query 操作类型来请求 GraphQL 模式顶级查询上的 searchUsers 字段，该字段返回一个包含作为参数传递的搜索词的全名用户的数组。然后我们指定一个选择集来选择我们感兴趣的用户字段，例如用户的 ID、全名、简介、用户名和图像。

- 6. 最后，创建 shared/index.ts 文件（桶文件）并按如下方式导出已定义的常量：

```typescript
export * from './constants/auth';
export * from './constants/user';
```

在定义将通过 Apollo Client 解析并发送到服务器的查询和变更之后，我们必须定义将从 API 服务器返回的响应/结果的类型。

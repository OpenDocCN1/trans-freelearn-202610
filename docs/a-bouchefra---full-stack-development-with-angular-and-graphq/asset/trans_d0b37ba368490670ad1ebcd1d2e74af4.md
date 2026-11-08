
## 为用户认证定义类型

TypeScript 是一种强类型编程语言。我们将使用它来编写类型安全代码，这有助于防止意外错误并为 IDE 提供自动完成功能以提高生产力。

让我们看看如何创建 TypeScript 类型以对服务器响应进行强类型化。我们将从 User 模型开始，它表示单个用户并封装了对应于 GraphQL API 用户信息的字段：

- 1. 创建 shared/models/user.model.ts 文件并添加，然后导出一个 TypeScript 接口，其中包含以下引用用户详细信息及其类型的字段：
- 2. 创建 shared/types/auth.ts 文件并首先导入 User 模型：

```ts
export interface User {
  id: string;
  fullName: string;
  bio: string;
  username: string;
  email: string;
  image: string;
  coverImage: string;
  postsCount: number;
  createdAt: string;
}
```

前面的代码块指定了 GraphQL API 返回的用户数据的格式。

```ts
import { User } from '../models/user.model';
```

然后，为了对认证状态进行强类型化，定义并导出以下接口：

```ts
export interface AuthState {
  isLoggedIn: boolean;
  currentUser: User | null;
  accessToken: string | null;
};
```

接下来，定义并导出 register 和 signIn 突变的响应类型：

```ts
export type AuthResponse = {
    token: string;
    user: User;
};
export type RegisterResponse = {
    register: AuthResponse;
};
export type LoginResponse = {
    signIn: AuthResponse;
};
```

3. 创建 shared/types/user.ts 文件并首先添加以下导入：

```ts
import { Observable } from 'rxjs';
import { User } from '../models/user.model';
```

接下来，定义并导出以下类型以对 getUser 和 searchUsers 查询的响应进行类型化：

```ts
export type UserResponse = {
    getUser: User;
};
export type UsersResponse = {
    searchUsers: User[];
};
export type SearchUsersResponse = {
    data: Observable<UsersResponse>;
    fetchMore: (users: User[]) => void
};
```

我们从 RxJS 导入了 Observable 类并导入了之前定义的 User 模型，然后定义并导出了一组类型以对服务器返回的响应进行类型化。这些类型定义了服务器为每个操作返回的响应的数据形状。我们将使用 SearchUsersResponse 来为包含搜索用户查询响应以及用于分页的获取更多数据的方法的变量进行类型化。

4. 最后，打开 shared/index.ts 文件并添加以下导出：

```ts
export { User } from './models/user.model';
export * from './types/user';
export * from './types/auth';
```

在下一节中，我们将了解如何构建服务来封装发送用于用户认证的查询和突变的请求的逻辑。

我们将用户搜索功能添加到认证服务中，因为它将对我们的应用程序全局可用，并在仅在我们懒加载用户模块时才加载的个人资料服务之前访问。

## 定义 Apollo 服务

在定义了 GraphQL 查询和突变以及请求发送到服务器后将返回的响应类型之后，我们现在必须创建一系列用于注册、认证和搜索用户的服务。

Angular 的 Apollo Client 库支持两种与服务器通信的方法：

- 使用 Apollo 服务，一个 Angular 服务，其中包含诸如 `query()` 和 `watchQuery()` 等方法来发送查询，以及 `mutate()` 和 `subscribe()` 来发送突变和订阅。
- 内置的 `Query`、`Mutation` 和 `Subscription` 服务。

Mutation、Query 和 Subscription 是泛型 TypeScript 类，它们在类名后接受尖括号之间的泛型类型参数。我们为每个服务指定预期的响应类型以及将发送到服务器的突变、查询或订阅的 document 属性。查看 https://apollo-angular.com/docs/data/services。

> **注意**
> 有关 TypeScript 中泛型类的更多信息，请查看 https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-classes。

在本书中，我们将使用这两种方法与后端通信。更多信息可以在官方文档 https://apollo-angular.com/docs/ 中找到。

现在，让我们创建用于注册、认证和搜索用户的服务：

- 1. 创建 core/services/auth/graphql/register.service.ts 文件并添加以下代码：
- 2. 创建 core/services/auth/graphql/login.service.ts 文件并添加以下代码：
- 3. 创建 core/services/auth/graphql/getuser.service.ts 文件并添加以下代码：

```ts
import { Injectable } from '@angular/core';
import { Mutation } from 'apollo-angular';
import { RegisterResponse, REGISTER_MUTATION } from
  'src/app/shared';

@Injectable({
  providedIn: 'root',
})
export class RegisterGQL extends
  Mutation<RegisterResponse> {
    document = REGISTER_MUTATION;
  }
```

```ts
import { Injectable } from '@angular/core';
import { Mutation } from 'apollo-angular';
import { LOGIN_MUTATION, LoginResponse } from
  'src/app/shared';

@Injectable({
  providedIn: 'root',
})
export class LoginGQL extends Mutation<LoginResponse> {
  document = LOGIN_MUTATION;
}
```

```ts
import { Injectable } from '@angular/core';
import { Query } from 'apollo-angular';
import { UserResponse, USER_QUERY } from
  'src/app/shared';
```

```ts
@Injectable({
  providedIn: 'root',
})
export class GetUserGQL extends Query<UserResponse> {
  document = USER_QUERY;
}
```

我们导入必要的 API，例如 Injectable 装饰器用于装饰类以定义 Angular 服务，例如 apollo-angular 的 Mutation 和 Query 服务、包含将发送到服务器的查询和突变的常量以及响应类型。

接下来，我们通过扩展 Mutation 或 Query 服务并将 document 属性设置为适当的查询或突变来创建我们的 GraphQL 服务。

封装用户认证功能的认证服务将接下来实现。

## 实现用户认证服务

认证服务是 app 模块的一部分，所有之前定义的 Apollo 服务将被注入到其中。

该服务将包含用户的认证逻辑，并为用户的组件提供注册、登录、注销、检查认证状态、按 ID 获取特定用户以及最后搜索用户的方法。

让我们通过以下步骤开始实现认证服务：

- 1. 打开 core/services/auth/auth.service.ts 文件并首先添加这些导入：

```ts
import { Apollo } from 'apollo-angular';
import { Observable, BehaviorSubject } from 'rxjs';
import { map, tap } from 'rxjs/operators';
```

此外，从 shared 模块添加以下导入：

```ts
import {
  User,
  RegisterResponse,
  LoginResponse,
  AuthState,
  UserResponse,
  UsersResponse,
  SearchUsersResponse,
  SEARCH_USERS_QUERY,
  ACCESS_TOKEN,
  AUTH_USER
} from 'src/app/shared';
```

接下来，从相应的源文件导入 GraphQL 服务：

```ts
import { GetUserGQL } from
'./graphql/getuser.service';
import { LoginGQL } from './graphql/login.service';
import { RegisterGQL } from
'./graphql/register.service';
```

2. 定义以下公共属性和私有属性：

```ts
authState: Observable<AuthState>;
isLoggedInAsync: Observable<boolean>;
private readonly authSubject:
BehaviorSubject<AuthState>;
```

我们定义了两个 Observables，authState 和 isLoggedInAsync，以及一个 authSubject 私有属性，它提供了存储当前用户认证状态的所有不同方式。这些 Observables 可用于对认证状态的实时更改做出反应。

3. 接下来，通过服务构造函数注入 Apollo 服务和 GraphQL 服务，如下所示：

```ts
constructor(
private apollo: Apollo,
private registerGQL: RegisterGQL,
private loginGQL: LoginGQL,
private getUserGQL: GetUserGQL) {}
```

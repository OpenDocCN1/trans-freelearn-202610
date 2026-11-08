
## 创建模块、服务和组件

你可以通过调用相关的 Angular CLI 命令在 Angular 中自动创建组件、服务和模块，该命令会生成创建基本组件、服务或模块所需的最基本代码。

让我们使用 Angular CLI 生成我们的模块、服务和组件。

为应用程序的每个功能创建一个模块。这是一条基本规则。例如，首页信息流可以包含在单独的模块中。将应用程序拆分为模块使你能够延迟加载组件并高效维护它。

### 创建模块

在我们的应用程序中，我们将使用以下模块：

- 一个 `users` 模块，包含登录、注册和个人资料的组件。
- 一个 `feed` 模块，包含使用 GraphQL 获取的帖子所需的组件。
- 一个 `shared` 模块可能包含与应用程序中其他模块共享的元素。
- 一个根模块，包含应用程序的全局服务和通用布局组件，例如身份验证服务以及页眉和页脚组件。

共享模块通过避免导入多个模块所需的通用模块、组件和其他构件来节省您的时间。

例如，通过从共享模块导入并导出 ReactiveFormsModule，我们可以通过简单地导入共享模块来避免在 users 和 feed 模块中分别导入它。

此外，我们将导入并重新导出 Angular Material 组件模块到共享模块中，以便在项目中的其他模块中使用。

让我们开始使用 Angular CLI 自动创建模块。打开一个新的终端窗口，浏览到包含 Angular 项目的文件夹，并运行以下命令：

```
ng generate module shared
ng generate module users --routing
ng generate module feed --routing
```

这些命令将在 src/app/ 文件夹下创建多个文件，每个文件都有一个以我们指定的模块命名的子目录。

例如，共享模块将创建在 src/app/shared/shared.module.ts 文件下。打开该文件并进行以下更改：

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ReactiveFormsModule } from '@angular/forms';

@NgModule({
    declarations: [],
    imports: [
        CommonModule,
        ReactiveFormsModule
    ],
    exports: [
        ReactiveFormsModule
    ]
})
export class SharedModule { }
```

我们只是将 ReactiveFormsModule 导入到 imports 数组中，然后通过 exports 数组重新导出它。这使得它可用于导入共享模块的所有模块，从而消除了单独导入的需求。

--routing 选项指示 CLI 为每个生成的模块创建路由模块，使我们能够为每个模块的组件实现路由。此外，它将使我们能够使用延迟加载，从而通过允许 Angular 仅在需要时加载模块而不是在启动时全部加载来提高应用程序的速度。

> **重要提示**
> 
> 此外，我们可以将 CommonModule 添加到 exports 数组中以从共享模块重新导出它，然后将其从导入共享模块的任何其他模块中移除。虽然你可能认为共享模块对于小型应用程序是不必要的，但随着应用程序变得更大，它将为你节省大量重复的导入行，因此遵循这一做法是明智的。

### 创建组件和服务

接下来，让我们开发模块的组件和服务，首先是可能属于根应用程序模块的 header、footer、page-not-found 和 search-dialog 组件：

```
ng generate component core/components/header --module=app
```

```
ng generate component core/components/footer --module=app
```

```
ng generate component core/components/page-not-found --module=app
```

```
ng generate component core/components/search-dialog --module=app
```

```
ng generate service core/services/auth/auth
```

> **重要提示**
> 
> 另外，我们可以创建一个包含这些组件的核心模块，并导入和重新导出诸如 BrowserModule、AppRoutingModule 和 BrowserAnimationsModule 之类的模块，同时将 app 模块留空。然而，我们选择使用 core/ 文件夹而不是单独的模块来聚合应用程序的主要功能并直接从应用程序模块导入它们。

接下来，我们使用以下命令生成 users 模块的组件：

```
ng generate component users/components/login --module=users
```

```
ng generate component users/components/signup --module=users
```

```
ng generate component users/components/profile --module=users
```

```
ng generate service users/services/profile/profile
```

接下来，我们将创建 feed 模块的组件：

```
ng generate component feed/components/posts --module=feed
```

该命令已自动将创建的组件添加到各自的模块中。

接下来，我们创建共享模块的组件和服务：

```
ng generate component shared/components/create-post --module=shared
```

```
ng generate component shared/components/post --module=shared
```

```
ng generate component shared/components/dialog --module=shared
ng generate service shared/services/post/post --module=shared
```

然后，在 src/app/feed/feed.module.ts 文件中，按如下方式导入共享模块：

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { FeedRoutingModule } from './feed-routing.module';
import { PostsComponent } from './components/posts/posts.component';
import { SharedModule } from '../shared/shared.module';

@NgModule({
  declarations: [PostsComponent],
  imports: [
    CommonModule,
    FeedRoutingModule,
    SharedModule
  ]
})
export class FeedModule { }
```

此外，你必须在 app 和 users 模块中导入共享模块，因为它将用于导入这些模块所需的组件（例如 Angular Material 组件）。

现在我们需要做的就是构建应用程序的模块、组件和服务的结构。一些其他构件也将在本书的后续章节中添加。在接下来的几章中，我们将开发每个组件和服务的底层功能。

在下一节中，我们将了解 Angular Router 以及如何使用它将应用程序用户界面的组件映射到特定的 URL 路由并提供应用内导航。

## 理解 Angular 路由

通常，你需要在 Web 应用程序中实现路由，因为你无法将应用程序的所有功能都放在一个页面上——这会造成糟糕的用户体验，甚至使某些任务对用户来说变得不可能，例如，在我们的例子中，通过其唯一 URL 访问个人资料的能力，或通过 **搜索引擎优化 (SEO)** 使个人资料可被搜索引擎找到。

作为一个一体化平台，Angular 具有内置的路由器模块，可简化具有众多视图和导航的单页应用的实现。此外，Angular CLI 在项目创建过程中可以通过自动配置应用程序中的路由模块来帮助你。

Angular Router 是一个静态路由库。与动态路由相反，路由是在应用程序初始化时指定的。你可以在包含应用程序所有路由的单个路由模块中实现路由，也可以为每个模块单独添加路由。

接下来，我们将探讨 Angular Router 的工作原理以及 Angular CLI 如何自动化我们在项目中配置路由的过程。

### Angular Router 的工作原理

而不是向服务器请求获取新页面，你可以通过显示或隐藏由组件控制的视图来管理用户在 Angular 应用程序中看到的内容。这是通过 **客户端路由** 而非服务器端路由实现的。

为了提供良好的用户体验，你的应用程序应根据业务领域为每个活动或相关活动组呈现不同的视图，而不是将所有功能都合并到单个视图中。此外，它应启用不同视图之间的导航——这可以通过使用 Angular Router 和浏览器 URL 实现。

Angular Router 是一个客户端 JavaScript 解决方案，它通过监控 URL 的变化并使用路由/组件映射数组将路径映射到视图来运行。

你在路由模块中指定路径如何映射到组件，当路径匹配时（即当用户导航到与路由条目关联的某个路径时），路由器会处理显示适当的组件。

路由器还允许你提供一个通用的通配符条目，当未识别到匹配的路径时将使用该条目。

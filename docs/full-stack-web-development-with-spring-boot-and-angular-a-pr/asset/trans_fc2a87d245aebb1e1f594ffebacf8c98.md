

```
declarations: [ListComponent, FormComponent],
imports: [RouterModule.forChild(routes)],
exports: [RouterModule]
})
export class AntiHeroRoutingModule {}
```

在前面的示例代码中，我们为表单和列表页面定义了路由。这意味着页面组件将在我们的应用程序中拥有直接路由，并且我们还将 ListComponent 定义为此模块的基础路由。

在成功创建页面组件并定义路由后，我们希望根模块（AppModule）拥有 AntiHeroModule 的路由。

为实现此目的，我们将在 app-routing.module.ts 中放置以下代码：

```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
const routes: Routes = [
  {
    path: "",
    redirectTo: "anti-heroes",
    pathMatch: "full",
  },
  {
    path: "anti-heroes",
    loadChildren: () =>
      import("./anti-hero/anti-hero.module").then((m) =>
        m.AntiHeroModule),
  }
];
@NgModule({
  imports: [RouterModule.forRoot(routes)],
})
export class AppRoutingModule { }
```

在前面的示例代码中，我们使用懒加载为 AntiHeroModule 创建了一个路由。一旦我们访问 {baseUrl}/anti-heroes URL，这将加载 AntiHeroModule 并重定向到基础路由，即 ListComponent。我们还使用了 RouterModule.forRoot() 来导入路由，因为这是根模块。

在成功为 AppModule 定义路由后，我们现在可以看到应用程序的当前结构：

![](img/7c3ddd2e5893c9b5a0220148ed886548_245_0.png)

图 10.6 – 创建 anti-hero 功能后的文件夹结构

现在我们已经在 Angular 中完成了功能模块，我们只有 pages 文件夹。我们将在开发应用程序时稍后添加其他块，例如组件和指令。下一步是创建共享模块。

## 共享模块下的块

现在，我们下一个目标是在共享模块下创建块。我们将共享模块定义为在整个应用程序中共享的组件、指令和管道，并且不得依赖于其他模块。为创建我们的共享模块，我们将执行 ng g m shared 命令。这将在新的 shared 文件夹中创建一个新的 shared 文件夹和模块文件。

现在在完成共享模块后，我们可以生成将被归类到此模块中的块。在我们的应用程序中，我们可以将 navbar 和 footer 作为共享组件包含在内，因为它们将在应用程序的每个部分中使用。

我们将执行 ng g c shared/layout/navbar 和 ng g c shared/layout/footer 命令来搭建 navbar 和 footer。我们可以看到 FooterComponent 和 NavbarComponent 已自动添加到 shared.module.ts 文件中，因为 Angular 检测到最接近组件的模块：

```
@NgModule({
  declarations: [
    NavbarComponent,
    FooterComponent
  ],
  imports: [CommonModule]
})
export class SharedModule { }
```

请记住在 NgModule 的 exports 元数据中添加 navbar 和 footer 组件，并且我们将在不同模块中导入共享模块：

```
@NgModule({
  declarations: [
    NavbarComponent,
    FooterComponent
  ],
  exports: [NavbarComponent, FooterComponent]
  imports: [CommonModule]
})
```

我们也可以通过执行 ng g c shared/directive/directive-name 和 ng g c shared/pipes/pipe-name 命令根据应用程序的需求在 shared 文件夹下添加共享指令和管道。在成功创建块后，我们将具有以下文件夹结构：

![](img/7c3ddd2e5893c9b5a0220148ed886548_246_0.png)

图 10.7 – 创建共享模块后的文件夹结构

我们还必须记住，共享模块不需要路由模块，因为它没有需要在应用程序中路由的组件。

## 核心模块下的块

我们需要创建的最后一个模块是核心模块。核心模块是我们共享在整个应用程序中的服务，并且它们只有一个实例。总是进入核心模块的一个服务是身份验证服务。

为创建我们的共享模块，我们将在完成核心模块后执行 ng g m core; 命令。我们将通过运行 ng g s core/services/authenticate 命令来搭建 authenticate 服务。

在成功创建 authenticate 服务后，我们将在 core.module.ts 文件下提供它以将服务包含在模块中。我们也可以根据应用程序的需求通过添加 models 文件夹在核心模块下添加共享模型。现在，我们具有以下文件夹结构：

![](img/7c3ddd2e5893c9b5a0220148ed886548_247_0.png)

图 10.8 – 创建共享模块后的文件夹结构

我们将在开发应用程序的过程中制作 authenticate 服务的内容，但现在我们已经创建了 Angular 应用程序的基础结构，我们将使用此结构来构建项目的其他部分。现在，我们将讨论如何在 Angular 中构建组件的结构。

## 构建组件

我们已经通过根据其使用和功能对模块进行分类来构建 Angular 应用程序，这将有利于代码重用和可维护性。然而，在特定模块下可能仍会创建大量组件，这将进一步提高应用程序的可维护性。在本节中，我们将讨论在组件级别构建 Angular 架构的另一种策略。

## 智能和哑或展示组件

构建 Angular 应用程序最常见且推荐的 **组件级架构** 是智能和哑组件架构。在上一节中，我们根据在应用程序中使用的方式将模块拆分为不同的类别。

此架构也提供了相同的概念。我们将组件分为两种类型——即 **智能组件** 和 **哑** 或 **展示组件**。

让我们讨论每种组件类型的特征。

### 智能组件

**智能组件** 也称为应用程序级组件或容器组件。这些组件的主要职责是与服务通信并从请求中消费数据。由于它们很智能，它们包含获取应用程序数据的所有依赖项和订阅。

智能组件可以被视为在我们的应用程序中具有直接路由的页面组件，并且它们是持有哑组件的父组件。让我们看一下创建智能组件的以下示例代码：

```
@Component({
  selector: 'app-home',
  template: `
    <h2>User List</h2>
    <div>
      <table class="table">
        <tbody>
          <tr (click)="selectUser(user)" *ngFor="let user of users">
            <td>{{user.firstName}}</td>
            <td>
              <span>{{user.lastName}}</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  `,
  styleUrls: ['./home.component.css']
}
```

```
})
export class HomeComponent implements OnInit {
  users: User[] = [];
  constructor(private userService: UserService) {
  }
  ngOnInit() {
    this.userService.getUsers()
      .subscribe(users => this.users = users);
  }
  selectUser(user: User) {
    // action
  }
}
```

在前面的示例代码中，我们创建了一个名为 HomeComponent 的组件，它将在表格中显示用户列表。我们还注入了 UserService 来从 HTTP 请求中获取用户。我们知道此组件将成功显示用户，但我们可以看出我们的模板很庞大，并且随着我们向此组件添加更多功能，它可能会变得过于复杂。

我们想要做的是省略仅用于展示目的的元素。在此场景中，我们希望从 HomeComponent 中移除表格，并且我们将具有以下代码：

```
@Component({
  selector: 'app-home',
  template: `
    <h2>User List</h2>
    <div>
      <!--we will place that dumb component here later-->
    </div>
  `,
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {
  users: User[] = [];
  constructor(private userService: UserService) {
  }
  ngOnInit() {
    this.userService.getUsers()
```


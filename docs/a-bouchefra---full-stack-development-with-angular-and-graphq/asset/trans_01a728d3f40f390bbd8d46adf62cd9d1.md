
5. 同样地，为组件的类添加以下用于订阅新点赞的方法：

```
subscribeToNewLikes() {
    const onPostLikedObs = this.onPostLiked
        .subscribe();
    onPostLikedObs.subscribe({
        next: (result) => {
            const like = result.data?.onPostLiked;
            if (this.authUser?.id !== like?.user.id) {
                this.notificationsCount++;
                this.changeDetectorRef.markForCheck();
            }
        }
    });
}
```

6. 按如下方式从组件的 ngOnInit() 方法调用之前的方法：

```
ngOnInit(): void {
    this.authService.authState
        .pipe(takeUntil(this.destroyNotifier$))
        .subscribe({
            next: (authState: AuthState) => {
                this.isLoggedIn = authState.isLoggedIn;
                this.authUser = authState.currentUser;
                this.changeDetectorRef.markForCheck();
                this.subscribeToNewComments();
                this.subscribeToNewLikes();
            }
        });
}
```

7. 最后，使用 Material 徽章在模板中显示通知计数。打开 src/app/core/components/header/header.component.html 文件并按如下方式更新 Notifications 按钮：

```
<button *ngIf="isLoggedIn" mat-icon-button matTooltip="Notifications">
  <mat-icon aria-hidden="false" aria-label="Notifications" [matBadge]="notificationsCount" matBadgeColor="warn">
    notifications
  </mat-icon>
</button>
```

这里，我们向 <mat-icon> 元素添加了 matBadge 属性并将其绑定到组件的 notificationsCount 属性。

就这样——现在你应该能够在应用程序中接收并显示新评论和点赞的通知了。查看提交 https://git.io/JDuoK。

你始终可以扩展此实现以显示用户收到的通知的更多信息。

## Summary

在本最后一章中，我们学习了如何为 GraphQL 订阅配置 Apollo Client，然后添加代码使用徽章在应用程序的页眉中显示通知。

在整本书中，我们使用 Angular 和 GraphQL 等前沿技术，使用 Lerna 开发了具有 monorepo 架构的全栈应用程序。我们在客户端和服务器上都使用 Apollo 来发送和响应 GraphQL 查询。

在设置开发环境并安装 Node.js 之后，我们使用 Express.js 和 Apollo Server 构建了一个支持 GraphQL 的服务器来实现后端。我们还在开始开发用于消费 API 并向用户展示用户界面以便与后端通信的前端应用程序之前，使用 Apollo Studio 与我们的 GraphQL API 通信。

之后，我们解释了如何安装 Express.js 并使用 TypeScript 和 GraphQL 对其进行配置。我们使用模拟在实现负责获取和添加数据的解析器之前提供了一个使用 Apollo Server 的工作 GraphQL 服务器。

然后，我们学习了如何使用 TypeORM 将 MySQL 数据库连接到我们的 Web 应用程序，并实现了用于与数据库通信的解析器。我们公开了一个工作中的 GraphQL API，其解析器从真实的 MySQL 数据库中查询和删除数据。

我们还利用 TypeORM 抽象数据库操作，使开发人员能够在不更改代码的情况下为其应用程序使用任何所需的数据库管理系统。我们学习了如何将 TypeORM 连接到 Apollo，以及如何生成和填充数据库表。

接下来，我们使用 Apollo Server 向我们的 GraphQL API 添加了身份验证和图像上传，并学习了使用 Node.js、Express 和 Apollo Server 添加身份验证以及处理图像上传所需的概念。

我们还向服务器应用程序添加了带身份验证的实时支持，这使我们能够在数据可用时立即将最新数据从服务器通信到客户端。为此，我们利用 Apollo Server 的 GraphQL 订阅，在用户帖子上的新评论和点赞被添加的瞬间将其从服务器推送到客户端。

在实现后端之后，我们使用 Angular CLI 创建了一个新的 Angular 项目，并回顾了许多基本的 Angular 概念，例如模块、组件和服务。我们还学习了如何使用 Angular CLI 生成构成应用程序结构的工件。

然后，我们在项目中使用 Angular Material 提供 Material 设计组件，以创建视觉上吸引人的应用程序 UI。

最后，我们使用 Apollo Client 将使用 Angular 构建的前端与后端集成，Apollo Client 设计用于向服务器发送 GraphQL 查询和变更以获取和写入数据。

在开发和测试应用程序并添加任何你想要的额外功能之后，你可以构建生产包并使用你喜欢的托管服务托管它们。请参阅 `https://angular.io/guide/deployment` 官方文档以获取有关此过程的更多信息。

感谢你抽出时间阅读本书，希望你喜欢它。

## Index

## Symbols

<base href> 标签 143

## A

AfterViewInit 接口
  参考链接 345
Amazon Relational Database Service
  (Amazon RDS) 9, 10
Angular
  关于 132
  图像上传 244, 245
  用于构建前端 10
Angular applications
  调试 152-154
Angular binding syntax
  参考链接 137
Angular command-line interface
  (Angular CLI)
  关于 128, 220
  命令 129
  安装 129
Angular forms 132

Angular Material
  关于 132
  添加 154
  组件导入 161, 162
Angular project
  初始化 130, 131
Angular Router
  关于 142
  工作方式 142, 143
Angular routing 142
Angular testing
  参考链接 220
Angular Universal 132
Apollo
  关于 9
  图像上传 244, 245
  与 TypeORM 集成 68-85
  用于集成后端 9
  用于集成前端 9
Apollo Client
  关于 9, 159
  安装 159, 160
  用于本地状态
    管理 209-218

## C

## B

Apollo Client for subscriptions
    设置 348, 349
Apollo Link
    接口 160
    参考链接 160, 294
Apollo services
    定义 168-170
Apollo Studio
    用于向服务器发送查询
      32, 33
Apollo, testing in Angular
    参考链接 220
application architecture 132
authentication
    关于 89
    实现 89-96
    与授权的区别 89
authentication over WebSockets
    参考链接 120
authentication system
    改进 293
auth guard
    测试 230-233
authorization 89
auth service
    测试 220-229
backend
    与 Apollo 集成 9
base component
    实现 256-265

cache
    删除评论后更新 344
cache configuration
    参考链接 301
cached field
    参考链接 344
cached fields behavior, customizing
    参考链接 210
client-side apps 5
client-side routing 142
code
    重新编译 26-29
    查看 26-29
commands, Angular CLI
    ng build 129
    ng generate 129
    ng new 129
    ng serve 129
comment box
    练习 347
    显示 345, 346
comments
    显示 315-325, 336-343
CommentsService
    实现 302-304
CommonModule 133
component
    关于 134
    创建 140, 141
    测试 220-241
Component decorator
    selector 136
    styleUrls 136
    templateUrl 136

component state
    使用响应式
      变量持久化 330-334
container components 282
create post component
    实现 282-289
    测试 289, 290
Cross-origin resource sharing (CORS)
    关于 44
    配置 44
custom events
    comment 305
    like 305
    listComments 305
    listLikes 305
    moreComments 305
    remove 305
D
data
    使用 MySQL 数据库保存 9, 10
    使用 TypeORM 保存 9, 10
database
    测试数据填充 62-67
dates
    使用 field policies 重写 334-336
dependency injection 136
development dependencies
    安装 22-24
development technologies 6
Document Object Model (DOM) 134
Don't Repeat Yourself (DRY) 256
dumb 282

E
Express.js
    关于 7
    安装 22-24
    用于运行 Web 服务器 7
Express server
    创建 25, 26
F
feed module
    关于 138
    组件生成 140
fetchMore() method
    参考链接 321
field policies
    关于 209, 210
    定义 273, 274
    参考链接 210, 212
    用于重写日期 334, 335
footer component
    实现 196
FormsModule 133
fragments
    参考链接 248
frontend
    使用 Angular 构建 10
    使用 TypeScript 构建 10
    与 Apollo 集成 9
full-stack application
    关于 4, 5
    后端 5
    前端 5
full-stack architecture 4


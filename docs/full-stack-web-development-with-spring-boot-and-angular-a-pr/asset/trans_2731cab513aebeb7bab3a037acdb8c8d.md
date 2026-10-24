

在已实现的的代码中，我们将响应式表单与电子邮件和密码输入绑定。我们还创建了一个条件，使按钮在页面当前处于登录或注册时发生变化。

我们已成功创建了我们的身份验证表单；现在，我们将创建我们的登录和注册页面。

## 登录和注册页面

登录和注册页面被视为我们应用程序的智能组件，因为这些是会分派调用身份验证 API 的 action 的组件。

要创建登录和注册页面，我们将执行以下命令：

```
ng g c auth/page/login auth/page/register
```

成功创建两个页面后，我们将运行登录和注册组件的代码：

### 登录页面

```
```typescript
//TS File
export class LoginComponent{
  constructor(private authService: AuthenticateService,
      private router: Router) {
  }
  submit(data:{email:string, password:string}) {
    this.authService.login(data).subscribe((data) => {
      this.router.navigate(['/anti-heroes']);
      localStorage.setItem('token', data.token);
    });
  }
}
// HTML File
<app-auth-form (submitEmitter)="submit($event)"></app-auth-form>
```
```

### 注册页面

```
```typescript
// TS File
export class RegisterComponent {
  error: string = "";
```
```

```
constructor(private authService: AuthenticateService) {
}
submit(data: User) {
    this.authService.register(data).subscribe((data) => {
        this.router.navigate([' /']);
    });
}
// HTML File
<app-auth-form title="Register"
(submitEmitter)="submit($event)"></app-auth-form>
```

在前面的代码示例中，我们可以看到登录页面和注册页面正在使用相同的身份验证表单组件。一旦表单提交，它将把表单值传递到 login() 或 register() 函数中，分别用于验证或创建用户。如果登录成功，我们将把用户重定向到反英雄列表页面，并将 API 生成的 token 放入本地存储中。

## 路由模块

下一步是创建 auth-routing 模块，该模块将定义身份验证模块的路由。要创建该模块，让我们执行以下命令：

```
ng g m auth/auth-routing --flat
```

创建路由模块后，我们将运行以下代码：

```
const routes: Routes = [
  {
    path: "",
    component: LoginComponent
  },
  {
    path: "register",
    component: RegisterComponent
  }
];
@NgModule({
  declarations: [],
```

```
imports: [RouterModule.forChild(routes)],
exports: [RouterModule],
})
export class AuthRoutingModule {}
```

我们还需要修改我们的 app-routing 模块，因为我们需要我们的基本路径重定向到登录页面；让我们实现以下修改：

```
const routes: Routes = [
  {
    path: "",
    redirectTo: "login",
    pathMatch: "full",
  },
  {
    path: "login",
    loadChildren: () =>
      import("./auth/auth.module").then((m) => m.AuthModule),
  },
  {
    path: "anti-heroes",
    loadChildren: () =>
      import("./anti-hero/anti-hero.module").then((m) =>
        m.AntiHeroModule),
  }
];
```

在前面已实现的代码中，我们可以看到一旦我们访问基本路径，这将现在加载 AuthModule 并将我们重定向到登录页面，如图 14.1 所示。

## Angular CRUD

![](img/7c3ddd2e5893c9b5a0220148ed886548_347_0.png)

图 14.1 – 登录页面

我们现在应该能够使用数据库中的用户登录。如果没有创建用户，我们可以使用注册页面创建一个新的用户，并且一旦登录成功，我们将被重定向到反英雄列表页面，如 *图 14.2* 所示。

## Angular CRUD

![](img/7c3ddd2e5893c9b5a0220148ed886548_347_1.png)

图 14.2 – 反英雄列表页面

我们还可以观察到我们的有效 JWT 已经放置在我们的本地存储中，因为 HTTP 拦截器正在使用 JWT。当我们打开由我们的应用程序发出的请求时，我们可以看到标头中具有生成的 JWT：

```
Accept:
application/json, text/plain, */*
Accept-Encoding:
gzip, deflate, br
Accept-Language:
en-AU,en-US;q=0.9,en;q=0.8,bs;q=0.7,fr-CA;q=0.6,fr;q=0.5,tl;q=0.4
Authorization:
Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0ZXN0OQGdtYWlsLmNvbSIIm1hdCI6MTY1OTY5ODM2NSwiZXhwIjoxNjU5NzM0MzY1fQ.2SDLmvQcME5Be9Xj-zTeRlc6kGfQVNCMIWUBOBS5afg
```

在前面的示例标头中，我们可以看到 Authorization 标头包含应用程序调用的每个 API 请求的有效 JWT。JWT 在标头中的放置是在我们的登录成功并且我们被重定向到 AntiHeroModule 时完成的。

## Token 验证

我们需要做的下一步是添加 token 验证以检查我们的 token 是否已过期。要实现此功能，我们通过执行以下命令添加 @auth0/angular-jwt 库：

```
npm install @auth0/angular-jwt --save
```

@auth0/angular-jwt 库提供了有用的函数，例如 isTokenExpired()，用于检查 JWT 是否已过期，以及 decodeToken()，用于从 JWT 中检索信息。

成功安装库后，我们将向我们的 authenticate 服务添加以下代码：

```
isAuthenticated(): boolean {
  const token = localStorage.getItem('token') ?? '';
  // Check whether the token is expired and return
  // true or false
  return !this.jwtHelper.isTokenExpired(token);
}
```

我们还需要将 JWT 模块导入到我们的 `app.module.ts` 文件中：

```
```typescript
imports: [
    JwtModule.forRoot({})
],
```
```

我们将在我们的登录和注册页面上使用 `isAuthenticated()` 函数来检查本地存储中是否存在 JWT。如果存在有效的 JWT，我们将把应用程序重定向到反英雄列表页面。

让我们看看以下实现：

```
```typescript
//login page (TS File)
constructor(private authService: AuthenticateService, private
router: Router,) {
    this.checkJWT();
}
checkJWT() {
    if(this.authService.isAuthenticated()) {
        this.router.navigate(['/anti-heroes'])
    }
}
```
```

## 注销实现

我们需要实现的最后一个功能是注销功能。要添加此功能，我们唯一需要添加的就是一个将 token 从存储中移除的函数。让我们按如下方式进行代码实现：

### authenticate.service.ts

```
```typescript
export class AuthenticateService {
... other functions
doLogout() {
    let removeToken = localStorage.removeItem('token');
    if (removeToken == null) {
        this.router.navigate(['login']);
    }
}
}
```
```

在前面的代码示例中，我们添加了一个 doLogout() 函数，该函数移除存储中的 token 并将应用程序重定向到登录页面。现在，让我们编辑我们的 navbar 组件以具有注销按钮：

### navbar.component.html

```
<p>
    <mat-toolbar color="primary">
        <span>Angular CRUD</span>
        <span class="example-spacer"></span>
        <button *ngIf="loggedIn" (click)="submit('logout')" mat-icon-button>
            <mat-icon>logout</mat-icon>
        </button>
    </mat-toolbar>
</p>
```

### navbar.component.css

```
.example-spacer {
    flex: 1 1 auto;
}
```

### navbar.component.ts

```
export class NavbarComponent implements OnInit{
    @Output() actionEmitter = new EventEmitter();
    @Input() loggedIn = false;
    submit(action: string) {
        this.actionEmitter.emit(action);
    }
}
```

在前面的代码实现中，我们为我们的 navbar 组件创建了一个 emitter。这将发出我们在 navbar 中触发的 action，并将其传递到我们的 app 组件中。

最后一步是在注销按钮被点击时在我们的 app 组件中调用 doLogout() 函数。让我们看看如下的代码实现：

### app.component.html

```
```html
<app-navbar [loggedIn]="url != '/' && !url.includes('login')"
(actionEmitter)="submit($event)"></app-navbar>
<div class="container">
    <router-outlet></router-outlet>
</div>
```
```

### app.component.ts

```
```typescript
export class AppComponent {
  title = 'superheroes';
  url: string = "";
  constructor(private authService: AuthenticateService,
              private router: Router){
    this.getRoute();
  }
  submit(action: string) {
    switch (action) {
      case 'logout':
        this.authService.doLogout();
        break;
      default:
        break;
    }
  }
  getRoute() {
    this.router.events.subscribe(data => {
      if(data instanceof NavigationEnd) {
        this.url = data.url;
      }
    });
  }
}
```


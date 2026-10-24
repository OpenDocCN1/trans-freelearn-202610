

在前面的代码实现中，我们将 authenticate 服务注入到我们的应用组件中并调用了 doLogout() 函数。如果操作是注销，我们还添加了一个路由变化的监听器来检查我们当前的路由是否在登录或注册页面，如果是，我们将移除导航栏组件上的注销按钮。

我们已经成功为我们的应用实现了用户认证，但在本章的后续内容中我们仍会继续改进此实现。在下一节中，我们将讨论如何在 Angular 应用中保护路由。

## 保护路由

Angular 的核心特性之一是路由守卫。守卫在我们想要防止未认证直接访问路由或防止用户在意外导航时丢失更改时非常有帮助。

守卫是 Angular 提供的接口，允许我们通过提供的条件来控制路由的可访问性。这些守卫直接应用于我们想要保护的路由。

让我们来看一下 Angular 提供的一些守卫：

- CanActivate：这是在我们想要阻止访问的路由上实现的。
- 方法签名：

```
canActivate(route:ActivatedRouteSnapshot,
state:RouterStateSnapshot):Observable<boolean | UrlTree>
| Promise<boolean | UrlTree> | boolean | UrlTree
```

前面的代码定义了 CanActivate 守卫的签名。该函数接受 ActivatedRouteSnapshot 和 RouterStateSnapshot 参数，并返回一个 Observable 或 Promise，其类型可以是 Boolean 或 UrlTree。

- 创建守卫：

```
export class AuthGuard implements CanActivate {
constructor(private auth: AuthService, private router: Router) {}
canActivate(route: ActivatedRouteSnapshot,
state:RouterStateSnapshot): Observable<boolean> |
Promise<boolean> | boolean {
// return true permitted in the route, else return
// false
}
}
```

在前面的代码示例中，我们创建了一个名为 AuthGuard 的新类。我们还使用 CanActivateChild 守卫实现了它，并为所需逻辑添加了 canActivateChild() 函数。

- 使用守卫：

```
{
  path: 'user',

  canActivateChild: [AuthGuard],
  component: UserComponent,
  children: [
    { path: ':id', component: ProfileComponent },
    { path: ':id/edit', component: SettingsComponent }
  ]
}
```

在前面的代码示例中，我们在 user 路径中使用了新创建的 AuthGuard 类，以保护其子路由免受没有有效 JWT 的用户访问，子路由导航到 ProfileComponent 和 SettingsComponent 组件。

- CanLoad：此守卫用于懒加载模块。CanActivate 守卫只能阻止用户通过路由导航；CanLoad 守卫可同时阻止导航到懒加载模块以及下载该模块。

- 方法签名：

```
canLoad(route:Route,segments:UrlSegment[]):
Observable<boolean>|Promise<boolean>|boolean
```

前面的代码示例定义了 CanLoad 守卫的签名。该函数接受 Route 和 UrlSegment[] 参数，并返回一个 Observable 或 Promise，其类型可以是 Boolean。

- 创建守卫：

```
import { CanLoad, Route, Router } from '@angular/router';
export class AuthGuard implements CanLoad {
  constructor(private router: Router) {}
  canLoad(route:Route,segments:UrlSegment[]):Observable<boolean>|Promise<boolean>|boolean {
    // return true or false based on a condition to load
    // a module or not
  }
}
```

在前面的代码示例中，我们创建了一个名为 AuthGuard 的新类。我们还使用 CanLoad 守卫实现了它，并为所需逻辑添加了 canLoad() 函数。

使用守卫：

```
{
  path: "hero",
  loadChildren: () =>
    import("./hero/hero.module").then((m) =>
      m.AntiHeroModule),
  canLoad: [AuthGuard]
}
```

在前面的代码示例中，我们在 hero 路由中使用了新创建的 AuthGuard 类，以防止没有有效 JWT 的用户访问和下载资源。

CanDeactivate：这是一个用于防止用户离开当前路由的守卫。这在填写应用中的表单等场景下很有用，以避免在意外导航时丢失更改。

方法签名：

```
canDeactivate(component: T, currentRoute: ActivatedRouteSnapshot, currentState: RouterStateSnapshot, nextState?: RouterStateSnapshot): Observable<boolean|UrlTree>|Promise<boolean|UrlTree>|boolean |UrlTree;
```

前面的代码示例定义了 CanDeactivate 守卫的签名。该函数接受一个泛型组件、ActivatedRouteSnapshot 和 RouterStateSnapshot 参数，并返回一个 Observable 或 Promise，其类型可以是 Boolean 或 UrlTree。

创建守卫：

```
// CanDeactivateGuard service
import { Observable } from 'rxjs/Observable';
import { CanDeactivate, ActivatedRouteSnapshot,
RouterStateSnapshot } from '@angular/router';
export interface CanComponentDeactivate {
canDeactivate: () => Observable<boolean> |
Promise<boolean> | boolean;
}
```

```
export class CanDeactivateGuard implements CanDeactivate<CanComponentDeactivate> {
    canDeactivate(component:CanComponentDeactivate,currentRoute:ActivatedRouteSnapshot, currentState:RouterStateSnapshot, nextState?: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
        return component.canDeactivate();
    }
}
```

在前面的实现中，我们创建了一个接口，该接口将在 CanDeactivateGuard 服务的组件中使用：

```
export class FormComponent implements OnInit, CanComponentDeactivate {

canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
/* return true or false depends on a specific condition if you want to navigate away from this route or not.*/
}
}
```

在前面的代码示例中，我们为组件实现了我们创建的接口。

- 使用守卫：

```
{ path: ':id/edit', component: FormComponent, canDeactivate: [CanDeactivateGuard] }
```

在前面的代码示例中，我们使用了新创建的 CanDeactivateGuard 来防止用户根据 canDeactivate() 函数上的应用条件离开 FormComponent。

我们已经了解了可以在应用中使用的不同守卫。现在，让我们在 Angular 项目中实现它。

## 项目实现

我们需要在应用中应用的第一个守卫是 CanLoad 守卫。这是因为我们希望在没有有效 JWT 时保护 anti-heroes 路由不被访问。要创建 CanLoad 守卫，执行以下命令：

```
ng g g core/guards/auth
```

执行命令后，选择 CanLoad 选项以生成新的 AuthGuard 类。这里唯一需要更改的是 canLoad() 函数的实现。我们想要应用的条件是：如果 JWT 有效，则允许加载路由和模块。

让我们来看一下以下实现：

```
@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanLoad {
  constructor(private router: Router, private auth: AuthenticateService) {}
  canLoad(route: Route, segments:UrlSegment[]):
    Observable<boolean | UrlTree> | Promise<boolean | UrlTree> | boolean | UrlTree {
    if (!this.auth.isAuthenticated()) {
      this.router.navigate(['login']);
      return false;
    }
    return true;
  }
}
```

在前面的代码示例中，我们使用了 isAuthenticated() 函数来检查 JWT 是否有效且未过期。如果有效，它将返回 true 并允许我们导航路由。否则，它将重定向我们到登录页面。

最后一步是在 anti-heroes 路由中应用 AuthGuard 类；在 app-routing.module.ts 文件中，我们将使用以下内容：

```
const routes: Routes = [
  ... other routes here
  {
    path: "anti-heroes",
    loadChildren: () =>
      import("./anti-hero/anti-hero.module").then(m => m.AntiHeroModule),
    canLoad: [AuthGuard]
  }
];
```


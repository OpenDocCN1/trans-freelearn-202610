
## 测试认证守卫

让我们编写 auth guard 测试。我们将编写一些测试来断言守卫会根据用户的认证状态适当地重定向用户。如果用户已登录，我们的守卫会使用 auth service 来确定他们是否已登录；否则，它会使用 router 的 parseUrl() 方法将他们重定向到登录路径。

为了确保我们的实现按预期工作，我们需要添加一个隔离的单元测试套件。因此，我们必须定义 auth service 的一个存根，供 auth guard 使用以确定用户是否已通过身份验证。我们还将为 parseUrl() 方法定义一个间谍对象。按以下步骤操作：

- 1. 打开 core/guards/auth/auth.guard.spec.ts 文件并开始通过包含以下导入来配置测试：

```
import { TestBed }
  from '@angular/core/testing';
import {
  ActivatedRouteSnapshot,
  Router,
  RouterStateSnapshot,
  UrlTree } from '@angular/router';
import { Observable, of } from 'rxjs';
import { AuthService }
  from '../../services/auth/auth.service';
```

然后，添加以下用于模拟 auth service 的类：

```
class AuthServiceMock {
  get isLoggedIn(): Observable<boolean> {
    return of(true);
  }
}
```

我们使用 of() 函数返回一个同步的 Observable，该 Observable 为身份验证状态发出 true 值。

- 2. 接下来，我们需要通过定义这些变量来扩展我们的 describe() 块：

```
let router: Partial<Router>;
let authService: Partial<AuthService>;
let routerStateSnapshot: RouterStateSnapshot;
let activatedRouteSnapshot: ActivatedRouteSnapshot;
```

- 3. 在 beforeEach() 函数内部，我们必须通过向 TestBed. configureTestingModule({}) 调用添加以下 providers 来配置测试模块：

```
providers: [
  {
    provide: Router,
    useValue: {
      parseUrl: jasmine.createSpy('navigate')
    }
  },
  {
    provide: AuthService,
    useClass: AuthServiceMock
  },
]
```

- 4. 接下来，按如下方式注入 router 和 auth 服务：

```
router = TestBed.inject(Router);
authService = TestBed.inject(AuthService);
```

然后，按如下方式创建虚拟对象：

```
routerStateSnapshot = { url: '/feed' } as RouterStateSnapshot;
activatedRouteSnapshot = {} as ActivatedRouteSnapshot;
```

而不是使用完整对象，我们只设置测试所需的最小属性。CanActivate 接受 RouterStateSnapshot 和 ActivatedRouteSnapshot 对象作为参数，但 auth guard 实际上并不使用它们来允许或拒绝访问。因此，我们可以使用这些空对象作为参数。

- 5. 添加第二个测试用例，如下所示：

```
it('should allow access when user is logged in', () =>
  {});
```

在 it() 块内部，按如下方式调用守卫的 canActivate() 方法：

```
const canActivateObs: Observable<boolean | UrlTree> =
  guard.canActivate(
    activatedRouteSnapshot,
    routerStateSnapshot
) as Observable<boolean | UrlTree>;
```

然后，订阅返回的 Observable 并断言 isLoggedIn 变量为 true，如下所示：

```
canActivateObs.subscribe((isLoggedIn) => {
    expect(isLoggedIn).toBe(true);
});
```

- 6. 添加第三个测试用例，如下所示：

```
it('should redirect to login page when user is logged out', () => {});
```

在 it() 函数内部，通过执行以下代码来监视 auth service 的 isLoggedIn 属性：

```
spyOnProperty(authService,
    'isLoggedIn').and.returnValue(of(false));
```

然后，按如下方式调用守卫的 canActivate() 方法：

```
const canActivateObs: Observable<boolean | UrlTree> =
    guard.canActivate(
        activatedRouteSnapshot,
        routerStateSnapshot
    ) as Observable<boolean | UrlTree>;
```

最后，订阅返回的 Observable 并断言 router 的 parseUrl() 方法已被调用并传入 /users/login 路径，如下所示：

```
canActivateObs.subscribe(() => {
    expect(router.parseUrl)
        .toHaveBeenCalledOnceWith('/users/login');
});
```

我们的测试应该通过，如以下屏幕截图所示：

![](img/c798729e4c42765fcb1c98950164d0ba_250_0.png)

图 8.4 – AuthGuard 测试

你可以在此提交获取这些测试用例的代码：https://git.io/J6xBe。

## 测试 header 组件

让我们在编写测试之前先配置 header 组件的测试。
按以下步骤操作：

- 1. 打开 core/components/header/header.component.spec.ts 文件并包含以下导入：

```
import {
    ChangeDetectionStrategy,
    NO_ERRORS_SCHEMA
} from '@angular/core';
import { fakeAsync, tick } from
    '@angular/core/testing';
import { MatDialog } from '@angular/material/dialog';
import { MatSnackBar } from '@angular/material/
    snack-bar';
import { By } from '@angular/platform-browser';
import { Router } from '@angular/router';
import { defer, of, Subject } from 'rxjs';
import {
    AuthState,
    SearchUsersResponse,
    UsersResponse } from 'src/app/shared';
import { AuthService } from 'src/app/core';
```

- 2. 在 describe() 函数的回调中定义以下变量：

```
let authService: AuthService;
let authServiceStub: Partial<AuthService>;
let router: Router;
let routerStub: Partial<Router>;
let matSnackBar: MatSnackBar;
let matDialog: MatDialog;
```

- 3. 你必须在 beforeEach() 函数的回调中创建 auth service 的存根，如下所示：

```
authServiceStub = {
  authState: of({
    isLoggedIn: false,
    currentUser: null
  } as AuthState),
  logOut: jasmine.createSpy('logOut'),
  searchUsers: (searchQuery: string, offset: number,
    limit: number): SearchUsersResponse => {
    return {
      data: defer(() => Promise.resolve({
        searchUsers: []
      } as UsersResponse)),
      fetchMore: jasmine.createSpy('fetchMore')
    };
  }
};
```

然后，按如下方式创建 router 服务的存根：

```
routerStub = {
  navigateByUrl: jasmine.createSpy('navigateByUrl')
};
```

- 4. 然后必须通过向传递给 TestBed.configureTestingModule() 方法的对象添加以下 providers 来配置测试模块：

```
providers: [
  { provide: MatDialog, useValue: {} },
  {
    provide: Router, useValue: routerStub
  },
  {
    provide: MatSnackBar, useValue: {
      open: jasmine.createSpy('open')
    }
  },
{
    provide: AuthService, useValue: authServiceStub
}
]
```

- 5. 之后，你必须通过执行以下代码来配置测试模块以忽略未知指令：

```
await TestBed.configureTestingModule({
    // [...] 
    schemas: [NO_ERRORS_SCHEMA]
})
```

- 6. 覆盖 header 组件并将 changeDetection 设置为默认策略而不是与测试不兼容的 OnPush 策略。操作方法如下：

```
await TestBed.configureTestingModule({
    // [...] 
}).overrideComponent(HeaderComponent, {
    set: {
        changeDetection: ChangeDetectionStrategy.Default
    }
})
.compileComponents();
```

- 7. 在 compileComponents() 方法正下方按如下方式注入 header 组件所需的服务：

```
authService = TestBed.inject(AuthService);
router = TestBed.inject(Router);
matSnackBar = TestBed.inject(MatSnackBar);
matDialog = TestBed.inject(MatDialog);
```

在配置完测试后，让我们按如下方式为 header 组件编写测试用例：

- 8. 编写第一个测试用例，如下所示：

```
it('should display the app title', () => {
    const h1Element: HTMLElement = fixture.nativeElement
        .querySelector('h1');
    expect(h1Element).toBeDefined();
    expect(h1Element.textContent).toContain('ngSocial');
});
```

这断言正确标题已渲染。

- 9. 编写第二个测试用例，断言 isLoggedIn 属性初始为 false，如下所示：

```
it('should have #isLoggedIn set to false after construction', () => {
  expect(component.isLoggedIn).toEqual(false);
});
```

- 10. 编写第三个测试用例，断言 authUser 属性初始为 null，如下所示：

```
it('should have #authUser set to null after construction', () => {
  expect(component.authUser).toEqual(null);
});
```

- 11. 编写第四个测试用例以断言如果用户已登录则搜索栏出现，如下所示：

```
it('should display the search bar if user is logged in', () => {
  component.isLoggedIn = true;
  fixture.detectChanges();
  const searchbar = fixture.debugElement
    .query(By.css('.searchbar'));
  expect(searchbar).toBeTruthy();
});
```

- 12. 实现以下测试用例以断言组件将注销委托给 auth service：

```
it('should logout by delegation to AuthService', () => {
  component.logOut();
  expect(authService.logOut)
    .toHaveBeenCalledTimes(1);
});
```

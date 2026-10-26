

```ts
declarations: [
  vPlanetApp,
  AboutPage,
  AccountPage,
  LoginPage,
  PopoverPage,
  SignupPage,
  WalkThroughPage,
  HomePage,
  CategoriesPage,
  ProductsPage,
  ProductsFilterPage,
  ProductDetailPage,
  SearchPage,
  WishlistPage,
  ShowcartPage,
  CheckoutPage,
  SettingsPage,
  SupportPage,
  OrdinalPipe,
],
imports: [
  BrowserModule,
  HttpModule,
  IonicModule.forRoot(vPlanetApp, {locationStrategy: 'hash'}, deepLinkConfig),
  Ionic2RatingModule,
  TranslateModule.forRoot({
    provide: TranslateLoader,
    useFactory: createTranslateLoader,
    deps: [Http]
  }),
  IonicStorageModule.forRoot({
    useFactory: provideStorage
  }),
  CloudModule.forRoot(cloudSettings)
],
bootstrap: [IonicApp],
entryComponents: [
  vPlanetApp,
  AboutPage,
  AccountPage,
  LoginPage,
  PopoverPage,
  SignupPage,
  WalkThroughPage,
  HomePage,
  CategoriesPage,
  ProductsPage,
  ProductsFilterPage,
  ProductDetailPage,
  SearchPage,
  WishlistPage,
  ShowcartPage,
  CheckoutPage,
  SettingsPage,
  SupportPage
],
providers: [
  SplashScreen,
  Deeplinks,
  TextToSpeech,
  AppRate,
  GoogleAnalytics,
  Camera,
  CallNumber,
  SocialSharing,
  Push,
  {provide: ErrorHandler, useClass: IonicErrorHandler},
  { provide: Storage, useFactory: provideStorage },
  UserService,
  DataService
]
})
export class AppModule {}
```

此外，我建议你查看本章中 vPlanet Commerce 应用的代码，这将有助于你很好地了解我们在应用程序中将使用哪些页面和组件。由于本章主要针对各种 Ionic 组件，我们将在应用程序中使用大量这些组件。

## Ionic 组件

在构建应用程序时，我们可以有效地使用许多 Ionic JavaScript 组件。最好的做法是寻找应用程序中需要的功能。让我们从我们的电子商务应用程序的首页开始，首页上将有一个带有横幅的图片滑块。

### Slides

Slides 组件是一个多节容器，可用于多种场景，例如教程视图或横幅滑块。`<ion-slides>` 组件包含多个 `<ion-slide>` 元素，可以左右拖动或滑动。Slides 提供了多种配置选项，可传递给 ion-slides，例如 autoplay、pager、direction: vertical/horizontal、initial slide 和 speed。

使用 slides 非常简单，我们只需将它们包含在我们的 home.html 中；home.ts 文件中不需要任何依赖即可包含它：

```html
// Banner Slider in src/pages/home/home.html

<ion-slides pager #adSlider (ionSlideDidChange)="logLengthTrackEvent()" style="height: 250px">
  <ion-slide *ngFor="let banner of banners">
    <img [src]="banner">
  </ion-slide>
</ion-slides>

// Defining banners image path

export class HomePage {

  products: any;
  banners: String[];

  constructor() {
    this.banners = [
      'assets/img/banner-1.webp',
      'assets/img/banner-2.webp',
      'assets/img/banner-3.webp'
    ]
  }
}
```

正如前面的代码所示，ion-slides 可用的属性有很多，例如 pager、autoplay、direction 等：

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_104_0.png)

如果查看前面的截图，你可以看到我们使用了白色背景的横幅。实际上，作为一个建议，尝试在应用程序内部使用与横幅相同的背景颜色。这样横幅在低分辨率或高分辨率的手机和平板上都会显示得很好。

### Lists

列表是许多应用程序中最常用的组件之一。在列表中我们可以显示信息行。我们将在应用程序内部多次使用列表，例如在 `categories` 页面上，我们显示多个子类别：

```html
// src/pages/categories/categories.html
<ion-content class="categories">
  <ion-list-header *ngIf="!categoryList">Fetching Categories ....</ion-list-header>
  <ion-list *ngFor="let cat of categoryList">
    <ion-list-header>{{cat.name}}</ion-list-header>
    <ion-item *ngFor="let subCat of cat.child">
      <ion-avatar item-left>
        <img [src]="subCat.image">
      </ion-avatar>
      <h2>{{subCat.name}}</h2>
      <p>{{subCat.description}}</p>
      <button ion-button clear item-right (click)="goToProducts(subCat.id)">View</button>
    </ion-item>
  </ion-list>
</ion-content>
```

我们在这里使用了一个带有所属类别顶部标题的头像列表。可用的列表类型有很多，例如多行、缩略图和滑动列表。我们将在本章后面在 showcart 页面上集成滑动列表：

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_107_0.png)

正如你所见，前面的截图显示了类别页面上头像列表的外观。我们应该确保在计划在应用程序中使用列表之前查看文档，因为这有助于我们可视化哪种类型的列表可以在各种不同的应用程序中得到最佳利用。

### Loading and Toast

Loading 组件可用于指示某些活动，同时阻止任何用户交互。使用 Loading 组件最常见的情况之一是向服务器的 HTTP 调用，因为我们知道从服务器获取数据需要时间，在此期间为了良好的用户体验我们可以显示一些内容显示 Loading ...，或 Login wait ...，用于登录页面。

Toast 是一个提供反馈的小弹出窗口，通常在用户执行某些操作时使用。Ionic 3 现在将其 toast 组件作为其库的一部分提供；之前我们必须使用原生的 Cordova 插件来实现 toasts，现在也可以使用。

Loading 和 toast 组件都具有 `create` 方法。我们在创建这些组件时必须提供选项：

```ts
// src/pages/login/login.ts
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';
import { NavController, LoadingController, ToastController, Events } from 'ionic-angular';
import { SignupPage } from '../signup/signup';
import { HomePage } from '../home/home';

import { Auth, IDetailedError } from '@ionic/cloud-angular';

import { UserService } from '../../providers/user-service';

@Component({
  selector: 'page-user',
  templateUrl: 'login.html'
})
export class LoginPage {
  login: { email?: string, password?: string } = {};

  submitted = false;

  constructor(public navCtrl: NavController,
    public loadingCtrl: LoadingController,
    public auth: Auth,
    public userService: UserService,
    public toastCtrl: ToastController,
    public events: Events) { }

  onLogin(form: NgForm) {
    this.submitted = true;

    if (form.valid) {
      // start Loader
      let loading = this.loadingCtrl.create({
        content: "Login wait...",
        duration: 20
      });
      loading.present();

      this.auth.login('basic', this.login).then((result) => {
          // user is now registered
          this.navCtrl.setRoot(HomePage);
          this.events.publish('user:login');
          loading.dismiss();
          this.showToast(undefined);
        }, (err: IDetailedError<string[]>) => {
          console.log(err);
          loading.dismiss();
          this.showToast(err)
        });
    }
  }

  showToast(response_message: any) {
    let toast = this.toastCtrl.create({
      message: (response_message ? response_message : "Log In Successfully"),
      duration: 1500
    });
    toast.present();
  }

  onSignup() {
    this.navCtrl.push(SignupPage);
  }
}
```

正如前面的代码所示，在代码层面创建 loader 和 toast 几乎相似。创建时提供的选项也相似；我们在这里在登录时使用了 loader，之后使用 toast 来显示所需消息。

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_111_0.png)

*设置 duration 选项是好的，因为如果 loader 被 dismiss 或在代码中未正确处理，那么我们将阻止用户对应用进行任何进一步交互。在向服务器的 HTTP 调用中，我们可能会遇到连接问题或失败情况；在这种情况下可能会导致用户被阻塞。*


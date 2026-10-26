

| 商品 | 价格1 | 价格2 |
| --- | --- | --- |
| 纯银 ... | USD5.28 | USD6.28 |
| 纯银 ... | USD6.63 | USD7.95 |
| 9K金CZ Cr... | USD15.20 | USD22.25 |
| 9K金CZ Cr... | USD7.60 | USD11.25 |

```
// src/filters/ordinal.ts

import { Pipe, PipeTransform } from '@angular/core';

@Pipe({name: 'ordinal'})

export class OrdinalPipe implements PipeTransform { 
 transform(value: number, args: string[]): any { 

 if (!value) return value;

 if(isNaN(value) || value < 1) { 
 return value;
 } else { 
 var lastDigit = value % 10;

 if(lastDigit === 1) { 
 return value + 'st' 
 } else if(lastDigit === 2) { 
 return value + 'nd' 
 } else if (lastDigit === 3) { 
 return value + 'rd' 
 } else if (lastDigit > 3) { 
 return value + 'th' 
 } 
 } 
 } 
}

// using pipe in template

<p> {{ data.value | ordinal }} </p>

import { OrdinalPipe } from './filters/ordinal';

@NgModule({ 

 declarations: [ 
 . 
 . 
 . 
 OrdinalPipe, 
 Settings 
 ] 

})

<p>{{ data.created_at | date | uppercase }}</p>
```

```typescript
// src/pages/login/login.ts
import { NavController, LoadingController, ToastController, Events } from 'ionic-angular';

constructor(public navCtrl: NavController, public loadingCtrl: LoadingController, public auth: Auth, public userService: UserService, public toastCtrl: ToastController, public events: Events) { }

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

<ion-buttons end>
  <button ion-button (click)="goToCart()">
    <ion-icon ios="ios-cart" md="md-cart"></ion-icon>
  </button>
</ion-buttons>

// src/pages/categories/categories.ts
import { NavController, PopoverController } from 'ionic-angular';
import { ShowcartPage } from '../showcart/showcart';

export class CategoriesPage {
  constructor(public navCtrl: NavController) { }

  goToCart() {
    this.navCtrl.push(ShowcartPage);
  }
}
```

这里你可以看到我们已经推送了 ShowcartPage 组件。我们还可以将数据作为 push 函数的下一个参数传递到下一页，并在下一页使用 navParams 类及其 get 方法来获取数据。

## 从覆盖组件进行导航

最近，在从弹出框导航到页面时，我遇到了一个问题。每次我点击弹出框内的链接并直接推送下一个组件，应用就会冻结。我们将实现弹出框组件，并在其中导航到 SettingsPage 和 SupportPage：

```
// src/pages/popover/popover.ts
import { Component } from '@angular/core';
import { ViewController, NavController, App, ModalController } from 'ionic-angular';

import { SupportPage } from '../support/support';
import { SettingsPage } from '../settings/settings';

@Component({
  template: ` 
    <ion-list>
      <button ion-item
      (click)="goToPage('SettingsPage')">Settings</button>
      <button ion-item
      (click)="goToPage('SupportPage')">Support</button>
      <button ion-item
      (click)="close('http://ionicframework.com/docs/v2/getting-started')">Learn Ionic</button>
    </ion-list>
  `;
})
export class PopoverPage {

  classes: any;

  constructor(
    public viewCtrl: ViewController,
    public navCtrl: NavController,
    public app: App,
    public modalCtrl: ModalController
  ) {

    this.classes = {
      'SettingsPage': SettingsPage,
      'SupportPage': SupportPage
    }

  }

  goToPage(page:any) {
    // Navigating from Overlay Components
    // https://ionicframework.com/docs/v2/api/navigation/NavController/
    // If you simply push it will freeze the UI
    this.viewCtrl.dismiss();
    this.app.getRootNav().push(this.classes[page]);
  }

  close(url: string) {
    window.open(url, '_blank');
    this.viewCtrl.dismiss();
  }
}
```

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_128_0.png)

如你所见，我们没有直接推送 Page 组件，而是先通过 getRootNav() 方法获取应用中 rootNav 的引用。

## 生命周期事件与导航守卫

生命周期事件在 Ionic 1 中就已经存在，原生平台也有结构完善的生命周期，这让我们能够在导航的各阶段执行操作。目前有六个页面事件和两个导航守卫可用：

- ionViewDidLoad: 页面加载完成时
- ionViewWillEnter: 页面即将进入时
- ionViewDidEnter: 页面完全进入时
- ionViewWillLeave: 页面即将离开时
- ionViewDidLeave: 页面结束离开时
- ionViewWillUnload: 页面即将被销毁时
- ionViewCanEnter: 视图进入前运行
- ionViewCanLeave: 视图退出前运行

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_131_0.png)

ionViewCanEnter 和 ionViewCanLeave 是两个导航守卫，用于控制视图的进入和离开。例如，如果我们正在填写表单，不小心点击了某个按钮或菜单项，用户已填写的数据会立即被清除。为了保护数据，我们可以像在 support 页面上那样应用 ionViewCanLeave：

```typescript
// src/pages/support/support.ts

// If the user enters text in the support question and
// then navigates
// without submitting first, ask if they meant to leave
// the page
ionViewCanLeave(): boolean | Promise<boolean> {
// If the support message is empty we should just
// navigate
if (!this.supportMessage ||
this.supportMessage.trim().length === 0) {
return true;
}

return new Promise((resolve: any, reject: any) => {
let alert = this.alertCtrl.create({
    title: 'Leave this page?',
    message: 'Are you sure you want to leave this page? Your support message will not be submitted.'
});
alert.addButton({ text: 'Stay', handler: reject });
alert.addButton({ text: 'Leave', role: 'cancel',
handler: resolve });

alert.present();
});
}
```

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_132_0.png)

导航对于我们构建的任何应用来说都是重要组成部分，因此我个人的建议是，认真阅读并理解导航组件的所有重要主题，无论大小。我最初犯的一个错误是没有通读全部文档，由于之前提到的冻结问题，这几乎浪费了我一整天。你可以通过 [https://ionicframework.com/docs/v2/api/navigation/NavController/](https://ionicframework.com/docs/v2/api/navigation/NavController/) 查看导航 API。

## 国际化与本地化

当我们需要构建一款影响世界各地用户生活的全球化应用时，我们必须考虑国际化与本地化等多个方面。我们将把它集成到我们的电商应用中，以便添加多语言支持。

我们首先安装 `ng2-translate` npm 模块：

```
npm install ng2-translate --save
```

接下来，我们将在 `app.module.ts` 中引入所需模块和方法：

```
// /src/app/app.module.ts
import { TranslateModule, TranslateLoader, TranslateStaticLoader } from 'ng2-translate/ng2-translate';

export function createTranslateLoader(http: Http) {
  return new TranslateStaticLoader(http, './assets/i18n', '.json');
}

@NgModule({
  imports: [
    IonicModule.forRoot(vPlanetApp),
    TranslateModule.forRoot({
      provide: TranslateLoader,
      useFactory: createTranslateLoader,
      deps: [Http]
    }),
    CloudModule.forRoot(cloudSettings)
  ]
})

export class AppModule {}
```

我们创建了一个独立函数 `createTranslateLoader`，在其中定义了 `i18n/` 文件夹，该文件夹包含每种语言的 JSON 文件。现在我们可以使用该模块了，并在 `app.component.ts` 中初始化 `TranslateService`，以确定使用哪种语言：

```
import {TranslateService} from 'ng2-translate/ng2-translate';

@Component({
  templateUrl: 'app.template.html',
  providers: [TranslateService]
})

export class vPlanetApp {

  constructor(
    public translate: TranslateService
  ) {

    this.initializeTranslateServiceConfig();

  }

  // We are using the user default language
  // and set that default locale

  initializeTranslateServiceConfig() {
    var userLang = navigator.language.split('-')[0];
    userLang = /(en|es)/gi.test(userLang) ? userLang :
    'en';
    this.translate.setDefaultLang('en');
    this.translate.use(userLang);
  }
}
```


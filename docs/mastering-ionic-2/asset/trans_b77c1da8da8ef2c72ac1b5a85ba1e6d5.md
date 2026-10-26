

高度，虽然不一定要给出确切数字，但接近确切高度会更好。

无限滚动是指在用户滚动到指定距离后，页面内容会被动态追加。在无限滚动中，会调用一个函数来异步获取新数据并追加到列表中：

```html
<ion-content>
  <ion-list>
    <ion-item *ngFor="let product of products">
      {{product.name}}</ion-item>
  </ion-list>

  <ion-infinite-scroll
    (ionInfinite)="getNewProducts($event)">
    <ion-infinite-scroll-content></ion-infinite-scroll-content>
  </ion-infinite-scroll>
</ion-content>
```

`ionInfinite` 属性用于调用 `getNewProducts()` 函数，该函数最终会获取新数据：

```typescript
export class ProductsPage {

  constructor() {}

  getNewProducts(infiniteScroll) {
    // Call an HTTP function or service
    // fetches new data and append to the products array list.
    // Make sure to call complete method to close the loader icon.
    infiniteScroll.complete();
  }
}
```

正如你所看到的，虚拟滚动和无限滚动在模板及其方法方面几乎是相同的。你可以在两个组件中找到相似的属性，在其中可以声明 `loadingText` 和 `loadingSpinner`。如果需要，我们可以进一步使用自定义的 SVG 和 CSS 动画来定制虚拟滚动和无限滚动的设计。

### Ionic 评分

Ionic 评分是一个用于可视化评分条的第三方模块。我们将在应用的商品详情页中使用这个模块。该模块以 NPM 模块的形式提供，因此集成起来非常快速简单：

```bash
$ npm install --save ionic2-rating
```

```typescript
// src/app/app.module.ts

import { Ionic2RatingModule } from 'ionic2-rating';

imports: [
    IonicModule.forRoot(vPlanetApp),
    Ionic2RatingModule,
    TranslateModule.forRoot({
        provide: TranslateLoader,
        useFactory: createTranslateLoader,
        deps: [Http]
    }),
    CloudModule.forRoot(cloudSettings)
],

// src/pages/product-detail/product-detail.html
<rating [(ngModel)]="rate" readOnly="false" max="5"></rating>
```

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_145_0.png)

Ionic 社区在过去三年中发展迅速；GitHub 上有多个第三方组件可用，你可以在应用中间巧妙地使用它们，就像我们在电商应用中使用 ionic-rating 插件一样。其中一些组件包括：

-   [Ionic 评分](https://github.com/andrucz/ionic2-rating)
-   [图片缓存和加载器](https://github.com/zyra/ionic-image-loader)
-   [强制安装](https://github.com/NextFaze/ionic-manup)
-   [地理围栏](https://github.com/tsubik/ionic2-geofence)
-   [手风琴组件](https://github.com/mahmoudissmail/ionic2Accordion)
-   [日历组件](https://github.com/twinssbc/Ionic2-Calendar)

寻找这些优秀代码的最佳方式是在 GitHub 上搜索并查看星标数最高的 Ionic 项目。我一直在使用这种技巧来了解开发者们在 Ionic 上默默做着什么或正在研究什么。大多数情况下，这些项目都采用 MIT 许可证，因此你可以在应用中自由使用它们。

## 使用 Ionic 3 实现懒加载

懒加载是 Ionic 3 最近发布的一个备受期待的功能。懒加载将改善应用的启动时间，减少打包体积，并消除在每个类中包含相同组件路径的麻烦，从而简化设置 Ionic 路由的过程。让我们看看需要做哪些更改，以及你的应用将如何改进。另外，在第 5 章“身份验证、授权与安全”中，应用演示将实现懒加载。

目前，懒加载组件存在一些已知的缺陷，Ionic 团队正在积极解决。此外，设置懒加载会覆盖 `IonicModule.forRoot` 中定义的 `DeepLinkConfig`。我们将为每个组件引入 `IonicPageModule`，并在每个页面组件中添加一个新的 `module.ts` 文件。请务必持续关注 Ionic 博客以及任何可能带来更多缺陷修复和新功能的新版本。

以前，我们在 `app.module.ts` 中一次性导入所有页面，而现在我们只初始化加载 `app.component.ts`。因此，像 `HomePage` 这样的所有组件都从 declarations 和 entryComponents 中移除。接下来，在我们的 `src/pages/home` 目录中，我们引入了另一个文件 `home.module.ts`，类似于我们在 `app.module.ts` 中的文件。现在，如果你使用 Ionic CLI 生成一个页面，你会发现创建了四个文件：

```text
home.html

home.module.ts

home.scss

home.ts

// src/pages/home/home.ts

import { Component } from '@angular/core';

import { IonicPage, NavController } from 'ionic-angular';

@IonicPage()

@Component({
    selector: 'page-home',
    templateUrl: 'home.html',
})

export class HomePage {
    constructor(public navCtrl: NavController) {}

    moveTo(data: any) {
        this.navCtrl.push("ContactPage", data);
    }
}
```

此外，现在如果我们想导航到（push）或返回（pop）任何其他组件，不再需要导入它。我们可以直接将其作为字符串处理；在构建过程中，会生成知道如何处理该字符串的深层链接。该字符串实际上是对 `@IonicPage` 装饰器的 `name` 属性的引用，该属性默认为类名字符串。如果我们将该 `name` 属性改为其他值，还需要更新在其他地方使用的引用。你可以在这里（http://ionicframework.com/docs/v2/nightly/api/navigation/IonicPage/）找到关于 `IonicPage` 及其支持字段的更多信息。

### 小结

Ionic 组件和相关资源是任何 Ionic 应用的基础。通过智能使用组件和可用的第三方组件，我们可以获得更好的性能，并使我们的应用对用户更友好。在正确的位置选择正确的组件是很讲究的，最有趣的部分是我们必须从最终用户的角度以及最终用户对应用体验的角度去思考。我们必须确保用户不会感觉自己处于一个不同的世界，不会脱离原生体验，这种情况经常发生。我们可以在本章中涵盖尽可能多的重要组件和重点，但更建议的是持续关注社区动态，并寻找可以轻松用于应用的可用资源。接下来，我们将转向应用内插件的集成。由于几乎每个应用都使用某种插件，因此正确集成插件对于生产级应用至关重要。相机、GPS、文件系统和网络插件是常用的插件。我们将集成这些常用插件，以及一些许多开发者在集成时遇到问题的复杂插件。Ionic 团队已经发布了一个很棒的项目——Ionic Native，它简化了 Cordova 插件的使用流程。Ionic Native 是一个基于许多插件的包装库，因此我们可以轻松地将其集成到基于 Angular 的 Ionic 应用中。

## Ionic Native 和插件

现在我们已经了解了 Ionic 中的一些高级组件和 API。构建混合移动应用的另一个重要部分是 Cordova 插件。插件在原生 API 和 JavaScript 之间建立了桥梁，帮助我们使用相机、GPS、文件等原生 API。插件由特定平台的原生库代码和用于调用相应原生平台方法的 JavaScript 接口组成。Ionic 依赖 Cordova 来构建、调试和发布应用。在本章中，我们将涵盖有关 Cordova 插件、Ionic Native 以及一些常见和高级插件的几乎所有内容。起初，Ionic 团队推出了基于 Angular 1.x 的 ngCordova，但后来我们有了支持 Ionic 1、2 和 3 应用的 Ionic Native。ngCordova 绑定于 Ionic 1，而 Ionic Native 与框架无关：

-   Cordova 以及插件的工作原理
-   Ionic Native
-   常用 Cordova 插件（相机、GPS、通讯录）
-   构建音乐播放器
-   Cordova 与 PhoneGap 的对比



现在我们可以像使用管道一样在模板中使用它。因为我们在电商应用中有循环渲染的侧边菜单列表项，所以我们首先会在侧边菜单上实现翻译：

```
<ion-list>
    <ion-list-header>
        {{'navigate' | translate}}
    </ion-list-header>
    <button ion-item menuClose *ngFor="let p of appPages" (click)="openPage(p)">
        <ion-icon item-left [name]="p.icon" [color]="isActive(p)"></ion-icon>
        {{ p.title | translate }}
    </button>
</ion-list>
```

一旦设置了区域设置，它就会从 `/assets/i18n` 文件夹中的 JSON 文件提取对应的标签。目前我们在该应用中只实现了 `en` 和 `es` 两种区域设置，但我们可以通过为电商应用想要支持的每种语言添加更多 `locale.json` 文件来轻松扩展：

```
// es.json
{
    "menu": "Menú",
    "home": "Casa",
    "navigate":"Navegar",
    "categories":"Categorías",
    "cart":"Carrito de compras",
    "wishlist":"Lista de deseos",
    "about": "Acerca de",
    "categories":"Categorías",
    "login": "Iniciar sesión",
    "logout":"Cerrar sesión",
    "account":"Cuenta",
    "signup":"Regístrate",
    "popular":"Productos Populares",
    "settings":"Ajustes",
    "feedback":"Realimentación"
}
```

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_136_0.png)

上面是一个 `es.json` 示例，其中我们放置了不同的标签，用于进行内容翻译。接下来，如果你想翻译 TypeScript 文件中的任何 JavaScript 值，可以使用 `translateService` 及其 `get` 方法，它会订阅 observable 并返回翻译后的字符串：

```
translateService.get('HELLO').subscribe(
  value => {
    // value returned is translated
    console.log(value);
  }
)
```

最初设置和翻译该模块可能会花费一些时间，因为涉及多个部分，开发人员在设置时可能会忽略一些要点。为应用添加国际化功能将产生巨大的影响，并拓宽你能够吸引的用户群；这有助于你在 Play Store 上获得更好的评分。

## ItemSliding 和下拉刷新

在现代应用中，动画和手势是必不可少的，它们能够在用户使用应用功能时提升用户体验。你会在大多数 iOS 应用中找到 `ItemSliding` 功能，而下拉刷新通常用于获取最新数据，就像 Facebook 应用一样，你会用它来获取最新的信息流。

`ItemSliding` 是一个可滑动的列表，可以向左或向右滑动以显示操作按钮。我们将在收藏夹页面使用可滑动列表，在该页面中，我们可以对产品执行从列表中删除或添加到购物车的操作：

```
// src/pages/wishlist/wishlist.ts
<ion-content>
  <ion-list>
    <ion-list-header>
      4 Items in your Wishlist (Swipe for options).
    </ion-list-header>

    <ion-item-sliding *ngFor="let product of products">
      <ion-item>
        <ion-avatar item-left>
          <img [src]="product.image">
        </ion-avatar>
        <h2>{{product.name}}</h2>
        <h3>{{product.price | currency}}</h3>
        <p>{{product.rating}} stars and 420 reviews</p>
      </ion-item>
      <ion-item-options side="right">
        <button danger>
          <ion-icon name="trash"></ion-icon>
          Remove
        </button>
        <button secondary>
          <ion-icon name="cart"></ion-icon>
          Add Cart
        </button>
      </ion-item-options>
    </ion-item-sliding>
  </ion-list>
</ion-content>
```

让我们来看看 `itemSliding` 上可用的一些属性：

- 滑动方向可以是向右或向左；两者都会显示操作按钮。我们可以使用 `<ion-item-options side="right">` 来设置右滑选项
- 在滑动列表项时，可以使用 `IonDrag` 和 `ionSwap` 事件来监听或调用某些函数
- 图标可以放在按钮文本的上方或左侧，例如 `<ion-item-options icon-left>`
- 也可以导入 `ItemSliding` 类来访问 `close()` 方法

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_139_0.png)

如你所见，上面的截图展示了 `itemSliding` 列表的外观。下拉刷新是另一个组件，主要用于我们需要刷新现有内容并从后端服务器获取新数据时。`refresher` 模块会检测垂直滑动，并在调用 `refresh` 函数时显示加载图标。虽然我们在应用中没有确切的用例需要使用它，但实现 `refresher` 非常简单：

```html
<ion-content>
    <ion-refresher (ionRefresh)="updateFeed($event)">
        <ion-refresher-content
            pullingText="Updating Feed"
            refreshingSpinner="circles"
            refreshingText="Updating...">
        </ion-refresher-content>
    </ion-refresher>
    <h4>User Feed</h4>
</ion-content>
```

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_139_1.png)

`ion-refresher` 组件具有 `ionRefresh` 属性，当刷新器被垂直拉动到一定程度时会调用该属性。之后，正如你在 `ion-refresher-content` 中看到的，有多组属性实际上充当着刷新器的配置设置，具有多种选项：

```typescript
import { Http } from '@angular/http';

export class FeedPage {
    constructor(public http: Http) {}

    updateFeed(refresher) {

        // Calling a web service to fetch new feeds
            result
            this.http.get('path/to/data.json')
                .map(res => res.json())
                .subscribe(data => {
                    // After the callback we will
                        complete the refresher
                        // and update the data inside the view.
                        refresher.complete();
                });
        }
}
```

在我们调用 `ionRefresh` 函数后，一旦从服务器获取到数据，就需要关闭刷新过程。当来自服务器的异步请求完成时，会调用 `complete()` 方法。我们必须确保调用此方法，否则加载图标将不会消失。

## 虚拟滚动和无限滚动

虚拟滚动和无限滚动是两个重要的组件，它们同样广泛应用于不同的应用中，以处理列表中大量的项目。虚拟滚动用于实现一次渲染可能会过长而无法完成的列表，例如包含数千个联系人的联系人列表。如果我们尝试加载整个列表并进行渲染，将会阻碍我们应用的性能，并且在向下滚动列表时会感到卡顿。让我们来看看如何在我们的产品页面中实现虚拟滚动：

```
// src/pages/products/products.html
<ion-content class="products">
<ion-list>
<ion-item>
<ion-note item-left>
Duplicate products for VirtualScroll demo
</ion-note>
</ion-item>
</ion-list>
<ion-grid [virtualScroll]="products" approxItemHeight="40px">
<ion-row *virtualItem="let product" (click)="goToProductDetail(product)">
<ion-col width-33>
// Important to use ion-img rather than img tag
// this will lazy load the images as we scroll downwards
// Also, image sizes should not vary after its loaded
<ion-img class="v-img" [src]="product.image">
</ion-img>
</ion-col>
<ion-col width-67>
<p>{{ product.name }}</p>
<ion-badge item-right>4.1</ion-badge> 340 ratings
<p class="price"> {{ product.price | currency: 'INR' }} </p>
</ion-col>
</ion-row>
</ion-grid>
</ion-content>
```

在使用虚拟滚动组件时，需要注意的一些重要点是：在列表加载后，我们不应进行任何 DOM 操作或更改数据集，因为这需要重置或刷新整个列表，这是一项对内存消耗很大的操作。为了虚拟滚动能够流畅工作，需要指定列表内项目的近似宽度和高度。默认值为 40px。

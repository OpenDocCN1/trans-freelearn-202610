# Firebase 和 TasteBite 应用简介

开始使用 Firebase 非常简单；这也是许多初创公司和成长型产品使用像 Firebase 这样的后端即服务平台的主要原因。我们将快速使用 Firebase 和 Ionic 创建一个 TasteBite 应用。在这个应用中，我们为使用 Firebase 的 Angular 应用集成了 AngularFire2 包装器，不过我们也可以在应用内直接使用 JavaScript SDK。你需要在 Firebase 创建一个账户（最初可以使用免费账户；随着使用量增加，你可以轻松升级账户）。

Firebase 让开发者的生活更轻松，因为他们不必花时间构建后端服务器来提供简单的 API。我们可以用最少的精力轻松完成所有 CRUD 操作，并且借助 WebSocket 支持，数据同步速度极快。此外，Firebase 还提供离线同步、用户身份验证、以及通过规则集保护数据的功能：

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_297_0.png)

创建账户后，你就可以在 Firebase 中创建应用；你会看到添加项目的选项。添加/创建项目后，你可以管理并进入你的应用。你会看到诸如分析、身份验证、数据库、存储、托管、通知以及许多其他选项。随着我们继续为应用创建数据，我们将使用其中的许多功能。你可以在下面的截图中看到，我们的应用中已经存储了一些数据。所有 Firebase 数据库数据都存储为 JSON 对象，不存在关系表。

## 构建 TasteBite 并安装 Firebase

我们首先从零开始创建一个应用，并安装此应用所需的所有插件和包。在此应用中，我们将使用 Tabs 模板并集成懒加载。我们将在应用中使用地理位置、相机和应用内浏览器插件：

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_299_0.png)

```
$ ionic start tastebite tabs
$ ionic cordova platform add android
$ ionic cordova plugin add cordova-plugin-inappbrowser
$ ionic cordova plugin add cordova-plugin-geolocation
$ ionic cordova plugin add cordova-plugin-camera
$ npm install --save @ionic-native/in-app-browser
$ npm install --save @ionic-native/geolocation
$ npm install --save @ionic-native/camera
```

现在，我们准备在 Ionic 应用中设置 Firebase：

```
$ npm install angularfire2 firebase --save
```

接下来，我们将在 app.module.ts 中导入 angularfire2 库：

```
// src/app/app.module.ts

import { AngularFireModule } from 'angularfire2';

export const firebaseConfig = {
  apiKey: "AIzaSyDnd6hMcfJDWGdGr6EOSS05NoMM2CeY2bw",
  authDomain: "tastebite-ec5c4.firebaseapp.com",
  databaseURL: "https://tastebite-ec5c4.firebaseio.com",
  storageBucket: "tastebite-ec5c4.appspot.com",
  messagingSenderId: "762350093850"
};

@NgModule({
  declarations: [
    Tastebite
  ],
  imports: [
    BrowserModule,
    HttpModule,
    IonicModule.forRoot(Tastebite, {
      preloadModules: true
    }),
    AngularFireModule.initializeApp(firebaseConfig)
  ],
```

确保你如上例所示包含 `firebaseConfig`，它会有多个配置参数。此外，下一步是在初始化 Firebase 模块时注入 `firebaseConfig`。我们现在可以开始读取和写入应用中的数据了。

## 使用 Firebase 进行身份验证和安全

我们现在来看看 Firebase 用户身份验证，以及我们如何保护数据。Firebase 在内部支持登录和身份验证，并且还提供与社交登录提供商集成的支持。大多数应用需要某种内部登录机制；使用 Firebase 设置起来非常容易。每当我们使用 Firebase 登录时，就会创建一个会话，并返回一个唯一 ID `uid`，该 ID 在所有提供者中都是唯一的，并且对于特定的已认证用户永远不会改变。这也将在以后帮助我们保护数据。

用户完成身份验证后，即使应用重启，其会话也会被管理和记住。Firebase 为多个平台提供用户身份验证：

- **自定义** - 用于与现有身份验证系统集成
- **电子邮件** - 通过电子邮件和密码注册并验证用户。
- **匿名** - 用于无需任何个人信息的访客账户
- Facebook
- Google
- Twitter
- GitHub

## 启用提供程序

我们需要从应用程序的“身份验证-登录方式”选项卡中启用身份验证提供程序。我们已经为 TasteBite 应用启用了基于电子邮件的登录和 Facebook 登录。使用基于电子邮件的登录来注册、登录和注销用户非常容易。如果我们已经有一些身份验证系统，则可以使用自定义身份验证；如果我们想允许访客登录，可以使用匿名用户身份验证。我们也将演示如何轻松集成社交登录。

所有社交登录都需要在社交渠道上创建的应用的客户端 ID 和客户端密钥。所有社交提供者都有不同的创建应用的方式，创建应用后，我们可以将客户端 ID 和客户端密钥放到 Firebase 应用中。我们需要确保已将 OAuth 重定向 URL 放置在 Facebook 登录产品中：https://tastebite-ec5c4.firebaseapp.com/__/auth/handler。在此之前，你必须在 Facebook 上创建一个应用程序，我们将把它的 app_id 和 app_secret 添加回 Firebase Facebook 提供者设置中。在此之后，你的后端工作就完成了，你只需从 Ionic 应用调用一个函数即可。

我们现在将生成 AuthServiceProvider，它将用于 AngularFire2 身份验证：

```
$ ionic g provider AuthService
```

在我们开始处理此提供程序之前，需要在 app.module.ts 文件中设置 FirebaseAuthConfig：

```
import { AngularFireModule, AuthProviders, AuthMethods } from 'angularfire2';

const myFirebaseAuthConfig = {
  provider: AuthProviders.Facebook,
  method: AuthMethods.Redirect
};

imports: [
  AngularFireModule.initializeApp(firebaseConfig,
    myFirebaseAuthConfig)
]
```

这样我们就不必将此配置传递给基于 AngularFireAuth 的登录函数。不过，我们可以通过内部传递登录方法来覆盖此配置并使用任何其他提供程序。在上面的代码中，你可以看到 AuthMethods 选择的是 Redirect，但为了在浏览器上测试，你可以将其改为 Popup。

现在让我们看看我们的 AuthService：

```
import { Injectable } from '@angular/core';
import { AngularFireAuth, FirebaseAuthState } from 'angularfire2';
import * as firebase from 'firebase';

import { Platform } from 'ionic-angular';
import { Facebook, FacebookLoginResponse } from '@ionic-native/facebook';

@Injectable()
export class AuthServiceProvider {

  private authState: FirebaseAuthState;

  constructor(public auth$: AngularFireAuth, private fb: Facebook,
    public platform: Platform) {
    this.authState = auth$.getAuth();
```



```typescript
auth$.subscribe((state: FirebaseAuthState) => {
    console.log(state);
    this.authState = state;
});
}

get authenticated(): boolean {
    return this.authState !== null;
}

signInWithFacebook(): firebase.Promise<FirebaseAuthState | FacebookLoginResponse> {
    if (this.platform.is('cordova')) {
        return this.fb.login(['email', 'public_profile']).then(res => {
            const facebookCredential = firebase.auth.FacebookAuthProvider.credential(res.authResponse.accessToken);
            return firebase.auth().signInWithCredential(facebookCredential);
        });
    } else {
        return this.auth$.login();
    }
}

signOut(): void {
    this.auth$.logout();
}

authData(): any {
    if (this.authState != null) {
        return this.authState.auth;
    } else {
        return '';
    }
}
}
```

我们最初安装了 InAppBrowser Cordova 插件来处理此身份验证和重定向。

我们还确认过，当我们在真实设备上运行时，会使用 Ionic Native 的 Facebook 插件。在这里，我们先调用 Facebook 插件，成功登录并从 Facebook 获取令牌后，再将其传递给 Firebase 的 `firebase.auth.FacebookAuthProvider` 函数进行身份验证。我们在 TasteBite 应用中使用了 Facebook 登录，不过如果需要，我们也可以实现其他提供商。

## 用户认证状态

Firebase 提供了一些与认证相关的 API，帮助我们获取认证状态并持久化它。为了侦测与密码更改或会话过期相关的任何认证变化，我们需要在初始化应用时监听 AngularFireAuth 类的 subscribe 函数：

```typescript
constructor(public auth$: AngularFireAuth) {
  this.authState = auth$.getAuth();
  auth$.subscribe((state: FirebaseAuthState) => {
    console.log(state);
    this.authState = state;
  });
}
```

我们将这段代码放在了 AuthServiceProvider 中；同样，如果你想停止监听这些变化，可以调用 offAuth() 函数，这样就不会再收到任何更新。在用户会话有效期间，Firebase 会把会话保存到本地存储中；不过，如果你需要获取认证信息，可以调用 getAuth() 函数获取所有会话详情。为了减少服务器请求，我们可以使用 localStorage 方法从存储中获取所有细节：

```typescript
signOut(): void {
  this.auth$.logout();
}
```

用户会话目前会一直保持有效，直到调用 logout()（或用户清除本地存储）才会终止。最后，我们需要一个从 Firebase 注销会话的函数，可以使用 unauth()。

## 使用 AngularFire2 进行 CRUD 操作与数据安全

现在我们已经涵盖了与认证相关的代码，并为我们的 TasteBite 应用搭建好了基础模块。接下来，我们将了解 Firebase Realtime Database，这是一个 NoSQL 云数据库。数据以 JSON 格式存储，并通过 WebSocket 实时同步。Firebase 数据库会将数据持久化到磁盘，这也为我们的应用提供了离线支持。我们将在这里使用两个对象：foodItems 和 rateItems，其中 foodItems 包含菜品的详细信息，而 rateItems 是一个嵌套对象，包含每个“菜品 + 城市”组合的评分和评论：

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_309_0.png)

正如你所看到的，我们根据 userid 组织了 foodItem 数据，以便区分与用户相关的数据。我们可以在仪表盘中遍历到 personalFavourite，然后访问用户数据节点。我们可以访问数据中的任何子节点。我们可以在仪表盘中将数据可视化；此外，Firebase 团队还创建了一个名为 Vulcan 的 Chrome 插件，我们可以用它来查看数据树。

由于 Firebase 中存储的数据都是对象，并且不原生支持数组。不过，为了简化开发，Firebase 在其 SDK 中提供了将数据推送到数组的方法。Firebase 使用顺序键来存储数据：

```js
// push send this ['a', 'b', 'c', 'd', 'e']
// Firebase databases store this
{0: 'a', 1: 'b', 2: 'c', 3: 'd', 4: 'e'}
```

在 Firebase 上使用数组类型存储存在一些限制。此外，使用 Firebase 时，子节点的深度最多可达 32 层，键长度可以为 768 字节。我们不会深入探讨 Firebase 的基础知识，因为我们可以轻松地在其官方文档中找到这些内容。

## CRUD 操作

要从数据库读取或写入数据，我们需要一个数据库引用实例。虽然我们使用的是 AngularFire，这些函数已经被封装，我们直接使用 AngularFire 库提供的函数；但有时我们也必须使用底层方法，就像稍后在该项目中要做的那样。现在，我们首先开始查询 foodItems，以便将它们显示在我们的 HomePage 上：

```typescript
// src/pages/home/home.ts
import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';
import { AngularFire, FirebaseListObservable } from 'angularfire2';
import { CityProvider } from '../../providers/city/city'

@IonicPage()
@Component({
  selector: 'page-home',
  templateUrl: 'home.html',
})
export class HomePage {

  foodItems: FirebaseListObservable<any[]>;
  item;
  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public cityService: CityProvider,
    public af: AngularFire) {
```

由于我们将按城市查询数据，因此我们使用了 cityService，它会根据我们的地理位置返回我们当前所在的城市。这样我们将只获取来自我们城市的数据：

```typescript
this.cityService.getCityName().then(result => {
    let cityName = result[0]['address_components'][3].long_name;
    this.foodItems = af.database.list('/foodItems', {
      query: {
        orderByChild: 'city',
        equalTo: cityName
      }
    });
  })
}

ratingPage(data:any) {
  console.log(data);
  // In FoodRatingPage we will be showing
  // all the reviews and rating for each item
  this.navCtrl.push("FoodRatingPage", data);
}
}
```

接下来，如果用户点击任何一个菜品，它将显示与该菜品关联的所有评论和评分。

如前所述，我们使用了一个城市提供者，它首先使用 Ionic Native 地理定位插件，然后将纬度和经度传递给 Google API，Google API 会返回该地点的完整详细信息，包括街道名称、城市、州和国家。然后我们从中解析出城市名称，并在应用中使用它：

```typescript
// src/providers/city/city.ts
import { Injectable } from '@angular/core';
import { Http } from '@angular/http';
import 'rxjs/add/operator/map';

import { Geolocation } from '@ionic-native/geolocation';

@Injectable()
export class CityProvider {

  constructor(public http: Http, private geolocation: Geolocation) {
    console.log('Hello CityProvider Provider');
  }

  getCityName() {
    return new Promise(resolve => {
      this.geolocation.getCurrentPosition().then((resp) => {
        let url =
        "http://maps.googleapis.com/maps/api/geocode/json?latlng="
        +resp.coords.latitude+","+resp.coords.longitude
        this.http.get(url).map(res =>
        res.json()).subscribe(data => {
          resolve(data.results);
        });
      }).catch((error) => {
        console.log('Error getting location', error);
      });
    })
  }
}
```

当用户导航到下一页时，我们会传递特定的 foodItem 数据，这些数据稍后会在该页面中用于获取 foodRatings。我们将 foodRatings 和 foodItems 使用相同的 `$key` 进行存储，这样在这种情况下更容易获取数据：

```typescript
// src/pages/food-rating/food-rating

import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';

import { AngularFire, FirebaseListObservable } from 'angularfire2';

@IonicPage()
@Component({
  selector: 'page-food-rating',
  templateUrl: 'food-rating.html',
})
export class FoodRatingPage {
  item:any;
  ratings:any;
  foodRatings: FirebaseListObservable<any[]>;

  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public af: AngularFire) {

    this.item = this.navParams.data;
    this.foodRatings =
      af.database.list('/rateItems/'+this.navParams.data.$key);
  }

  ionViewDidLoad() {
    console.log('ionViewDidLoad FoodRatingPage');
  }

  ratingPage() {
    // Will push to RateFoodPage where we will rate this
    // specific foodItem+city combo
    this.navCtrl.push("RateFoodPage",
    this.navParams.data);
  }
}
```





现在，让我们看看如何在 Firebase 中创建一条条目。假设我们要对 `foodItem` 给出**评论**和**评级**，我们会在 `rateFoodPage` 页面上完成。这里我们再次使用同一个 `$key` 在数据库中存储新的评论。

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_313_0.png)

```typescript
// src/pages/rate-food/rate-food

import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';

import { AngularFire, FirebaseListObservable } from 'angularfire2';
import * as firebase from 'firebase';

@IonicPage()
@Component({
  selector: 'page-rate-food',
  templateUrl: 'rate-food.html',
})
export class RateFoodPage {

  rateItems: FirebaseListObservable<any[]>;
  data: {name?: string, review?: string, rating?: any} = {};
  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public af: AngularFire) {

    this.rateItems = af.database.list('/rateItems');
    console.log(this.navParams.data);
    this.data.name = this.navParams.data.name;
  }

  rate() {
    let data = {
      "rating": this.data.rating,
      "review": this.data.review
    }
    let url = "rateItems/" + this.navParams.data.$key +
      "/" +
      this.makeid();
    firebase.database().ref().child(url).set(data);
    this.navCtrl.pop();
  }

  makeid() {
    var text = "";
    var possible =
      "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

    for( var i=0; i < 8; i++ )
      text += possible.charAt(Math.floor(Math.random() *
        possible.length));

    return text;
  }
}
```

我们使用了 `makeid()` 函数为多个用户对同一 `foodItems` 的评论/评级生成随机的 `$keys`。在这里你可以看到，我们没有使用 `AngularFire` 方法，而是直接使用了 Firebase 方法，因为我们无法使用 `AngularFire` 标准函数以这种方式插入数据。

除了创建和查询数据之外，我们还可以使用 `AngularFire` 更新和删除对象。我们还没有在应用中使用这些功能，但它们实际上很容易执行：

```javascript
// Deleted the object and handles error via promise
const promise = af.database.object('/item').remove();
promise
  .then(_ => console.log('success'))
  .catch(err => console.log(err, 'You don\'t have access!'));

// updates data of an object
const itemObservable = af.database.object('/item');
itemObservable.update({ age: newAge });
```

> 每一项技术和框架都在定期更新，因此我必须确保每个章节所关联的代码库尽可能频繁地更新。此外，很多时候章节中无法写尽所有功能，所以我已经尽最大努力在每个章节的演示项目中覆盖尽可能多的主题。因此，请务必仔细阅读代码库，只要有不一致的地方，我都会在代码中添加注释。

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_314_0.png)

## 数据的结构与安全

在 Firebase 中存储和构建数据时，我们必须再三考虑如何为应用获取数据。这与使用任何其他 NoSQL 数据库（例如 MongoDB）时遇到的问题相同。我们应该倾向于将数据扁平化，而不是把所有内容都嵌套起来，并且在获取数据时必须将扁平化的数据重新联结在一起。

让我们以博客网站为例：用户创建博客文章，且所有博客文章都有评论。因此，我们更倾向于将数据拆分，而不是在 Firebase 中嵌套数据：

```json
{
  "users": {
    "user1": {
      "name": "John Smith",
      "type": "editor",
      "blogs": {
        "post1": {
          "id": 123456,
          "title": "Welcome to Ionic",
          "post": "Ionic is an open source SDK for hybrid apps"
        },
        "post2": {
          "id": 123457,
          "title": "Welcome to Ionic",
          "post": "Ionic is an open source SDK for hybrid apps"
        }
      }
    },
    "user2": {
      "name": "Adam",
      "type": "admin",
      "blogs": {
        "post1": {
          "id": 234567,
          "title": "Welcome to Ionic",
          "post": "Ionic is an open source SDK for hybrid apps"
        }
      }
    }
  }
}
```

这种存储数据的方式在初期很容易处理，但随着数据量的增加，查询数据集会变慢，性能也会受损。因此，最佳的存储数据方式是：

```json
{
  "users": {
    "user1": {
      "name": "John Smith",
      "type": "editor",
      "blogs": {
        "123456": true,
        "123457": true
      }
    },
    "user2": {
      "name": "Adam",
      "type": "editor",
      "blogs": {
        "234567": true
      }
    }
  },
  "blogs": {
    "123456": {
      "title": "Welcome to Ionic",
      "post": "Ionic is an open source SDK for hybrid apps"
    },
    "123457": {
      "title": "Welcome to Ionic 2",
      "post": "Ionic is an open source SDK for hybrid apps"
    }
  }
}
```

现在我们已经将数据存储在 Firebase 中，下一步是获取数据并将其重新联结起来。联结扁平化数据有点困难，但这种方法带来了很多性能和模块化方面的好处。让我们看看如何获取上述数据并加以联结。

由于 Firebase 使用 WebSocket，通过这种方式获取数据时性能会很高，而且 Firebase 会在内部对传入和传出请求进行优化。

Firebase 还提供了一组安全规则来保护我们的数据，并帮助确定谁对数据集拥有读取和写入权限。所有安全规则都位于仪表板的**安全与规则（Security & Rules）**选项卡中。在我们的应用内部，应用数据是由用户定义并与用户关联的，因此我们相应地设置了规则：

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_317_0.png)

共有三种类型的规则：

- `.read` - 当允许用户读取数据时
- `.write` - 当允许写入数据时
- `.validate` - 定义格式正确的数据，并在推送到数据库之前进行验证

安全规则采用 JSON 结构，正如你在前面的截图中所看到的，这使得理解和设置规则更加容易。该数据会与用户关联，因此我们相应地设置了规则。在我们的应用中，我们首先需要在注册过程中存储与用户相关的数据，例如姓名、电子邮件和其他详细信息：

```json
{
  "rules": {
    "users": {
      "$user_id": {
        ".read": "$user_id === auth.uid",
        ".write": "$user_id === auth.uid"
      }
    }
  }
}
```

我们创建了一个 users 模型来存储这些详细信息。我们将数据嵌套在 `$user_id` 键中，以便创建访问控制，并且只有创建该数据集的用户才能编辑和访问它。类似地，对于 `feeds`，用户只能读取订阅源，而不能向其写入内容：

```json
{
  "feeds": {
    ".read": true,
    ".write": false
  }
}
```

Firebase 内部提供了一些有助于提高查询数据性能的方法。例如，如果我们在一个模型中存储了学生的分数，并且需要按分数查询这些数据，那么我们可以在规则中使用 `.indexOn`，这样获取记录时的性能就会提高。你可以在此处了解更多关于保护 Firebase 中数据的信息：https://firebase.google.com/docs/database/security/。

## 总结

将 Ionic 与 BaaS 平台结合使用，确实为初创公司减轻了很多压力，因为你可以在更短的时间内构建适用于多个平台的应用，而无需任何资源或时间来搭建后端服务器。如今，许多正在成长的公司正转向 Ionic，因为它开发速度快、成本更低。拥有良好 JavaScript 技能的小型开发团队可以构建一切，即使他们想为自定义任务建立后端，也可以使用 Node.js 并在 JavaScript 之上构建整个技术栈。像 Firebase 这样的平台对移动应用来说非常强大，因为它使用 WebSocket，速度非常快，并提供实时数据同步。

接下来，我们将研究一些有趣的最新科技：物联网（IoT）。Ionic 在开发者中一直很出名，因为它可以快速构建应用，通过蓝牙或 HTTP API 控制各种物联网设备。我们将重点介绍使用 BLE 和 iBeacons 协议构建应用。我们还将了解 Google 的 Physical Web 计划，该计划使用 Eddystone 协议，实现与物理物体的物理交互。此外，市场上已有多种物联网设备，我们可以轻松地将它们与 Ionic 应用对接，解决许多问题。





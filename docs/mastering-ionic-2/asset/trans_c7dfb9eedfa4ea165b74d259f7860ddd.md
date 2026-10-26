

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


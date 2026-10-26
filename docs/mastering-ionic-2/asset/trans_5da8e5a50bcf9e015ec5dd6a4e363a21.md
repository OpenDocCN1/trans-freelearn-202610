

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


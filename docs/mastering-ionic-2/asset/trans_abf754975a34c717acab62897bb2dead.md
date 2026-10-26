
```html
<link rel="manifest" href="manifest.json">
```

```json
// src/manifest.json
{
  "name": "Currency Converter PWA",
  "short_name": "Currency",
  "start_url": "index.html",
  "display": "standalone",
  "icons": [{
    "src": "assets/logo.png",
    "sizes": "512x512",
    "type": "image/png"
  }],
  "background_color": "#db0000",
  "theme_color": "#000000",
  "gcm_sender_id": "124917710568"
}
```

-   fullscreen：使用所有可用的用户区域，且不显示用户代理Chrome。
-   display：定义开发者偏好的Web应用显示模式。
-   start_url：指定用户从设备启动应用时加载的URL。在我们的Ionic应用中，它始终是index.html。
-   short_name：用于没有足够空间显示Web应用完整名称的地方。
-   name：这是用户打开应用时在启动页（splashscreen）上看到的名称。
-   Standalone：应用在视觉和体验上都将像一个独立应用。这可以包括应用拥有不同的窗口、在应用启动器中拥有自己的图标等。
-   minimal-ui：应用在视觉和体验上像一个独立应用，但会有一组用于控制导航的最小UI元素。这些元素因浏览器而异。
-   Browser：应用在常规浏览器标签页或新窗口中打开，具体取决于浏览器和平台。这是默认情况。
-   Icons：用于应用各种不同位置的图标数组，例如，当我们将其添加到主屏幕时，该图标会与应用短名称一起放置。
-   background_color：Web应用的预期背景颜色。
-   theme_color：应用的默认主题颜色。
-   gcm_sender_id：用于发送Web推送通知，代表Firebase Cloud中的sender_id：

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_360_0.png)

你可以看到，在实现了上述部分属性后，安装应用时启动画面的外观。

## 服务工作线程

在Ionic 3中使用服务工作线程非常简单，Ionic自带`service-worker.js`文件，该文件最初在`index.html`文件中被注释掉。我们只需取消注释该部分，你就会看到服务工作线程在浏览器中注册。

服务工作线程本质上充当代理服务器，位于Web应用与浏览器之间，以及浏览器与网络之间（当可用时）。它们旨在（除其他功能外）实现有效的离线体验、拦截网络请求，并根据网络是否可用以及服务器上是否有更新的资源来采取适当的行动。它们还允许访问推送通知和后台同步API。

服务工作线程具有生命周期和各种操作的事件。我们首先在页面上注册服务工作线程，成功的注册生命周期从install和activate事件开始。Ionic 3在`src`文件夹内的`index.html`文件中带有内置注册代码，只需取消注释即可。现在，你可以访问[chrome://serviceworker-internals/](chrome://serviceworker-internals/)并查看你的站点，来检查服务工作线程是否已启用：

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_362_0.png)

正如你所看到的，我们在`localhost:8100`本地提供的应用已注册服务工作线程。不过，当我们部署PWA时，它必须通过安全方式提供服务（HTTPS且无证书错误）。Firebase托管和GitHub Pages通过HTTPS托管，因此它们可以作为托管PWA应用的最佳场所。

注册完成后，接下来就是安装事件以开始其工作。在install事件中，我们缓存所有静态文件。`event.waitUntil()`方法确保服务工作线程在其中的代码执行完成之前不会被安装。现在，`cache.addAll()`将静态资源缓存到缓存存储中：

```javascript
var cacheName = 'cache-v1';
// pre-cache our key assets
var files = [
  './build/main.js',
```

```javascript
caches.match(request).then((response) => {
    if (response) {
        return response;
    }

    //if request is not cached, add it to cache
    return fetch(request).then((response) => {
        var responseToCache = response.clone();
        caches.open(cacheName).then((cache) => {
            cache.put(request, responseToCache).catch((err) => {
                console.warn(request.url + ': ' + err.message);
            });
        });

        return response;
    })
});
```

> *在sw-toolbox支持之后，我们使用sw-toolbox路由方法来获取资源，并根据可用的不同策略对其进行缓存，我们将在演示中进一步看到。*

服务工作线程可以执行推送事件、创建用户通知、打开特定页面或运行一些后台脚本：

```javascript
self.addEventListener('push', (event) => {
console.log('Received a push message', event);

const title = 'Yay a message.';
const body = 'We have received a push notification.';
const icon = 'assets/icon.png';
const tag = 'simple-push-demo-notification-tag';

event.waitUntil(
    self.registration.showNotification(title, {
        body: body,
        icon: icon,
        tag: tag
    })
);
});
```

最近，Ionic增加了对sw-toolbox的支持，这是一个`service-worker`库，它减少了很多工作量。

### 服务工作线程工具箱（sw-toolbox）

它现在已内置到starter和conference应用中。这允许你使用高级API而不是原始Service Worker API来定制你的服务工作线程。我们的开箱即用配置将为你的应用提供良好的、不依赖网络的体验，但你可以轻松定制以适应应用独特的用例。

我们已经介绍了服务工作线程的各种事件及其工作原理。这里最重要的是使用Chrome开发者工具来调试服务工作线程，并查看服务工作线程的状态和可用缓存。如需进一步研究服务工作线程以及可以使用它们做什么，你可以查看Google和Mozilla提供的以下文档链接：

https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers
https://developers.google.com/web/fundamentals/getting-started/primers/service-workers.

```javascript
localStorage.setItem('anonKey', subKey);
});
})
.catch(err => console.log('Error', err));
}
</script>
```

你可以在这里看到，成功注册后，我们从pushManager订阅推送通知。这将生成订阅端点，用于向所有用户发送通知。下一步是设置我们的`manifest.json`文件：

```json
{
  "name": "Currency Converter PWA",
  "short_name": "Currency",
  "start_url": "index.html",
  "display": "standalone",
  "icons": [{
    "src": "assets/logo.png",
    "sizes": "512x512",
    "type": "image/png"
  }],
  "background_color": "#db0000",
  "theme_color": "#000000",
  "gcm_sender_id": "124917710568"
}
```

我们在上一节中讨论了所有属性，这里我们将为用户通知添加`gcm_sender_id`。你可以从云消息中的**Firebase设置**获取`gcm_sender_id`。从那里复制服务器密钥，因为稍后发送Web推送通知时会需要它。

服务工作线程对于渐进式Web应用至关重要。以下是我们默认的`service-worker.js`，我们根据需求进行了一些进一步修改。Ionic默认不使用`sw-toolbox`库，但该库大大减少了代码和工作量：

```javascript
// src/service-worker.js

'use strict';
importScripts('./build/sw-toolbox.js');

var version = 1;

self.toolbox.options.debug = true;

self.toolbox.options.cache = {
  name: 'ionic-cache' + version
};

// pre-cache our key assets
self.toolbox.precache(
  [
  './build/main.js',
  './build/main.css',
  './build/polyfills.js',
  'index.html',
  'manifest.json',
  '/'
  ]
);
```

我们从设置缓存名称开始，使用指定的版本号命名。因此，只要有新更改，我们就会更改版本，并生成新的`cacheStorage`。接下来，我们使用`precache()`方法缓存所有静态资源，并传入所有静态文件路径。你会看到，我们添加了另一个默认文件中没有的资源，因为我们的应用在浏览器`localhost:8100`上运行，如果首次加载后离线，该资源将不会被缓存，你会看到错误。如果你不想添加这个，只需刷新页面两次，因为这样可以将它缓存。

# Ionic 与物联网

开发者已经开始认识到物联网（IoT）的巨大潜力。市场上物联网设备种类繁多，同时也需要相应的应用程序来远程控制这些设备。此时，Ionic 便应运而生，帮助开发者快速且低成本地创建应用。Ionic 在构建这类应用时非常高效，同时还提供对设备功能的完整原生支持，例如通过蓝牙向物联网设备发送信号。目前已有多种设备通过蓝牙或 HTTP API 进行数据同步和执行各种操作。当前有许多令人兴奋的项目正在进行中，我们将尝试深入了解它们。下面来看看使用 Ionic 2 构建你的下一个物联网应用的一些理由：

- 使用 Ionic 2，我们可以通过单一代码库构建 Web 应用、Android、iOS 以及不久的将来的桌面应用。尤其对于原型开发而言，这非常有用。
- Ionic 应用使用 Web 技术构建，因此上手非常快。
- Ionic 自带多套工具，因此你可以高效地引导应用启动并轻松部署。

## Ionic + Node = 强力组合

Node.js 正在解决许多硬件和物联网设备的问题。我们可以用很多方式使用 Node.js 构建后端部分，并通过 HTTP API 与 Ionic 应用通信。其中一个例子是使用 Ionic、Node.js、MQTT 和 Wi-Fi 模块，通过移动应用在互联网上通过 Wi-Fi 模块控制继电器。我们使用免费的在线代理 MQTT 将消息从 Node.js 服务器发送到 Wi-Fi 模块，该模块在 Arduino IDE 中设置。另一方面，我们使用 HTTP REST API 从 Ionic 应用调用 Node.js 服务器。这样，我们就能够向 Wi-Fi 路由器发送信号并执行特定操作。

用户需要某种界面来与这些物联网设备交互，这为基于 Ionic 的移动应用开辟了道路。我们将在本章后面实现一个名为 bleno 的节点模块，用于实现 BLE 外设。Noble 和 bleno 是用于 BLE 实现的两个常用模块。Noble 扫描其附近的周边外设，bleno 作为外设进行传输。此外，还有其他多个物联网模块可作为 NPM 包使用，能让你快速上手：

- **Cylon.js**：主要用于机器人和物联网
- **Node-RED**：用于将多个物联网硬件设备连接在一起的工具
- **DeviceJS**：面向物联网的 JavaScript 引擎，物联网领域的 jQuery
- **noduino**：用于控制 Arduino 的 Node.js 框架

WebSocket 可以像 MQTT 一样成为另一种消息传递协议。我们将在本章的演示中使用 WebSocket。我们可以做的事情是无穷无尽的。Arduino 和 Raspberry Pi 是用于构建可与其他硬件设备交互的应用的两个著名开发板。你可以在网上找到许多在黑客马拉松或学习期间构建的示例，其中开发者使用 Ionic 应用实时点亮 LED。其中一个示例使用 Raspberry Pi GPIO 引脚和连接到它上面的面包板。Node.js 代码驻留在 Raspberry Pi 上，LED 灯泡连接到面包板。我们可以想到的另一个示例是连接温度、GPS 等传感器。你可以在 https://github.com/arjunsk/Ionic-IoT-Monitor 找到一个有趣的 Ionic 应用示例，它使用 MQTT 实时从传感器获取数据并在应用中可视化。

可能性是无限的，随着物联网行业的发展，你会发现更多传感器和硬件设备不断涌现，以解决许多现实问题。你甚至可以考虑控制所有家庭设备并构建一个家庭自动化项目，类似于马克·扎克伯格几个月前用他的 Jarvis AI 所做的。尽管他在项目中使用 AI，但我们完全可以简单地根据我们的需求开始控制所有智能设备。作为开发者，最好的部分是，即使你只写一小段让你的生活更轻松的代码，你也会非常喜欢它。

## BLE、信标和物理 Web

BLE 代表低功耗蓝牙（Bluetooth Low Energy）。蓝牙 4.0 为物联网行业带来了创新性的变革。曾几何时，蓝牙被视为交换文件的一种媒介，但随着技术的发展，我们已经看到健身手环和智能手表市场的增长。蓝牙智能设备功耗低，有助于延长智能手机电池寿命，并被证明对物联网行业非常有效。随着蓝牙再次渗透到市场，我们看到物联网行业中对蓝牙设备的使用率很高。

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_331_0.png)

这是我在一次黑客马拉松期间构建的应用截图，它使用 Kontakt 信标广播 iBeacon，而我们基于 Ionic 的移动应用检测到该信标并触发本地通知。

我们一直在谈论很多关于信标的内容。让我们来看看 iBeacon 是什么。iBeacon 是由 Apple 开发的一种协议，它设定了如何传输数据信号的布局。通常，BLE 广播有两种协议：iBeacon 和 Eddystone。标准的 iBeacon 广播包含 UUID、major 和 minor：

```
UUID: f7826da6-4fa2-4e98-8024-bc5b71e0893e

Major ID: 65279

Minor ID: 63150
```

## Eddystone 与物理 Web

Eddystone 是由 Google 发布的 BLE 信标协议，它是开源的且跨平台。Eddystone 包含三种数据类型帧：Eddystone-UID、Eddystone-URL 和 Eddystone-TLM：

- **Eddystone-UID**：一个唯一的 16 字节静态 ID，通常被原生应用使用
- **Eddystone-URL**：一个物理 Web 项目，传输可直接供客户端使用的压缩 URL
- **Eddystone-TLM**：广播信标信息，如电池电量、传感器数据和其他信息

现在几乎所有主要的信标制造商都支持 iBeacon 和 Eddystone 数据格式。Google 目前正在推广信标技术，以提供更好的定位和邻近体验。目前，Eddystone-URL 帧被广泛用于物理 Web 计划中。

*物理 Web 是一种开放方法，旨在实现对物理对象和位置的快速无缝交互。*

物理 Web 将 URL 的力量扩展到日常物理对象上。这使你无需下载特定于设备的应用即可与任何物理设备（例如自动售货机租赁）进行交互。使用起来非常顺畅：你只需点击设备上附带的 URL，并在网络浏览器中使用它。物理 Web 是一个很小的发现服务，它提供附近的移动设备可以接收的特定 URL。这将带你进入一个完全交互式的网页，或深层链接到原生应用。

我们已经面临着安装多个应用会占用设备内存和存储空间的挑战。此外，管理多个应用也很头疼，这也是开发者开始考虑将移动 Web 与 PWA 结合起来的另一个原因。同样，随着物联网设备不断增加，为每台设备安装应用并不具备可扩展性。因此，物理 Web 在这里提供了帮助，你可以打开 URL 在 Web 上操作设备，而后端可以通过多种协议与设备通信。让我们想一个现实生活中的例子。如果你正在做演讲，你可以使用信标广播幻灯片 URL，并让听众打开蓝牙和定位服务来接收该 URL。

我们也不需要真正的硬件信标设备来演示这一点或在会议中广播。有一个 NPM 包，名为 Eddystone-beacon，我们可以用它来广播 Eddystone URL：

```
$ npm install eddystone-beacon --save
$ sudo node -e "require('eddystone-beacon').advertiseUrl('https://www.facebook.com/veloice');"
```

只需确保你所使用的 Linux 笔记本电脑已启用蓝牙即可。现在启用 Android 手机上的蓝牙和定位服务，你会看到一个带有 Web URL 标题和描述的通知。





![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_334_0.png)

这里我广播了三个不同的 URL，你可以在通知菜单中看到全部三个 URL 的详细信息。这是一个关于如何在公开场合分享 URL 的小例子。你可以在 Physical Web 网站上找到很多这样的例子（https://google.github.io/physical-web/）并看看如何从这一倡议中构建出东西。

我们可以用 iBeacons 和 Eddystone 执行各种功能，例如测距、区域监控和其他设置。基于 tx 和 RSSI 值，我们可以获取设备与信标之间的近似距离。借助信标，我们甚至可以进行室内地图绘制和地理围栏。

## 基于接近度的屏幕锁定，使用 BLE

这是一直以来我想实现的东西，也是我现实生活中的一个问题。是时候研究一下使用 BLE 栈与 Ionic 应用结合是否切实可行了。在办公室工作时，我经常需要离开座位去接一些紧急电话或参加会议。大多数时候我忘记给笔记本电脑屏幕锁定。所以，我想用 BLE 和 WebSockets 构建这个基于接近度的屏幕锁定应用。

让我们先看看应用程序的工作原理。基于 Ionic 的应用程序会监听笔记本电脑持续广播的 iBeacon 数据包。当我带着手机离开座位时，在某个点上它会退出笔记本电脑的广播区域。立即，它会通过网络向运行在我 Ubuntu 笔记本上的 Node.js 应用发送一个 socket 请求，并在我的笔记本上调用屏幕锁定命令。同样地，当我重新进入广播范围时，它会发送另一个 socket 请求来解锁笔记本。

我们在这里使用的技术栈是：

- Node.js
- Web Sockets
- BLE
- Ionic
- Bash script

我们必须先准备好 Node.js 后端和 bash 脚本，才能开始我们的应用程序。在此之前，请确保已安装用于 BLE 和 Node.js 的所有 Debian 包。

```
$ sudo apt-get install bluetooth bluez libbluetooth-dev libudev-dev
```

现在我们可以使用 `npm init` 命令初始化项目：

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_337_0.png)

我们为应用程序使用以下 NPM 包：

- `app` 和 `http` - 用于创建 Web 服务器并在 5000 端口监听
- `io` - 用于 websocket 实现
- `bleno` - 用于实现 BLE 外设
- `child_process` - 用于执行 bash 文件

```javascript
$ npm install express http bleno socket.io   // index.js

var app = require('express')(),
    http = require('http').Server(app),
    io = require('socket.io')(http),
    bleno = require('bleno');
    exec = require('child_process').exec;

// Start advertising iBeacon
var uuid = '38a48d7e-05ab-11e7-93ae-92361f002671';
var major = 10; // 0x0000 - 0xffff
var minor = 5; // 0x0000 - 0xffff
var measuredPower = -59 // -128 - 127

bleno.on('stateChange', function(state) {
    console.log('on -> statechange: ' + state);
    if (state === 'poweredOn') {
        bleno.startAdvertisingIBeacon(uuid, major, minor,
            measuredPower);
    } else {
        bleno.stopAdvertising();
    }
});
```

最初，我们声明了用于广播 iBeacon 的属性。我们在这里监听 `stateChange` **listener**，这样在 **Bluetooth** 开启/关闭时，我们就可以分别使用 `startAdvertisingIBeacon()` 和 `stopAdvertising()` 方法启动或停止 **iBeacon** 广播。

现在我们准备好开始我们的 Ionic 应用以与后端交互。让我们使用 tabs starter 模板启动应用：

```
gaurav@gaurav-thinkpad:~/projects/ionic$ ionic start lock-app tabs --v2
Creating an Ionic 2.x app in /home/gaurav/projects/ionic/lock-app based on the tabs template.
Downloading: https://github.com/driftyco/ionic2-app-base/archive/master.zip
Downloading: https://github.com/driftyco/ionic2-starter-tabs/archive/master.zip
Installing npm packages (may take a minute or two)...
/ ♪ ♪ ♪ ♪  Your Ionic app is ready to go! ♪ ♪ ♪ ♪
Some helpful tips:
Run your app in the browser (great for initial development):
  ionic serve
Run on a device or simulator:
  ionic run ios[android,browser]
Share your app with testers, and test on device easily with the Ionic View companion app:
  http://view.ionic.io
gaurav@gaurav-thinkpad:~/projects/ionic$
```

由于我们已经清楚应用中将使用哪些内容，让我们安装平台、Cordova 插件和 NPM 包：

```
// add android platform
$ ionic platform add android

// add ibeacon and diagnostic plugins
$ ionic plugin add cordova-plugin-ibeacon --save
$ ionic plugin add cordova.plugins.diagnostic --save

// install NPM package for socket.io-client
$ npm install socket.io-client --save
```

我们首先通过检查蓝牙是否已启用并要求用户授予位置权限来开始，这在使用 diagnostic 插件时自 Android 6 起成为强制要求：

```typescript
// src/app/app.component.ts

import { Component } from '@angular/core';
import { Platform } from 'ionic-angular';
import { IBeacon, Diagnostic, StatusBar, Splashscreen } from 'ionic-native';

import { TabsPage } from '../pages/tabs/tabs';

declare var cordova: any;

@Component({
  templateUrl: 'app.html'
})
export class MyApp {
  rootPage = TabsPage;

  constructor(platform: Platform) {
    platform.ready().then(() => {
      // Okay, so the platform is ready and our plugins are available.
      // Here you can do any higher level native things you might need.

      StatusBar.styleDefault();
      Splashscreen.hide();

      if (platform.is('cordova')) {

        // Checks if bluetooth is enabled
        IBeacon.isBluetoothEnabled().then((isEnabled) => {
          console.log("isEnabled: " + isEnabled);
          if (!isEnabled) {
            // If not, enableBluetooth() enables it
            IBeacon.enableBluetooth();
          }
        });

        // Check if location is enabled
        Diagnostic.isLocationEnabled().then((result) => {
          // If not, open location settings
          if (!result) {
            Diagnostic.switchToLocationSettings();
          }
        });

        // Diagnostic plugin
        // After Android 6, we need to explicitly
        // ask for permission from the user
        Diagnostic.requestRuntimePermissions([
          cordova.plugins.diagnostic.permission.ACCESS_FINE_LOCATION,
          cordova.plugins.diagnostic.permission.ACCESS_COARSE_LOCATION
        ]);
      });
  }
}
```

最初，我们检查 isBluetoothEnabled()，它会根据状态返回一个布尔值。之后，如果返回 false，则我们调用 enableBluetooth() 方法来启用蓝牙。同样，isLocationEnabled() 用于检查位置是否已启用，如果未启用，则调用 switchToLocationSettings() 方法，该方法会打开位置设置。

Diagnostic.requestRuntimePermissions() 需要获取运行时权限，这对于运行 Android 6 及以上版本的所有设备都是必需的。对于 BLE 的监控和测距，我们需要 ACCESS_FINE_LOCATION 和 ACCESS_COARSE_LOCATION 权限：

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_340_0.png)

```typescript
IBeacon.enableDebugNotifications();
}
}
manageLaptop(data, status) {
    this.socket.emit('status', status);
}
```

`startMonitoringForRegion()` 方法开始对指定区域进行监控，该区域的 `uuid`、major 和 minor 与我们的 Node.js 代码广播的一致。现在，当你离开座位并移出蓝牙范围时，`didExitRegion()` 方法将被调用，它会订阅并进一步调用我们的 `manageLaptop()` 方法，传入数据和锁定状态。同样，当我们返回时，`didEnterRegion()` 将被调用：

```typescript
initialize() {
    this.socket = io.connect(this.socketHost);

    this.socket.on("connect", (msg) => {
        console.log('on connect');
    });

    this.socket.on("reconnecting", (msg) => {
        console.log('on reconnecting');
    });

    this.socket.on('disconnect', function() {
        console.log('user disconnected');
    });

    this.socket.on("result", (msg) => {
        this.laptop_status = msg;
        console.log(msg, "==received=msg===")
    });
}
```

在我们的 `manageLaptop()` 方法内部，我们向服务器发出自定义事件。在此之前，我们还初始化了 `initialize()` 函数，以在后端 Node.js 应用运行的主机上与服务器建立初始连接。

这样我们的整个演示就完成了。对我个人而言，它运行得非常好，并且提供了手动锁定和解锁按钮，我也可以远程锁定我的笔记本电脑。你可以用 Ionic 和 IoT 快速构建许多小技巧和原型。





## Summary

信标及其应用即将在多个领域出现，尤其是在零售领域。然而，就广泛实施而言，它仍然是一种非常新的技术。零售商应尝试思考新的创新，利用新技术来提升用户体验。我们已经讨论并快速实现了许多关于如何将物联网与 Ionic 应用结合使用的用例。我一直定期关注 Ionic 博客，看到许多团队使用 Ionic 来与他们的硬件设备进行交互，其中之一就是 jewelbots，它可以在朋友靠近时启用或向你发送消息。机会是巨大的，只需要一个想法来实现它并将其带到世界。

我们几乎接近旅程的尾声，但仍有一个有趣的话题等待在下一节中介绍。我们在本章中讨论了为访问设备而安装多个原生应用程序的问题，而这个问题正是通过为 Ionic 引入 PWA 支持而得到解决的。PWA 即使在低网速下也让用户体验变得沉浸式。印度许多电子商务和大型科技公司在 Flipkart 最初与 Chrome 团队合作推出其渐进式网络应用后，也推出了自己的渐进式网络应用。突然之间，我们可以看到市场正在增长，开发者正在构建平台以使当前网络应用能够启用 PWA 功能。尽管如此，它并不会终结原生应用市场，因为我们没有所有插件的能力。使用 Ionic 的最好之处在于，完全相同的代码可以同时转换为 PWA 和原生应用。

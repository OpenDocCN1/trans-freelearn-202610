

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


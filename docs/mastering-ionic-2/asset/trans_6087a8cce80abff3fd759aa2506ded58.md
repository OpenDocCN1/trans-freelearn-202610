
## Cordova 与插件的工作原理

Cordova 是一个使用 HTML、CSS 和 JavaScript 在单一代码库中构建原生移动应用的平台。Cordova 提供对多个平台和插件的支持。Cordova 插件允许 Cordova 的 Web 视图与原生平台通信，而我们的 Ionic 应用运行在 Web 视图中，因此我们可以访问平台 API，如相机、地理位置等。

### 什么是 Cordova？

关于 Cordova，官方描述如下：

> *Apache Cordova 是由 Apache 组织维护的一个项目。Cordova 是一组设备 API，允许移动应用开发者通过 JavaScript 访问相机或加速度计等原生设备功能。结合 Ionic 等框架，只需使用 HTML、CSS 和 JavaScript 即可开发智能手机应用。*

我们都可能在自己的应用中使用过 Cordova，也了解一些 Cordova 官方支持的常见插件。下面我将列出一些有助于理解 Cordova 的常见要点：

- Cordova 是一个应用容器
- Cordova 帮助为每个平台构建原生应用
- Cordova 提供对原生设备 API 的访问，例如相机、通讯录、GPS 等
- 还可以编写和支持自定义插件
- Cordova 是 Adobe PhoneGap 背后的引擎
- Cordova 开源且免费使用

如前所述，Cordova 官方支持多个平台和插件。我们可以看到许多新插件正在涌现，例如用于指纹、热点等功能的插件，我们可以利用它们来构建高级混合应用。大多数第三方插件都支持 iOS 和 Android 平台，因为这两个平台在设备市场中占据主要份额。最近，一些第三方 Cordova 插件也开始支持诸如蓝牙信标等新兴技术。在本书的后续内容中，我们将构建一个应用程序并使用这项技术。

### 构建和使用 Cordova 插件

开发者使用 CLI 命令在项目中构建或添加 Cordova 插件。你也可以通过 plugman 发布自己的插件并与社区共享。我们还将快速了解如何构建和发布插件。

```
<?xml version="1.0" encoding="utf-8"?>
<plugin xmlns="http://www.phonegap.com/ns/plugins/1.0" id="cordova-eddyblue" version="1.0.0">

<name>Custom Plugin</name>
<description> Cordova custom bluetooth plugin 
</description>
<author>Gaurav Saini</author>
<license>Apache 2.0</license>

<keywords>Eddystone, Beacons</keywords>


<engines>
<engine name="cordova" version=">=3.4.0" /> <!-- Needs cordova version greater than 3.4.0 -->
</engines>

<!-- Javascript module and file path -->
<js-module src="www/eddyblue.js" name="eddyblue">
<clobbers target="eddyblue" />
</js-module>

<!-- android platform -->
<platform name="android">

<config-file target="res/xml/config.xml" parent="/*">
<feature name="Scan">
<param name="android-package" 
value="com.eddyblue.plugin.scan"/>
</feature>
</config-file>

<source-file src="src/android/Eddyblue.java" target-dir="src/com/eddyblue/plugin/"/>
</platform>

<!-- ios platform -->
<platform name="ios">

<config-file target="config.xml" parent="/widget">
<feature name="Scan">
<param name="ios-package" value="HWPEddyblue" 
/>
</feature>
</config-file>

<header-file src="src/ios/HWPEddyblue.h" target-dir="Eddyblue"/>
<source-file src="src/ios/HWPEddyblue.m" target-dir="Eddyblue"/>
</platform>

</plugin>
```

```
module.exports = {
  scan: function (name, successCallback, errorCallback) {
    cordova.exec(successCallback, errorCallback, "Scan", "start", [name]);
  }
};
```

```
package com.eddyblue.plugin;

import org.apache.cordova.*;
import org.json.JSONArray;
import org.json.JSONException;

public class Scan extends CordovaPlugin {

 @Override
 public boolean execute(String action, JSONArray data, CallbackContext callbackContext) throws JSONException {

 if (action.equals("start")) {
 String message = "data to be sent";
 callbackContext.success(message);
 return true;
 } else {
 return false;
 }
 }
}
```

我们现在已经简要介绍了插件的工作原理。你现在可以轻松地与社区共享你的插件，并使用 plugman 将其添加到注册表中。有关插件开发的更多细节，你可以参阅 Cordova 官方文档，它会提供进一步的指导。

## Ionic Native

开发混合应用的开发者已经在 Ionic 1 中使用 ngCordova 一段时间了。现在，随着使用 ES6 和 TypeScript 构建的 Ionic 3 的出现，我们需要一个面向 ES6 或 TypeScript 的包装库，这正是 Ionic Native 的作用。它是基于 Cordova API 构建的 ES5/ES6/TypeScript 包装器集合，便于集成到 Ionic 应用中。Ionic 团队最初是应社区成员的功能请求而启动这个项目的。它最初只是一个实验，但由于插件集成时常常面临文档缺失和 API 混乱的问题，它获得了大量支持。这些 AngularJS 包装器简化了集成过程，并且也符合 Angular 的语法。目前，Ionic Native 支持大约 120+ 个 Cordova 插件，而且数量还在不断增加。

### 安装 Ionic Native

我们可以将 Ionic Native 添加到任何现有的 Ionic 项目中。在任何使用 Ionic 3 创建的入门项目中，你都会默认找到 `ionic-native`。如果由于某种原因需要安装它，推荐的方式是通过 NPM 包：**$ npm install @ionic-native/core --save**

如果你使用 ES6/TypeScript 构建 Ionic 3 应用，则无需在 `index.html` 文件中包含任何脚本标签。只需在文件中导入特定的 `ionic-native` 包即可：

```typescript
import { SplashScreen } from '@ionic-native/splash-screen'
import { StatusBar } from '@ionic-native/status-bar';
```

如果你使用的是 Ionic 1，则需要在 `index.html` 文件中添加 `ionic.native.js` 文件：

```html
<script src="ionic.native.js"></script>
```

我们必须将大多数插件调用包装在设备就绪事件中。因此，要检查设备是否已完全加载应用且插件可用，请使用 `Splashscreen` 插件的以下代码：

```typescript
constructor(platform: Platform, private
  splashScreen: SplashScreen, statusBar: StatusBar)
  {
    this.platform.ready().then(() => {
      // platform is ready to use available plugins.
      this.statusBar.styleDefault();
      this.splashScreen.hide();
    });
  }
```

### 在浏览器中测试插件

目前 Cordova 插件最大的问题之一是我们无法在浏览器中测试应用。Ionic 团队一直专注于改进 Ionic 工具，以便我们可以直接在浏览器中模拟应用的大部分功能。不过，对于当前的开发，有一个非常好的 Chrome 插件——Cordova mocks，它由 Paolo Bernasconi 创建，他是之前 ngCordova 项目的核心贡献者之一。安装该插件后，你就可以像在手机上测试一样开始测试应用。目前，此扩展支持大约十二个插件。这个 Chrome 插件在某些情况下可能很有效，可以帮助你在浏览器中测试受支持的插件。

### 常用的 Cordova 插件

每当你启动一个新的 Ionic 项目并添加平台时，你会看到有一些默认安装的插件。当你添加 Android 或 iOS 平台时，Ionic 会安装以下提到的插件：

- ionic-plugin-keyboard
- cordova-plugin-console
- cordova-plugin-device
- cordova-plugin-splashscreen
- cordova-plugin-whitelist
- cordova-plugin-statusbar

让我们开始了解上述插件及其用途吧。


# Installing Ionic Native

We can add Ionic Native to any existing Ionic project. You will find `ionic-native` by default in any starter project you will start with Ionic 3. If you somehow have to install it then the recommended way is via the NPM package: **$ npm install @ionic-native/core --save**

If you are using ES6/TypeScript for building Ionic 3 apps, you do not need any scripts tags to be included in the `index.html` file. Just import the specific `ionic-native` package inside your file:

```
```typescript
import { SplashScreen } from '@ionic-native/splash-screen'
import { StatusBar } from '@ionic-native/status-bar';
```
```

If you are using Ionic 1 then you need to add the `ionic.native.js` file inside the `index.html` file:

```
```html
<script src="ionic.native.js"></script>
```
```

We have to wrap most of the plugin calls with the device ready event, so to check if your device has fully loaded the application and plugins are available, use the following code for the `Splashscreen` plugin:

```
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
```
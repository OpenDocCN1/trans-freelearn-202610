# Building TasteBite and installing Firebase

Let's first start by creating an application from scratch and install all the plugins and packages required in this application. In this application we will be using the Tabs template and integrating Lazy Loading. We will be using geolocation, camera, and in-app browser plugins in our application:

![](bbox=[0.174, 0.156, 0.825, 0.449])

```
$ ionic start tastebite tabs
$ ionic cordova:platform add android
$ ionic cordova:plugin add cordova-plugin-inappbrowser
$ ionic cordova:plugin add cordova-plugin-geolocation
$ ionic cordova:plugin add cordova-plugin-camera
$ npm install --save @ionic-native/in-app-browser
$ npm install --save @ionic-native/geolocation
$ npm install --save @ionic-native/camera
```

Now, we are ready to setup Firebase in our Ionic application:

```
$ npm install angularfire2 firebase -save
```

Next we will import the angularfire2 library in app.module.ts:

```
// src/app/app.module.ts

import { AngularFireModule } from 'angularfire2';

export const firebaseConfig = {
  apiKey: "AIzaSyDnd6hMcfJDWG dGr6EOS S05NoMM2CeY2bw",
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
```
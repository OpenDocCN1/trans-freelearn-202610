
```
provide: TranslateLoader,
useFactory: createTranslateLoader,
deps: [Http]
})
],
providers: [
{ provide: ErrorHandler, useClass: IonicErrorHandler},
{ provide: Storage, useFactory: provideStorage },
{ provide: APP_BASE_HREF, useValue: '/'},
UserService,
DataService
]

// Deeplink Configuration
export const deepLinkConfig: DeepLinkConfig = {
links: [
{ component: HomePage, name: 'Home Page', segment: '' },
{ component: CategoriesPage, name: 'Categories Page', segment: 'categories' },
{ component: ProductsPage, name: 'Categories Product Page', segment: 'categories/:categoryId' },
{ component: ProductDetailPage, name: 'Product Details Page', segment: 'products/:productId' },
{ component: WishlistPage, name: 'Wishlist Page', segment: 'wishlist' },
{ component: ShowcartPage, name: 'Showcart Page', segment: 'cart' },
{ component: SupportPage, name: 'Support Page', segment: 'feedback' },
{ component: SettingsPage, name: 'About Page', segment: 'settings' },
{ component: AboutPage, name: 'About Page', segment: 'about' },
{ component: LoginPage, name: 'Login Page', segment: 'login' },
{ component: SignupPage, name: 'Signup Page', segment: 'signup' },
{ component: AccountPage, name: 'Account Page', segment: 'account' }
]
};
```

Now, similar to how you open a specific link with a unique URL, you can go to a specific view with a custom URL scheme. So now our application can be accessed at vplanet://wishlist, which will now open the application's wishlistPage directly.

## Facebook connect

The Facebook plugin is one of the most loved and widely used plugins for social integrations. With Ionic Native and improved documentation, now it's really easy to integrate the Facebook connect plugin inside your application. The Facebook plugin allows you to use the same JavaScript code in your Cordova application as you use in your web application. The best part about this plugin is it uses the Native Facebook app to perform Single Sign On for the user. If the Facebook Native app is not installed, then the sign on will degrade gracefully using the standard dialog based authentication. This plugin just has support for iOS and Android for now. Before installing this plugin you need to create a Facebook app via developers.facebook.com and then copy the appId and appName, which will be required while adding the plugin:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_203_0.png)

We are now ready to install the plugin for Facebook, which requires APP_ID and APP_NAME:

```
$ ionic cordova:plugin add cordova-plugin-facebook4 --variable APP_ID="419376571536241" --variable APP_NAME="chatApp"
```

Other than this, there are a set of requirement checks that need to be done for specific platforms. For Android you need to configure the project with your fb_app_id in the res/values/facebookconnect.xml file:

```
<resources>
    <string name="fb_app_id">419376571536241</string>
    <string name="fb_app_name">chatApp</string>
</resources>
```

For iOS, you have to change the following:

- Change FacebookAppID to project Info.plist
- Change the URL scheme to fb<YOUR APPID>: for example, fb 419376571536241

You are now ready to use Facebook plugin methods that the $cordovaFacebook module provides. The following are some of the methods that we have with Ionic Native:

- login(permissions): A string of arrays, such as ['public_profile', 'email']
- showDialog(options): A JSON object with three keys; method, link, and caption
- api(path, permissions): A Facebook API path query, for example, api("me", ["public_profile"])
- getLoginStatus(): Checks if a user is logged in or not
- getAccessToken(): Fetches the access token of the current logged in session
- logout(): Logs out from Facebook

For testing in a browser, you still need to make some changes in the application config:

```
let appID = 419376571536241;
let version = "v2.0"; // or leave blank and default is v2.0

constructor() {
  Facebook.browserInit(appID, version);
}
```

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_204_0.png)

Also, you have to set the redirect URL, which will look as follows. As Ionic servers run on port 8100, you can give http://localhost:8100 as a redirect URL:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_204_1.png)

## Building a music player

We will now build a media player application, where you will have audio and video streaming and download options to save the file to your mobile. We will be mainly integrating a media plugin for audio streaming via `ionic-audio` package, a `StreamingMedia` plugin will be used for playing the videos, and a `FileTransfer` plugin for downloading the files to a local system directory. Let's first create an application

```
gaurav@gaurav-thinkpad:~/projects$ ionic start jingle-app tabs --v2
Creating an Ionic 2.x app in /home/gaurav/projects/jingle-app based on the tabs template.

Downloading: https://github.com/driftyco/ionic2-app-base/archive/master.zip
Downloading: https://github.com/driftyco/ionic2-starter-tabs/archive/master.zip
Installing npm packages (may take a minute or two)...

♫♫♫♫ Your Ionic app is ready to go! ♫♫♫♫♫

Some helpful tips:

Run your app in the browser (great for initial development):
  ionic serve

Run on a device or simulator:
  ionic run [android,browser]

Share your app with testers, and test on device easily with the Ionic View companion app:
  http://view.ionic.io
gaurav@gaurav-thinkpad:~/projects$
```

for which we will be using the tabs template:

We just now need to add the **ionic platform add android** platform and we will be ready to integrate the plugins and build our jingle application.

## Media, streaming, and transfer plugin

We will be starting with a simple music player application that will demonstrate the usage of different plugins, but the possibilities here are endless. You might think of building a music recommendation application that will play music according to your inputs, mood, or other factors, although you will need a good backend application ready for this to support recommendations. As we have decided on the major functionality that our application will have, we will now need mainly three Cordova plugins, which we will install now:

```
$ ionic cordova:plugin add cordova-plugin-media
$ ionic cordova:plugin add cordova-plugin-file-transfer
$ ionic cordova:plugin add cordova-plugin-streaming-media
```

The next step will be to install their respective NPM packages and ionic-audio module, which we will use for the audio player:

```
$ npm install @ionic-native/media @ionic-native/filetransfer @ionic-native/streaming-media ionic-audio --save
```

Let's integrate the ionic-audio module, this module automatically switches between WebAudioProvider and CordovaMediaProvider according to the environment, which is really helpful while testing the application, as we can easily verify the application inside the browser itself:

```
// src/app/app.module.ts
import { IonicAudioModule, AudioProvider, WebAudioProvider, audioProviderFactory } from 'ionic-audio';
import { NgModule, ErrorHandler, CUSTOM_ELEMENTS_SCHEMA } from '@angular/core';
import { IonicApp, IonicErrorHandler, IonicModule } from 'ionic-angular';

@NgModule({
  imports: [
    IonicModule.forRoot(MyApp),
    IonicAudioModule.forRoot({ provide: AudioProvider,
      useFactory: audioProviderFactory })
  ],
  schemas: [ CUSTOM_ELEMENTS_SCHEMA ]
})
```

We have three tab views, HomePage, MusicPage, and VideoPage. Audio Player is integrated on the MusicPlayerPage. We will inject AudioProvider for controlling the audio player and its functions:

```
// app/pages/music-player/music-player

import { AudioProvider } from 'ionic-audio';

export class MusicPlayerPage {

  source: string;
  img: string;
  singleTrack: any;
  allTracks: any[];
  selectedTrack: number;

  constructor(
    public platform: Platform,
    public navParams: NavParams,
    public viewCtrl: ViewController,
    private _audioProvider: AudioProvider) {
```

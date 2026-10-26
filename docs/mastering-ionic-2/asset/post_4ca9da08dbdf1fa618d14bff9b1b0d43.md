
## Summary

When it comes to securing your application, the most important thing is to properly secure your server-side APIs. This way we will not lose any critical data and others' data will not get shared publicly. Although, when we think about securing our code base, nothing is 100% secure in native as well as hybrid mobile development. The goal should be to get as close to that number as possible though. Start by limiting your exposure and taking care that no sensitive data and logic should be on the client side. You should make sure you are doing your best to secure your hybrid mobile apps. However, if someone is dedicated enough, they can still work around it.

We have now covered some important concepts of security inside and outside of your Ionic application. We will now move one step ahead with integrations of our Ionic app with BaaS services such as Firebase and Ionic Cloud. In case you have very simple requirements for your Ionic application and don't have much server logic, you can quickly build an Ionic application with these services without setting up any server-side code base. We will be building a **TasteBite** application where we will be rating food items and restaurants combined, so that users can know what the famous dishes in their cities are and where they can find them cooked best.

## Introduction to Firebase and TasteBite apps

Getting started with Firebase is really simple; that is the main reason many startups and growing products use all these Backend as a Service platforms like Firebase. We will quickly start by creating a TasteBite application with Firebase and Ionic. In this application we integrated an AngularFire2 wrapper for Angular applications using Firebase, although, we can simply use the JavaScript SDK also inside our application. You need to create an account at Firebase (initially you can have a free account; as your consumption increases you can upgrade your account easily).

Firebase has made the life of developers easier as they don't have to spend time building a backend server to build simple APIs. We can easily do all CRUD operations with minimum effort and with WebSocket support, it's super fast in syncing data. Also, Firebase offers offline sync, user authentication, and features for securing the data with sets of rules:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_297_0.png)

As soon as you create an account, you create an app inside Firebase; you will find an option to add a project. After you add/create a project, you can now manage and enter your applications. You will see many options such as Analytics, Authentication, Database, Storage, Hosting, Notification and many other options. We will use many of them in our application as we move forward creating data for our application. You can already see in the following screenshot that we have some data stored in our application. All Firebase database data is stored as JSON objects and no relational tables exist.

### Building TasteBite and installing Firebase

Let's first start by creating an application from scratch and install all the plugins and packages required in this application. In this application we will be using the Tabs template and integrating Lazy Loading. We will be using geolocation, camera, and in-app browser plugins in our application:

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

Now, we are ready to set up Firebase in our Ionic application:

```
$ npm install angularfire2 firebase --save
```

Next we will import the angularfire2 library in app.module.ts:

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

Make sure you include `firebaseConfig` as shown in the previous example, which will have multiple configuration parameters. Also, the next step will be to inject `firebaseConfig` while initializing our Firebase module. We are now ready to read and write data inside our application.

### Authentication and security using Firebase

We will now look into Firebase user authentication and how we can secure our data. Firebase supports login and authentication internally and also provides support for integrating with social login providers. Most applications need some kind of login mechanism internally; with Firebase it's super easy to set this up. Whenever we log in using Firebase, a session is created and a unique ID, `uid`, is returned by Firebase, which is distinct across all providers and never changes for a specific authenticated user. This will also help us later in securing our data.

Once the user is authenticated, its session is managed and remembered even after the application is restarted. Firebase provides user authentication for multiple platforms:

- **Custom** - for integrating with existing authentication systems
- **E-mail** - register and authenticate users by e-mail and password.
- **Anonymous** - for guest accounts without any personal information required
- Facebook
- Google
- Twitter
- GitHub

### Enabling providers

We need to enable the auth provider from our application's Authentication Sign-In Method tab. We have enabled e-mail-based login and Facebook login for our TasteBite app. It's really easy to sign up, log in and log out users with e-mail-based login. We can use custom auth if we already have some authentication systems; if we want to have guest login, we can use anonymous user authentication. We will demonstrate how easily we can integrate social logins also.

All social logins require a client ID and client secret for the application created on social channels. All social providers have different ways of creating an application, and after creating an application we can put the client ID and client secret inside our Firebase app. We need to make sure that we have placed the OAuth redirect URL inside the Facebook Login product: https://tastebite-ec5c4.firebaseapp.com/__/auth/handler. Before this, you have to create an application on Facebook whose app_id and app_secret we will add back to our Firebase Facebook provider settings. After this, your backend work is done, and you just have to call a function from the Ionic application.

We will now generate AuthServiceProvider which we will use for AngularFire2 authentication:

```
$ ionic g provider AuthService
```

Before we start working on this provider, we need to set up FirebaseAuthConfig inside our app.module.ts file:

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

This way we don't need to pass this config to the AngularFireAuth-based login function. Although we can override this configuration and use any other provider by passing a login method internally. In the code above you can see AuthMethods is selected as Redirect, but for testing on browsers you can change it to Popup.

Let's look at our AuthService now:

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

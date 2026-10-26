
related to users should be accessed by that user. To-do items created by one user login should not have permission to other users. So basically it should be filtered according to current users. For this we use custom permissions, for example, while inserting an item we specified the `userId` to which the item is associated:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_235_0.png)

So now while fetching the items we will pass the current `userId`, which will retrieve data in the current user context:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_235_1.png)

```javascript
todoItems.findAll({user:this.user.id}).watch().subscribe( (items) => {
  // this will only fetch items that
  // login user have created
  console.log(items);
})
```

We have tried here to touch each possible section available with IonicDB and get with some sample code. This will really ease the initial learning curve and get you directly started on building the applications using IonicDB. With IonicDB we can also integrate that in our backend NodeJS application.

## Ionic push

Ionic push allows you to send messages and alerts to a user's device. Push notifications can be triggered even when application is closed. Push notifications have now been around for a long time and are commonly used in every application. Many of you probably already have good experience with integrating GCM and APNS in the past. So here our main interest is how we can use these services from the Ionic push service. Ionic push allows you to send notifications to both platforms from a single interface or API. With the Ionic push service we can send notifications to:

- Target users
- Schedule notifications
- Send contextual content
- Attach media to notifications
- Automatically send notifications on custom logic
- Trigger app events with background notifications

Before we can send notifications from the Ionic push service we need to set up FCM Project and API keys. We can find the FCM server key and sender ID in the firebase console settings and cloud messaging tab:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_237_0.png)

- The next step is to generate the keystore file, which we will be uploading with other details while creating the security profile for our application. While creating keystore it will first ask for the Keystore Password and after entering other details it will ask for the Key Password for alias. Just make sure you remember this as that will be needed while creating security profiles:

```
keytool -genkey -v -keystore vplanet.keystore -alias vplanet-alias -keyalg RSA -keysize 2048 -validity 10000
```

- Now we have everything ready to create a security profile. The FCM server Key from the Firebase console, attach the generated vplanet.keystore file, alias, and their respective passwords:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_238_0.png)

- We will be using phonegap's push plugin to receive push notifications. While installing the plugin you need to pass `SENDER_ID`, which you will get from the same place in the Firebase console settings:

```bash
// Install Cordova Push Plugin
$ionic cordova:plugin add phonegap-plugin-push@1.9.4 --variable
    SENDER_ID=76xxxxxxxxx --save

// NPM package
$ npm install @ionic-native/push --save
```

We now need to update our cloud configuration in our code in `app.module.ts`:

```typescript
// src/app/app.module.ts

const cloudSettings: CloudSettings = {
  'core': {
    'app_id': 'f8fec798'
  },
  'push': {
    'sender_id': '76xxxxxxxxx',
    'pluginConfig': {
      'ios': {
        'badge': true,
        'sound': true
      },
      'android': {
        'iconColor': '#343434'
      }
    }
  }
};
```

We now have to inject the push and integrate it in our application. We will call `Push.init()`, which will create an instance of the Push class. We will use this push instance to listen to registration and notification events:

```typescript
// src/app/app.component.ts

import { Push } from '@ionic-native/push';

platformReady() {
  // Call any initial plugins when ready
  this.platform.ready().then(() => {
    this.initPushNotification();
```

```javascript
}

initPushNotification() {
  console.log("calling push");
  let push = Push.init({
    android: {
      senderID: "762350093850"
    },
    ios: {
      alert: "true",
      badge: false,
      sound: "true"
    },
    windows: {}
  });

  push.on('registration', (data) => {
    console.log("device token ->", data.registrationId);
    //TODO - send device token to server
  });

  let self = this;

  push.on('notification', function(data) {
    TextToSpeech.speak({text: data.message, locale: 'en-IN', rate: 0.75})
      .then(() => console.log('Success'))
      .catch((reason: any) => console.log(reason));

    if (data.additionalData.foreground) {
      // if application open, show alert
      alert(data.message);
    }
    //if user NOT using app and push notification comes
    // for demo purpose for every push message we push
    // to AboutPage
    // Although you can apply logic and according to API
    // data
    // you can push to specific page
    self.nav.push(AboutPage, {message: data.message});

    push.finish(function() {
      console.log("processing of push data is finished");
    }, function() {
      console.log("something went wrong with push.finish for ID = " + data.additionalData.notId)
    }, data.additionalData.notId);
  });
}
```

Now we need to register the device before we can send push notifications. As we are using Ionic auth it's recommended to call the `register()` method after login so that the Push token saves to the authenticated user:

```typescript
// src/pages/login/login.ts

this.auth.login('basic', this.login).then((result) => {
  // user is now registered
  // Push Notification register
  this.push.register().then((data: PushToken) => {
    return this.push.saveToken(data);
  }).then((data: PushToken) => {
    console.log('Token saved:', data.token);
  });
}, (err: IDetailedError<string[]>) => {
  console.log(err);
  loading.dismiss();
  this.showToast(err)
});
```

Similarly, we will unregister a device when a user logs out:

```javascript
logout() {
    this.auth.logout();
    this.push.unregister();
    this.events.publish('user:logout');
    return true;
}
```

Let's send a push notification from the dashboard; there are multiple options available when you send a message. There are three steps and multiple options available when you send a notification:

- Campaign Name, Subject for Notification, message, payload (key/value pairs), and device options
- Targeting specific user segments or all users
- Scheduling push to send now, later, or based on some trigger. Also, which profile to send notification

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_240_0.png)

The next step from here can be integrating Push APIs in our backend codebase if required. This way there can be multiple possibilities and conditions we can send notifications. Just make sure that you don't send too many notifications to the user's device as many times this is the reason why users uninstall applications.

## Ionic deploy

The Ionic deploy service gives you access to live deployments, which helps you publish web assets such as HTML, CSS, and JS directly to your users without uploading to the Play Store every time. You will have many advantages of using this approach for live deployments:

- Real-time Updates on demand
- Save app stores approval time
- A/B testing by uploading different snapshots to different channels

Before we get started, let's install the deploy plugin, which will update your application on Android and iOS devices:

```
$ ionic cordova:plugin add ionic-plugin-deploy -save
```

We will be injecting the `Deploy` method inside our `app.component.ts`, so every time when the application is opened it checks for new updates: // src/app/app.component.ts

```typescript
import { AlertController } from 'ionic-angular';
import { Deploy } from '@ionic/cloud-angular';

constructor(
public deploy: Deploy,
public alertCtrl: AlertController
) {

// checks if new snapshot available
this.deploy.check().then((snapshotAvailable: boolean) => {
if (snapshotAvailable) {
let alert = this.alertCtrl.create({
title: 'Update Available !',
message: 'Do you want to update the application now ?',
buttons: [
{
text: 'Later',
role: 'cancel',
handler: () => {
console.log('Cancel clicked');
}
},
{
text: 'Update',
handler: () => {
this.deploy.download().then(() => {
```

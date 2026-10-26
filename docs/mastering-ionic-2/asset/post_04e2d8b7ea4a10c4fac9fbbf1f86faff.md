
## Ionic Platform and Services

So far, we have looked at almost all the key components for building an enterprise level application. Ionic platform is a set of beautiful tools and services that speed up the process of development and streamline everything on a single platform from building, testing, debugging, and deploying applications. With the Ionic team continuously working on improving Ionic platform and tools, you will see a lot more in the near future. We will cover almost all services and tools that Ionic provides so you can utilize your project:

- Ionic Cloud
- Ionic Auth
- Ionic DB
- Ionic push
- Ionic deploy
- Ionic package
- Ionic View, Creator, and Playground

## Ionic Cloud

Ionic started as a framework called Ionic framework, which has now become a complete platform with the launch of Ionic.io and tools and services involved with it. Ionic.io or Ionic Cloud Platform provides rich and hybrid-focused backend services and tools that ease the process of development and help build enterprise applications at a rapid pace. Ionic platform at present is out from beta phase and they have launched services now on some paid subscription models as it's necessary to sustain and generate revenue for the Ionic team. But the Ionic framework will always be open source; just the backend services will have some pricing plans. Getting started with the Ionic platform is as simple as signing up for an account. After this, you will be able to share, upload, and use all other services provided by the platform.

## Installing Ionic Cloud

Setting up Ionic Cloud is really simple; we need to install the Ionic Cloud npm package and hook Ionic account, which is just required for using Ionic services. We will be integrating cloud services to our existing vPlanet-commerce application. Recently, Ionic CLI v3 has launched and is currently in beta and it have changed a lot many things and changed the commands:

```bash
$ npm install @ionic/cloud-angular --save
$ ionic login
$ ionic link
```

> Be sure not to confuse Ionic CLI v3 and Ionic v3, which are completely different. Ionic CLI contains primary tools for the development of Ionic apps.

**To update an Ionic 2 or 3 based project to work with Ionic CLI v3, we have to run the following command.**

```bash
$ npm install --save-dev @ionic/cli-build-ionic-angular@beta @ionic/cli-plugin-cordova@beta
```

Now, when you will link the project to an existing project or create a new project, make sure you have logged in to Ionic Cloud which will ask for the username and password of the Ionic account, which you might have already created at `ionic.io`. After this you can upload your snapshot to Ionic Cloud.

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_221_0.png)

The next step is to import `cloudSettings` inside our `app.module.ts`:

```typescript
// src/app/app.module.ts

import { CloudSettings, CloudModule } from '@ionic/cloud-angular';

const cloudSettings: CloudSettings = {
  'core': {
    'app_id': 'f8fec798'
  }
};

@NgModule({
  declarations: [ ... ],
  imports: [
    IonicModule.forRoot(vPlanetApp),
    Ionic2RatingModule,
    TranslateModule.forRoot({
      provide: TranslateLoader,
      useFactory: createTranslateLoader,
      deps: [Http]
    }),
    CloudModule.forRoot(cloudSettings)
  ],
  bootstrap: [IonicApp],
  entryComponents: [ ... ],
  providers: [ ... ]
})
```

We have successfully now uploaded our vPlanet-Commerce application to ionic.io. We are now ready to start using Ionic services such as Push, Deploy, DB, and so on. You can find all your applications on the Dashboard (https://apps.ionic.io/apps):

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_222_0.png)

You can find all the information about the application inside the Settings tabs. After you click on your specific project, you will be able to see details about specific projects. Currently, we are using a free plan for our vPlanet-Commerce application, which gives around 5000 deploys, 10,000 push notifications, and 100 package builds. You can upgrade the plan at anytime as you will see that your application users are increasing and traffic is growing. What's best is that we will be integrating each and every Ionic Cloud service starting from Auth for user login and sign-up process. Ionic DB as our application database, Push Notification, Package, and Deploy:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_222_1.png)

Also, another important point to note is that when you have completed setting up the Ionic Cloud, you will find app_id and api_key inside .io-config.json and some other app details inside ionic.config.json:

```json
// .io-config.json
{"app_id":"f8fec798", "api_key":"5b22exxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx68e"}

// ionic.config.json
{
  "name": "vplanet-commerce",
  "app_id": "f8fec798",
  "v2": true,
  "typescript": true
}
```

After this chapter completion, you will be almost in mid of the book and in half way itself you are now able to build an application with Ionic, Integrate various Ionic components, Cordova plugins and able to integrate with Ionic Cloud services and finally deploy the application to the Play Store.

```typescript
}
}

showToast(response_message:any) {
    let toast = this.toastCtrl.create({
        message: (response_message ? response_message : "Log In Successfully"),
        duration: 1500
    });
    toast.present();
}

onSignup() {
    this.navCtrl.push(SignupPage);
}
}
```

We have integrated this with a login form that gets the data as an object and we pass it in this.auth.login() method, as you can see in the preceding code. This will return token and various other data that is stored inside local storage by default. In the following screenshot you can see mainly three key/value pairs that are saved, ionic_auth, ionic_insights_session, and ionic_user:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_226_0.png)

Next, we integrated this with a login form that gets the data as an object and we pass it in this.auth.login() method, as you can see in the preceding code. This will return token and various other data that is stored inside Local Storage by default. In the preceding screenshot, you can see mainly three key/value pairs. User data is mainly stored inside ionic_user, where you will find username, e-mail, and other details. Let's now look into registering a new user inside our application. We will be integrating it within our SignupPage:

```typescript
// src/pages/signup/signup.ts

import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';
import { NavController } from 'ionic-angular';
import { LoginPage } from '../login/login';
import { Auth, UserDetails, IDetailedError } from '@ionic/cloud-angular';

@Component({
    selector: 'page-user',
    templateUrl: 'signup.html'
})
export class SignupPage {

    signup: {name?: string, email?: string, password?: string} = {};

    details: UserDetails = {
        "name": this.signup.name,
        "email": this.signup.email,
        "password": this.signup.password
    };

    submitted = false;

    constructor(
        public navCtrl: NavController,
        public userService: UserService,
        public auth: Auth) {}

    onSignup(form: NgForm) {
        this.submitted = true;

        if (form.valid) {
            // registering new user
            this.auth.signup(this.signup).then(() => {
                // user is now registered
                this.navCtrl.push(LoginPage);
            }, (err: DetailedError<string[]>) => {
                for (let e of err.details) {
                    if (e === 'conflict_email') {
                        alert('Email already exists.');
                    } else {
                        // handle other errors
                    }
                }
            });
        }
    }
}
```

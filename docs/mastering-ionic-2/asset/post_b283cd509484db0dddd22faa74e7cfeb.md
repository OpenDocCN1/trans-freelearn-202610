
```ts
declarations: [
  vPlanetApp,
  AboutPage,
  AccountPage,
  LoginPage,
  PopoverPage,
  SignupPage,
  WalkThroughPage,
  HomePage,
  CategoriesPage,
  ProductsPage,
  ProductsFilterPage,
  ProductDetailPage,
  SearchPage,
  WishlistPage,
  ShowcartPage,
  CheckoutPage,
  SettingsPage,
  SupportPage,
  OrdinalPipe,
],
imports: [
  BrowserModule,
  HttpModule,
  IonicModule.forRoot(vPlanetApp, {locationStrategy: 'hash'}, deepLinkConfig),
  Ionic2RatingModule,
  TranslateModule.forRoot({
    provide: TranslateLoader,
    useFactory: createTranslateLoader,
    deps: [Http]
  }),
  IonicStorageModule.forRoot({
    useFactory: provideStorage
  }),
  CloudModule.forRoot(cloudSettings)
],
bootstrap: [IonicApp],
entryComponents: [
  vPlanetApp,
  AboutPage,
  AccountPage,
  LoginPage,
  PopoverPage,
  SignupPage,
  WalkThroughPage,
  HomePage,
  CategoriesPage,
  ProductsPage,
  ProductsFilterPage,
  ProductDetailPage,
  SearchPage,
  WishlistPage,
  ShowcartPage,
  CheckoutPage,
  SettingsPage,
  SupportPage
],
providers: [
  SplashScreen,
  Deeplinks,
  TextToSpeech,
  AppRate,
  GoogleAnalytics,
  Camera,
  CallNumber,
  SocialSharing,
  Push,
  {provide: ErrorHandler, useClass: IonicErrorHandler},
  { provide: Storage, useFactory: provideStorage },
  UserService,
  DataService
]
})
export class AppModule {}
```

Furthermore, I would suggest you look at the code in this chapter for the vPlanet Commerce app, which will help you get a good idea of what pages and components we will be using in our application. As this chapter mainly targets various Ionic components, we will be using a lot of these components in our application.

## Ionic components

There are many Ionic JavaScript components that we can effectively use while building our application. What's best is to look around for features that we will need in our application. Let's get started with the home page of our e-commerce application, which will have an image slider with banners on it.

### Slides

The slides component is a multi-section container that can be used in multiple scenarios, such as a tutorial view or banner slider. The `<ion-slides>` component has multiple `<ion-slide>` elements, which can be dragged or swiped left/right. Slides have multiple configuration options available, which can be passed in the ion-slides such as autoplay, pager, direction: vertical/horizontal, initial slide, and speed.

Using slides is really simple as we just have to include them inside our home.html; no dependency is required for this to be included in the home.ts file:

```html
// Banner Slider in src/pages/home/home.html

<ion-slides pager #adSlider (ionSlideDidChange)="logLengthTrackEvent()" style="height: 250px">
  <ion-slide *ngFor="let banner of banners">
    <img [src]="banner">
  </ion-slide>
</ion-slides>

// Defining banners image path

export class HomePage {

  products: any;
  banners: String[];

  constructor() {
    this.banners = [
      'assets/img/banner-1.webp',
      'assets/img/banner-2.webp',
      'assets/img/banner-3.webp'
    ]
  }
}
```

As you can see from the preceding code, there are multiple properties available for ion-slides, such as pager, autoplay, direction, and so on:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_104_0.png)

If you look at the preceding screenshot, you can see we have used white background banners. Actually, just as a suggestion, try to use the same background color in banners that are inside the application. This way the banners will come up nice in low or high resolution mobiles and tabs.

### Lists

Lists are one of the most used components in many applications. Inside lists we can display rows of information. We will be using lists multiple times inside our application, such as on the `categories` page where we are showing multiple subcategories:

```html
// src/pages/categories/categories.html
<ion-content class="categories">
  <ion-list-header *ngIf="!categoryList">Fetching Categories ....</ion-list-header>
  <ion-list *ngFor="let cat of categoryList">
    <ion-list-header>{{cat.name}}</ion-list-header>
    <ion-item *ngFor="let subCat of cat.child">
      <ion-avatar item-left>
        <img [src]="subCat.image">
      </ion-avatar>
      <h2>{{subCat.name}}</h2>
      <p>{{subCat.description}}</p>
      <button ion-button clear item-right (click)="goToProducts(subCat.id)">View</button>
    </ion-item>
  </ion-list>
</ion-content>
```

We are using an avatar list here with a header on top of each category. There are multiple types of lists available such as multi-line, thumbnail, and sliding lists also. We will be integrating the sliding list on the showcart page further in this chapter:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_107_0.png)

As you can see, the preceding screenshot shows what the avatar list will look like on the categories page. We should make sure that, before we plan to use lists in our application, we have a look at the documentation, as this helps us visualize which type of list can be utilized best in various different applications.

### Loading and Toast

The loading component can be used to indicate some activity while blocking any user interactions. One of the most common cases of using the Loading component is HTTP calls to the server, as we know it takes time to fetch data from the server, until then for good user experience we can show some content showing Loading ..., or Login wait ..., for login pages.

Toast is a small popup that provides feedback, usually used when some action is performed by the user. Ionic 3 now provides the toast component as part of its library; previously we had to use the native Cordova plugin for toasts, which can also be used now too.

Loading and toast components both have a `create` method. We have to provide options while creating these components:

```ts
// src/pages/login/login.ts
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';
import { NavController, LoadingController, ToastController, Events } from 'ionic-angular';
import { SignupPage } from '../signup/signup';
import { HomePage } from '../home/home';

import { Auth, IDetailedError } from '@ionic/cloud-angular';

import { UserService } from '../../providers/user-service';

@Component({
  selector: 'page-user',
  templateUrl: 'login.html'
})
export class LoginPage {
  login: { email?: string, password?: string } = {};

  submitted = false;

  constructor(public navCtrl: NavController,
    public loadingCtrl: LoadingController,
    public auth: Auth,
    public userService: UserService,
    public toastCtrl: ToastController,
    public events: Events) { }

  onLogin(form: NgForm) {
    this.submitted = true;

    if (form.valid) {
      // start Loader
      let loading = this.loadingCtrl.create({
        content: "Login wait...",
        duration: 20
      });
      loading.present();

      this.auth.login('basic', this.login).then((result) => {
          // user is now registered
          this.navCtrl.setRoot(HomePage);
          this.events.publish('user:login');
          loading.dismiss();
          this.showToast(undefined);
        }, (err: IDetailedError<string[]>) => {
          console.log(err);
          loading.dismiss();
          this.showToast(err)
        });
    }
  }

  showToast(response_message: any) {
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

As you can see from the preceding code, creating a loader and toast is almost similar at code level. The options provided while creating are also similar; we have used loader here while logging in and toast after that to show the desired message.

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_111_0.png)

*Setting the duration option is good, because if the loader is dismissed or not handled properly in code then we will block the user from any further interactions on the app. In HTTP calls to the server we might get connection issues or failure cases; in that scenario it may end up blocking users.*

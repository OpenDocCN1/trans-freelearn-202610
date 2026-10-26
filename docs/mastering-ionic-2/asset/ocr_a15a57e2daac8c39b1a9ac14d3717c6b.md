```
// src/pages/about/about.html

<ion-fab right bottom>
  <button ion-fab><ion-icon name="help"></ion-icon></button>
  <ion-fab-list side="top">
    <button (click)="callNow()" ion-fab><ion-icon name="call"></ion-icon></button>
    <button (click)="shareViaEmail()" ion-fab><ion-icon name="mail"></ion-icon></button>
    <button (click)="shareViaFacebook()" ion-fab><ion-icon name="logo-facebook"></ion-icon></button>
    <button (click)="shareViaWhatsApp()" ion-fab><ion-icon name="logo-whatsapp"></ion-icon></button>
    <button (click)="share()" ion-fab><ion-icon name="share"></ion-icon></button>
  </ion-fab-list>
</ion-fab>
```

![](bbox=[0.38, 0.27, 0.62, 0.5])

Previously listed are all the methods that the share plugin provides. You can see in the code that where we used the shareViaFacebook method, we wrapped it around the canShareVia method to pre check if the Facebook app is installed or not. Similarly, we can use it for Twitter, Instagram, and Whatsapp. Sharing via SMS and e-mail examples are also there, so we can directly share with those methods too. The share plugin also supports multiple images via array. The SaveToPhotoAlbum method is also available in the plugin core code, but until now Ionic Native did not have any wrapper for it. So you have to use it like this:

```
window.plugins.socialsharing.saveToPhotoAlbum(
  ['http://veloice.com/icon.png', 'www/veloice.jpg'],
  onSuccess, // optional success function
  onError // optional error function
);
```

We have now covered some of the most used plugins. We will continue to look at more complex plugins that need detailed examples, such as Facebook and SQLite. Our main focus will be to continue to closely stick to Ionic Native and make full use of it.
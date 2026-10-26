
```html
<link rel="manifest" href="manifest.json">
```

```json
// src/manifest.json
{
  "name": "Currency Converter PWA",
  "short_name": "Currency",
  "start_url": "index.html",
  "display": "standalone",
  "icons": [{
    "src": "assets/logo.png",
    "sizes": "512x512",
    "type": "image/png"
  }],
  "background_color": "#db0000",
  "theme_color": "#000000",
  "gcm_sender_id": "124917710568"
}
```

- fullscreen: All of the available user area is used and no user agent Chrome is shown.
- display: Defines the developers preferred display mode for the web application.
- start_url: Specifies the URL that loads when a user launches the application from a device. In our Ionic application it will always be index.html.
- short_name: This is to be used where there is insufficient space to display the full name of the web application.
- name: This is what the user will see when they open the application on the splashscreen page.
- Standalone: The application will look and feel like a standalone application. This can include the application having a different window, its own icon in the application launcher, and so on.
- minimal-ui: The application will look and feel like a standalone application, but it will have a minimal set of UI elements for controlling navigation. The elements will vary by browser.
- Browser: The application opens in a conventional browser tab or new window, depending on the browser and the platform. This is the default.
- Icons: Array of icons used in various different places for the application, for example, when we add it to the home screen this icon is placed with the short_name of the application.
- background_color: Expected background color for the web application.
- theme_color: Default theme color for an application.
- gcm_sender_id: Used for sending web push notifications, represents sender_id from Firebase Cloud:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_360_0.png)

You can see how the application splash screen will look like when we have installed it, with some of the preceding properties implemented.

## Service workers

Using service workers with Ionic 3 is really easy, Ionic comes with the `service-worker.js` file, which is initially commented in the `index.html` file. We just need to uncomment that section and you will see service workers registered in your browser.

Service workers essentially act as proxy servers that sit between web applications and between the browser and network (when available). They are intended to (amongst other things) enable the creation of effective offline experiences, intercepting network requests, and taking appropriate action based on whether the network is available and updated assets reside on the server. They will also allow access to push notifications and background sync APIs.

A service worker has life cycle and events for various actions. We initially start by registering a service worker on our page, on which successful registration life cycle initiate with install and activate events. Ionic 3 comes with the built-in registration code in the `index.html` file inside the `src` folder, all you need is to uncomment the code. Now you can check that a service worker is enabled by going to [chrome://serviceworker-internals/](chrome://serviceworker-internals/) by looking at your site:

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_362_0.png)

As you can see, our locally served application at `localhost:8100` has a registered service worker. Although, when we deploy PWAs, it must be served securely (HTTPS without cert errors). Firebase hosting and GitHub pages host over HTTPS, so they can be best place to host your PWA application.

Now that we are done with the registration, it's time to install events to start its work. Inside our install event we cache all our static files. The `event.waitUntil()` method ensures that the service worker is not installed until the code inside it is not executed. Now, `cache.addAll()` caches the static resources in cache storage:

```javascript
var cacheName = 'cache-v1';
// pre-cache our key assets
var files = [
  './build/main.js',
```

```javascript
caches.match(request).then((response) => {
    if (response) {
        return response;
    }

    //if request is not cached, add it to cache
    return fetch(request).then((response) => {
        var responseToCache = response.clone();
        caches.open(cacheName).then((cache) => {
            cache.put(request, responseToCache).catch((err) => {
                console.warn(request.url + ': ' + err.message);
            });
        });

        return response;
    })
});
```

> *After sw-toolbox support we use sw-toolbox router methods for fetching the resources and caching them according to different strategies available, which we will look at further in our demonstration.*

Service workers can perform push events, to create user notifications, to open a specific page, or to run some background script:

```javascript
self.addEventListener('push', (event) => {
console.log('Received a push message', event);

const title = 'Yay a message.';
const body = 'We have received a push notification.';
const icon = 'assets/icon.png';
const tag = 'simple-push-demo-notification-tag';

event.waitUntil(
    self.registration.showNotification(title, {
        body: body,
        icon: icon,
        tag: tag
    })
);
});
```

Recently, Ionic added support for sw-toolbox, which is a `service-worker` library and it reduces a lot of work.

### Service worker toolbox (sw-toolbox)

is now built-in to the starters and conference application. This allows you to customize your service worker using a high-level API instead of using the raw service worker API. Our out-of-the-box configuration will give your application a good, network independent experience, but you can easily customize this to fit your application's unique use cases.

We have gone through the various events of service workers and how exactly they work. An important thing here is to use the Chrome developer tools for debugging service workers and look into the service worker status and caches available. For further research on service workers and what all we can do using them you can look into the following documentation links by Google and Mozilla:

https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers
https://developers.google.com/web/fundamentals/getting-started/primers/service-workers.

```javascript
localStorage.setItem('anonKey', subKey);
});
})
.catch(err => console.log('Error', err));
}
</script>
```

You can see here that after successful registration we subscribe push notification from pushManager. This will generate the subscription endpoint, which is responsible for sending notifications to all the users. The next step is to set up our `manifest.json` file:

```json
{
  "name": "Currency Converter PWA",
  "short_name": "Currency",
  "start_url": "index.html",
  "display": "standalone",
  "icons": [{
    "src": "assets/logo.png",
    "sizes": "512x512",
    "type": "image/png"
  }],
  "background_color": "#db0000",
  "theme_color": "#000000",
  "gcm_sender_id": "124917710568"
}
```

We have discussed all the properties in the last section, here we will add our `gcm_sender_id` for user notifications. You will get the `gcm_sender_id` from **Firebase settings** in cloud messaging. From there copy the server key as later on it will be required to send a web push notification.

Service workers are the most important for progressive web applications. Here is our default `service-worker.js`, we made some further changes according to our requirements. Ionic, by default, does not use the `sw-toolbox` library, which has reduced the code and efforts a lot:

```javascript
// src/service-worker.js

'use strict';
importScripts('./build/sw-toolbox.js');

var version = 1;

self.toolbox.options.debug = true;

self.toolbox.options.cache = {
  name: 'ionic-cache' + version
};

// pre-cache our key assets
self.toolbox.precache(
  [
  './build/main.js',
  './build/main.css',
  './build/polyfills.js',
  'index.html',
  'manifest.json',
  '/'
  ]
);
```

We start with setting up the cache name, which we name with the version number specified. So whenever there is some new change we change the version and a new `cacheStorage` is generated. Next we cache all our static resources here using the `precache()` method and pass all static files paths. You will see that we have added another resource, which is not in the default file, as our application serve on browser at `localhost:8100` which in case went offline after first time will not cache it and you will see an error. If you don't want to add this just refresh the page twice as that will cache this

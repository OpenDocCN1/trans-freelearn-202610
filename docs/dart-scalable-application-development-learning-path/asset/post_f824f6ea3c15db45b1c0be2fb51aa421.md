
### There's more...

Local storage can be disabled (by user action, or via an installed plugin or extension), so we must alert the user when this needs to be enabled; we can do this by catching the exception that occurs in this case:

```dart
try {
    window.localStorage[key] = data;
} on Exception catch (ex) {
    window.alert("Data not stored: Local storage is disabled!");
}
```

Local storage is a simple key-value store and does have good cross-browser coverage. However, it can only store strings and is a blocking (synchronous) API; this means that it can temporarily pause your web page from responding while it is doing its job storing or reading large amounts of data such as images. Moreover, it has a space limit of 5 MB (this varies with browsers); you can't detect when you are nearing this limit and you can't ask for more space. When the limit is reached, an error occurs so that the user can be informed.

> These properties make local storage only useful as a temporary data storage tool; this means it is better than cookies, but not suited for a reliable, database kind of storage.

Web storage also has another way of storing data called sessionStorage used in the same way, but this limits the persistence of the data to only the current browser session. So, data is lost when the browser is closed or another application is started in the same browser window.

### See also

- For more information on the JSON format, refer to the Making toJSON and fromJSON methods recipe in Chapter 4, Object Orientation in your class
- A better alternative to simple local storage is IndexedDB; see the Storing data locally in IndexedDB recipe in Chapter 9, Working with Databases

## Using application cache to work offline

When, for some reason, our users don't have web access or the website is down for maintenance (or even broken), our web-based applications should also work offline. The browser cache is not robust enough to be able to do this, so HTML5 has given us the mechanism of ApplicationCache. This cache tells the browser which files should be made available offline. The effect is that the application loads and works correctly, even when the user is offline. The files to be held in the cache are specified in a manifest file, which has a `.mf` or `.appcache` extension.

### How to do it...

Look at the appcache application; it has a manifest file called `appcache.mf`.

1. The manifest file can be specified in every web page that has to be cached. This is done with the manifest attribute of the `<html>` tag:

   ```html
   <html manifest="appcache.mf">
   ```

   If a page has to be cached and doesn't have the manifest attribute, it must be specified in the CACHE section of the manifest file. The manifest file has the following (minimum) content:

   ```text
   CACHE MANIFEST
   # 2012-09-28:v3

   CACHE:
   Cached1.html
   appcache.css
   appcache.dart
   http://dart.googlecode.com/svn/branches/bleeding_edge/dart/client/dart.js

   NETWORK:
   *

   FALLBACK:
   / offline.html
   ```

2. Run cached1.html. This displays the **This page is cached, and works offline!** text. Change the text to This page has been changed! and reload the browser. You don't see the changed text because the page is created from the application cache.

3. When the manifest file is changed (change version v1 to v2), the cache becomes invalid and the new version of the page is loaded with the **This page has been changed!** text.

4. The Dart script of the page, `appcache.dart`, should contain the following minimal code to access the cache.

   ```dart
   main() {
     new AppCache(window.applicationCache);
   }

   class AppCache {
     ApplicationCache appCache;

     AppCache(this.appCache) {
       appCache.onUpdateReady.listen((e) => updateReady());
       appCache.onError.listen(onCacheError);
     }
   }

   void updateReady() {
       if (appCache.status == ApplicationCache.UPDATEREADY) {
           // The browser downloaded a new app cache. Alert the user:
           appCache.swapCache();
           window.alert('A new version of this site is available. Please reload.');
       }
   }

   void onCacheError(Event e) {
       print('Cache error: ${e}');
       // Implement more complete error reporting to developers
   }
   ```

### How it works...

The CACHE section in the manifest file enumerates all the entries that have to be cached. The `NETWORK:` and `*` options mean that to use all other resources, the user has to be online. FALLBACK specifies that offline.html will be displayed if the user is offline and a resource is inaccessible. A page is cached when either of the following is true:

- Its HTML tag has a manifest attribute pointing to the manifest file
- The page is specified in the CACHE section of the manifest file

The browser is notified when the manifest file is changed, and the user will be forced to refresh its cached resources. Adding a timestamp and/or a version number such as `# 2014-05-18:v1` works fine. Changing the date or the version invalidates the cache, and the updated pages are again loaded from the server.

To access the browser's app cache from your code, use the window.applicationCache object. Make an object of the class AppCache, and alert the user when the application cache has become invalid (the status is UPDATEREADY) by defining an onUpdateReady listener.

### There's more...

The other known states of the application cache are UNCACHED, IDLE, CHECKING, DOWNLOADING, and OBSOLETE. To log all these cache events, you could add the following listeners to the appCache constructor:

```dart
appCache.onCached.listen(onCacheEvent);
appCache.onChecking.listen(onCacheEvent);
appCache.onDownloading.listen(onCacheEvent);
appCache.onNoUpdate.listen(onCacheEvent);
appCache.onObsolete.listen(onCacheEvent);
appCache.onProgress.listen(onCacheEvent);
```

Provide an onCacheEvent handler using the following code:

```dart
void onCacheEvent(Event e) {
    print('Cache event: ${e}');
}
```

## Preventing an onSubmit event from reloading the page

The default action for a submit button on a web page that contains an HTML form is to post all the form data to the server on which the application runs. What if we don't want this to happen?

### How to do it...

Experiment with the submit application by performing the following steps:

1. Our web page `submit.html` contains the following code:

   ```html
   <form id="form1" action="http://www.dartlang.org" method="POST">
     <label>Job:<input type="text" name="Job" size="75"></input>
     </label>
     <input type="submit" value="Job Search">
   </form>
   ```

   Comment out all the code in `submit.dart`. Run the app, enter a job name, and click on the Job Search submit button; the Dart site appears.

2. When the following code is added to `submit.dart`, clicking on the submit button no longer makes the Dart site appear:

   ```dart
   import 'dart:html';

   void main() {
     querySelector('#form1').onSubmit.listen(submit);
   }

   submit(Event e) {
     e.preventDefault();
     // code to be executed when button is clicked
   }
   ```

### How it works...

In the first step, when the submit button is pressed, the browser sees that the method is POST. This method collects the data and names from the input fields and sends it to the URL specified in action to be executed, which only shows the Dart site in our case.

To prevent the form from posting the data, make an event handler for the onSubmit event of the form. In this handler code, `e.preventDefault();` as the first statement will cancel the default submit action. However, the rest of the submit event handler (and even the same handler of a parent control, should there be one) is still executed on the client side.

## Dynamically inserting rows in an HTML table

When displaying data coming from a database, you often don't know how many data records there will be. Our web page and the HTML table in it have to adapt dynamically. The following recipe describes how to do this.

### How to do it...

Look at the html_table application. The web page contains two `<table>` tags:

```html
<table id="data"></table>
<table id="jobdata"></table>
```

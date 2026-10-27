# There's more...

Local storage can be disabled (by user action, or via an installed plugin or extension), so we must alert the user when this needs to be enabled; we can do this by catching the exception that occurs in this case:

```
try {
    window.localStorage[key] = data;
} on Exception catch (ex) {
    window.alert("Data not stored: Local storage is disabled!");
}
```

Local storage is a simple key-value store and does have good cross-browser coverage. However, it can only store strings and is a blocking (synchronous) API; this means that it can temporarily pause your web page from responding while it is doing its job storing or reading large amounts of data such as images. Moreover, it has a space limit of 5 MB (this varies with browsers); you can't detect when you are nearing this limit and you can't ask for more space. When the limit is reached, an error occurs so that the user can be informed.

> These properties make local storage only useful as a temporary data storage tool; this means it is better than cookies, but not suited for a reliable, database kind of storage.

Web storage also has another way of storing data called sessionStorage used in the same way, but this limits the persistence of the data to only the current browser session. So, data is lost when the browser is closed or another application is started in the same browser window.

# See also

+   - For more information on the JSON format, refer to the Making toJSON and fromJSON methods recipe in Chapter 4, Object Orientation in your class
+   - A better alternative to simple local storage is IndexedDB; see the Storing data locally in IndexedDB recipe in Chapter 9, Working with Databases

# Using application cache to work offline

When, for some reason, our users don't have web access or the website is down for maintenance (or even broken), our web-based applications should also work offline. The browser cache is not robust enough to be able to do this, so HTML5 has given us the mechanism of ApplicationCache. This cache tells the browser which files should be made available offline. The effect is that the application loads and works correctly, even when the user is offline. The files to be held in the cache are specified in a manifest file, which has a .mf or .appcache extension.
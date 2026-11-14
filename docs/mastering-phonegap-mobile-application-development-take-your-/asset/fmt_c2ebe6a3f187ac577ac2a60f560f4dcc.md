
# Testing with Gulp and Mocha

## Unit Tests

```
"use strict";

var gulp = require("gulp"),
    notify = require("gulp-notify"),
    config = require("../config"),
    paths = require("../utils/paths"),
    mocha = require("gulp-mocha");

function test() {
    return gulp.src(paths.makeFullPath(config.test.code), 
                    {read: false})
               .pipe(mocha({reporter: "spec"}))
               .once("error", notify.onError(
               "TEST: <%= error.message %>\n <%= error.stack %>"))
               .once("error", function () {
                   process.exit(1);
               })
               .once("end", function () {
                   process.exit();
               });
}

module.exports = {
    task: test
}
```

This should be fairly straightforward; most of it is error handling and notifications. In fact, the `process.exit` methods are there to ensure a clean exit from the test. Otherwise, this is equivalent to executing `mocha . --reporter=spec`. Because we've required `Babel` at the top of `gulpfile.js`, all our tests automatically get transpiled by `Babel`.

## UI Automation Tests

Our automation tests aren't more complex. The only additional code is in handling the environment variables:

```
"use strict";

var gulp = require("gulp"),
    gutil = require("gulp-util"),
    notify = require("gulp-notify"),
    config = require("../config"),
    settings = require("../settings"),
    paths = require("../utils/paths"),
    mocha = require("gulp-mocha");

function testUI() {
    var iosUDID = gutil.env.udid;
    var profile = gutil.env.profile;
    var server = gutil.env.server || "local";

    process.env["UIA_PROFILE"] = profile;
    process.env["UIA_SERVER"] = server;
    if (settings.VERBOSE) {
        process.env["UIA_LOGGING"] = "enabled";
    }
    if (iosUDID) {
        process.env["UIA_UDID"] = iosUDID;
    }

    return gulp.src(paths.makeFullPath(config.test.ui), 
                    {read: false})
               .pipe(mocha({reporter: "spec"}))
               .on("error", notify.onError(
                   "TEST-UI: <%= error.message %>"))
               .once("error", function () {
                   process.exit(1);
               })
               .once("end", function () {
                   process.exit();
               });
}

module.exports = {
    deps: ["build"],
    task: testUI,
}
```

Other than the section dealing with the environment variables, the only other change is that of `config.test.ui` as the configuration setting that indicates where the tests are stored.

### Note

Also note that `test-ui` is dependent upon the `build` step. We make this a dependency, because we need the built app packages for testing. If you want, you can remove the dependency; but you'll need to remember to rebuild your app manually every time something changes.

Alternatively, you can create a setting that allows you to skip the build step whenever you need (which might be useful when you are simply changing your test code). Check the code package for this book for an example. Note that the change is actually in `gulp/tasks/cordova-build.js`, not `test-ui.js`.

## Running Tests

Now, to run code tests, you should just be able to execute the following:

```
gulp test
```

To run the automation tests, the command gets a little more complicated, but not terribly so:

```
gulp test-ui --target=device|emulator --profile=appium-profile --udid=ios-udid --verbose=yes|no --server=local|sauce
```

Your target should line up with the profile you are using for testing. If you are testing on an emulator, specify `emulator` for the target. If you're testing on a device, specify `device`. The `udid` switch is only required to test on physical iOS devices. Logging is controlled by `verbose`; if omitted, no logging occurs. `server` is also optional; if omitted, it will default to `local`.

There's no reason why you couldn't chain several of these commands together such that one `Gulp` command kicks them all off, but we'll leave it as an exercise for you.

Wow! We dealt with a lot of complicated stuff in this chapter. We installed `Mocha` and `Appium`. We also covered creating code tests and user interface automation tests. Finally, we integrated our processes into `Gulp`.

In the next chapter, we'll delve into `IndexedDB`, which is a persistent object store. We'll use this to store data for our app.

# IndexedDB Overview

## Storage Options

There are many different kinds of persistent storage that `Cordova` apps can use. Which storage mechanism you use depends largely upon the storage needs of your application. Furthermore, your app isn't restricted to a single type of storage. It might make the most sense to store user preferences in `localStorage` and larger files using the `File API`. On the other hand, neither `localStorage` nor the `File API` provides a simple mechanism for storing, retrieving, and searching structured data. You can use them for these purposes, of course, but you'll have to do most of the hard work yourself.

Two storage mechanisms come to our rescue so that we don't have to reinvent the wheel. The older (and now deprecated) standard, `Web SQL Database`, uses a relational data model typical of databases such as `Oracle`, `PostgreSQL`, `Microsoft SQL`, and so on. A newer standard called `IndexedDB` (short for Indexed Database) uses key-object storage. This is particularly useful with `JavaScript`, because we're most often working with objects. So, persisting these objects becomes very simple.

Neither `IndexedDB` nor `Web SQL Database` is specific to `Cordova`; you can use them on desktop browsers as well. This is a nice feature, because you can test your app's storage on a desktop browser for faster iteration and easier debugging. If you've written a website application for desktops, you could also potentially reuse your storage code (and probably a lot of other code as well). There's a catch though (there always is!): `IndexedDB` support is hit and miss; to use it without encountering a lot of painful issues, you'll probably need to use a polyfill or other wrapper. Thankfully, there are several available that are excellent.

## Chapter Contents

In this chapter, we'll cover the following:

*   `IndexedDB` support and polyfills
*   Differences between relational and key-object storage
*   Creating a database
*   Creating an object store within the database
*   Handling database upgrades
*   Transactions
*   Storing objects
*   Getting objects
*   Deleting objects
*   Using cursors and indexes
*   Closing the database

This chapter focuses mostly on the basics of using `IndexedDB`. If you want to see a real-life example, we've implemented `IndexedDB` into our demonstration app in the code package of this book located at `logology-v07/`.

Several of the snippets in this chapter are also available using the interactive snippet playground that accompanies the code package of this book. Instructions to launch the playground are in the `README.md` file in the code package. Once it starts, select **7: IndexedDB** and then browse and experiment with the examples. Where applicable, each section and snippet will indicate which chapter and example you should select.

## Browser Support

Before we go further, it's important to discuss the support situation for `IndexedDB`. As it is a more recent specification than `Web SQL Database`, it has much less support. On some platforms that support it, it can also be extremely limited and/or buggy.

### Note

For up-to-date browser support, see [http://caniuse.com/#feat=indexeddb](http://caniuse.com/#feat=indexeddb).

As of this writing, `IndexedDB` had the following mobile browser support:

*   **iOS**: No support on iOS 7.x; extremely buggy behavior on iOS 8.x as well as iOS 9.x
*   **Android**: No support for versions less than 4.4; full support on Android 4.4 and higher

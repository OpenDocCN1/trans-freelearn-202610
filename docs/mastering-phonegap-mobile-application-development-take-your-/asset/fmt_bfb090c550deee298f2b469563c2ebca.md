
```
project-root/
  src/
    config.xml            # Template file for our Cordova app's
                          # configuration
    res/                  # Icons & splash screens (covered in
                          # Chapter 11)
    www/                  # HTML, JavaScript, and CSS, and other
                          # web assets
      index.html          # Initial HTML file (as specified in
                          # config.xml)
      html/               # Additional HTML files, if any
      img/                # Image files, if we need them
      scss/               # Sassy CSS files (see Chapter 3)
        lib/              # Utility functions
        themes/           # Themes (appearance of the app)
        views/            # Styles specific to views in our app
      js/                 # JavaScript
        lib/              # Third-party library code and support
                          # code
        app/              # Our application code
          index.js        # The entry point for our app
          controllers/    # View controllers live here
          lib/            # App-specific utility files
          localization/   # Language translations
          models/         # Data models go here
          views/          # Views and templates live here
```

If you look at the directory structure of this chapter in the code bundle, you will notice that a lot of it is missing. This is because it's not necessary at this point; we'll fill it out in the future chapters.

If you're wondering where the Cordova files are, you're paying attention. There aren't any. *Yet*. This is because they are considered to be **build artifacts**. Build artifacts are files that are created when you compile your app. If this feels both a little strange and a little familiar at the same time, there's a good reason behind it: the Cordova projects already have portions that are considered to be build artifacts. The strange part is that you're probably used to editing the `www/` folder within the Cordova project, and executing `cordova build` to create the remaining build artifacts (namely, the native wrappers around your code, typically in `platforms/`).

In this book, however, we're going to take a higher level approach and consider the *entire Cordova project* as a build artifact. Why? Because Cordova has been extended by several other projects (such as Steroids: [http://www.appgyver.com/steroids](http://www.appgyver.com/steroids)) and they usually have their own project formats and build steps. If you ever want to target these platforms, you can readily do so since your code doesn't live within a Cordova project. Furthermore, you might find that you want to target other technologies entirely, such as Electron ([http://electron.atom.io](http://electron.atom.io)) which encapsulates your code with a Chromium webview suitable for desktop execution. The build steps and project structure for Electron are different than what you might expect for a Cordova project. In short, it's a way to avoid tying yourself down.

All said, when we're done with the chapter, you'll have a Cordova app filled with your source code. That project will be present in the `build/` directory.

### Tip

If you ever need to execute Cordova commands outside Gulp, you'll need to change to the `build/` directory first or the command will fail. This is because the Cordova CLI expects be to run within a Cordova project, and our app's root directory isn't a Cordova project. Only `build/` contains a valid Cordova project.

A crucial part of our workflow is going to be our project's `package.json` file. This file will contain the app's version information, Cordova configuration, and more. If you're starting from scratch, you will need to create this file yourself by changing to the project's root directory and executing `npm init`:

### Note

If you are using the code bundle for this chapter, the `package.json` file is already built for you.

```
# (in your project's root directory)
$ npm init [ENTER]
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sane
defaults.
…

name: (logology-v01) Logology [ENTER]
version: (1.0.0) [ENTER]
description: Logology and PhoneGap demonstration app [ENTER]
entry point: (index.js) [ENTER]
test command: [ENTER]
git repository: [ENTER]
keywords: dictionary word study phonegapcordova html5 javascript
css [ENTER]
author: Kerri Shotts<kerrishotts@gmail.com> [ENTER]
license: (ISC) MIT [ENTER]
About to write to .../logology-v01/package.json:

Is this ok? (yes) [ENTER]
```

At this point, you have the `package.json` file is created, but it will need a few more edits. Open the `package.json` file in your favorite editor and remove the `scripts` section. Then, add the following (for the full contents of this file you can refer to the code package):

```json
{ ...,
"cordova": {
  "name": "Logology",
  "id": "com.packtpub.logologyv1",
  "description": "Dictionary application",
  "author": {
    "name": "Kerri Shotts",
    "email": "kerrishotts@gmail.com",
    "site": "http://www.photokandy.com"
  },
  "template": "../blank",
  "platforms": [ "ios", "android" ],
  "preferences": {
    "permissions": "none",
    "fullscreen": "false",
    "orientation": "default",
    "stay-in-webview": "false",
    "ShowSplashScreenSpinner": "false",
    "AutoHideSplashScreen": "false",
    "disable-cursor": "false",
    "KeyboardDisplayRequiresUserAction": "false",
    "target-device": "universal",
    "prerendered-icon": "true",
    "webviewbounce": "false",
    "DisallowOverscroll": "true",
    "exit-on-suspend": "false",
    "deployment-target": "7.0",
    "detect-data-types": "false",
    "SupressesIncrementalRendering": "true",
    "android-minSdkVersion": "14",
    "android-installLocation": "auto",
    "android-windowSoftInputMode": "adjustResize",
  },
  "plugins": [
    "cordova-plugin-device@1.1.0",
    "cordova-plugin-network-information@1.1.0",
    "cordova-plugin-globalization@1.0.2",
    "cordova-plugin-whitelist@1.1.0",
    "ionic-plugin-keyboard@1.0.8",
    "cordova-plugin-inappbrowser@1.0.1"
    ]
  }
}
```

The preceding code should be fairly self-explanatory. With it, we are essentially duplicating the contents of Cordova's `config.xml` file. Because the Cordova project itself is considered to be a build artifact, it makes sense to manage plugins, platforms, and preferences somewhere else and, because `package.json` handles the other configuration aspects of our project, it makes sense to include these configuration settings here.

### Note

This doesn't remove the need for a `config.xml` file. We'll cover this later on in this chapter.

At this point, we're ready to install Gulp and any other dependencies our project might need.

Installing Gulp is easy, but is actually a two-step process. The first step is to install Gulp globally. This installs the command-line utility; but Gulp won't work without also being installed *locally* within our project. If you aren't familiar with Node.js, the packages can be installed locally and/or globally. A locally installed package is local to the project's root directory, while a globally installed package is specific to the developer's machine. Project dependencies are tracked in `package.json`, which makes it easy to replicate your development environment on another machine.

Assuming you have Node.js installed and `package.json` created in your project directory, the installation of Gulp will be very easy. Be sure you are positioned in your project's root directory and then execute the following:

```
$ npm install -g gulp@3.9.0
$ npm install --save-dev gulp@3.9.0
```

### Note

If you receive an error while running these commands on OS X, you may need to run them with `sudo`. For example: `sudo install -g gulp`.

You can usually ignore any `WARN` messages.

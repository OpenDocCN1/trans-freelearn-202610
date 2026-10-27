# How to do it...

To generate a script snapshot file called `prorabits` from the Dart script `prorabits.dart`, issue the following command:

```
```dart
dart --snapshot=prorabits prorabits.dart
```
```

Then, start the app with `dart prorabits args`, where args stands for optional arguments needed by the script.

# How it works...

A script snapshot is the byte representation of the app's objects in the memory (more precisely in the heap of the started isolate) after it is loaded, but before it starts executing. This enables a much faster startup because the work of tokenizing and parsing the app's code was already done in the snapshot.

# There's more...

This recipe is intended for server apps or command-line apps. A browser with a built-in Dart VM can snapshot your web app automatically and store that in the browser cache; the next time the app is requested, it starts up way faster from its snapshot. Because a snapshot is in fact a serialized form of an object(s), this is also the way the Dart VM uses to pass objects between isolates. The folder `dart/dart-sdk/bin/snapshots` contains snapshots of the main Dart tools.

# See also

+   - Occasionally, your app needs access to the operating system, for example, to get the value of an environment variable to know where you are in the filesystem, or to get the number of processors when working with isolates. Refer to the *Using isolates in the Dart VM* and *Using isolates in web apps* recipes, in *Chapter 8, Working with Futures, Tasks, and Isolates*, for more information on working with isolates.

# Getting information from the operating system

In this recipe, you will see how to interact with the underlying operating system on which your app runs by making system calls and getting information from the system.
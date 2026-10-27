# See also

+   - Solving problems when `pub get` fails
+   - Compiling your app to JavaScript (for `pub build`)
+   - Documenting your code from Chapter 2, Structuring, testing, and deploying an application
+   - Publishing your app to a pub (for pub publishing)
+   - Using snapshotting to start an app in Dart VM
+   - For additional information, refer to https://www.dartlang.org/tools/

# Solving problems when pub get fails

The pub package manager is a complex tool with many functionalities, so it is not surprising that occasionally something goes wrong. The `pub get` command downloads all the libraries needed by your app, as specified in the `pubspec.yaml` file. Running `pub get` behind a proxy or firewall used to be a problem, but it was solved in the majority of cases. If this still haunts you, look at the corresponding section at https://www.dartlang.org/tools/editor/troubleshoot.html.

# Getting ready

This recipe is especially useful when you encounter the following error in your Dart console while trying to open a project in Dart Editor during the pub get phase:

```
Pub install fails with 'Deletion failed'
```

# How to do it...

First try this; right-click on your project and select **Close Folder**. Then, restart the editor and open your project again. In many cases, your project will load fine. If this does not work, try the `pub gun` command:

+   1. Delete the pub cache folder from `C:\Users\{your username}\AppData\Roaming\Pub.`
+   2. Delete all the `packages` folders in your project (also in subfolders).
+   3. Delete the `pubspec.lock` file in your project.
+   4. Run `pub get` again from a command line or select **Tools** in the Dart Editor menu, and then select **Pub Get.**
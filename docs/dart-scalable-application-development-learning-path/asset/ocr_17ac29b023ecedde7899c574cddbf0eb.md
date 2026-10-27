# Debugging a Dart application

A good debugger is a key part of a productive development system. Let's step through our code so that we can look closely at what is going on.

To debug an application running in Dartium from WebStorm, we need to install a small browser extension to act as a bridge between the two applications.

> Follow the guide from JetBrains at the following web page:
> https://www.jetbrains.com/webstorm/help/using-jetbrains-chrome-extension.html

The main section to be concerned with is the "Installing the JetBrains Chrome extension" section. This only has to be done once.

+   1. Click on the first line of the `loadDocument` function.
+   2. Next, open the **Run** menu and choose **Toggle Line Breakpoint**. A red circle will appear to the right of the line:

![](bbox=[0.205, 0.455, 0.795, 0.595])

+   3. Select `index.html` in the **Project** tab, then open the **Run** menu and choose the `Debug index.html` menu.
+   4. Once Dartium opens, the `loadDocument` function should run and the breakpoint should hit. The **Debugger** tab will appear in WebStorm.
+   5. The **Debugger** tab shows the call stack and values of current variables. Under the **Run** menu and on the toolbar, the usual debug operations of **Step Into, Step Over,** and return are available.
+   6. Before we take any steps, hover the pointer over the return statement line. A tool-tip will appear to show that the string variable `readings` is currently set to null.
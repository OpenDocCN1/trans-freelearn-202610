6. In main(), we get information about the platform the app is being executed in; in our case, this displays {arch: x86-32, nacl_arch:x86-64, os: win} and we register rewriteText as a Click event handler:

```
void main() {
    chrome.runtime.getPlatformInfo().then((var m) {
        txtp.text = m.toString();
    });
    txtp.onClick.listen(rewriteText);
}
```

7. At each click event, we show a message with the number of times clicked in the <p> tag and we resize the window:

```
void rewriteText(MouseEvent e) {
    txtp.text = "Hey, you clicked ${n++} times on a Chrome packaged app!";
    resizeWindow(e);
}
```

```
void resizeWindow(MouseEvent e) {
chrome.ContentBounds bounds = chrome.app.window.current().
getBounds();
    bounds.width += boundsChange;
    bounds.left -= boundsChange ~/ 2;
    chrome.app.window.current().setBounds(bounds);
    boundsChange *= -1;
}
```

8. We cannot run this app from Dart Editor as it is; if you try it, you get the exception Unsupported operation: 'chrome.runtime' is not available.

9. Instead, we have to compile it to JavaScript. Open a command window and go to the web folder of your app. The easiest way to do this is to select the web folder in Dart Editor, click on the right mouse button and select Copy File Path. You can paste this path on your command line after cd and press Enter. Then, type the command dart2js chrome_pack.dart -o chrome_pack.dart.js.

10. The option -o allows you to give an output file name. Alternatively, from within Dart Editor, on the Dart file navigate to Tools | Pub Build.
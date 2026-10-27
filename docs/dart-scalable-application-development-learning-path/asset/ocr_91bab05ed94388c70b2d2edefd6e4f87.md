# Handling Web Applications

# How to do it...

Perform the following steps to create a Chrome app. The code for this recipe can be found in the project `chrome_pack`:

+   1. Start with creating a new Dart project and choose the **Chrome packaged application** template.
+   2. The Pub Get command starts automatically, and installs the Chrome package from the pub.
+   3. The first thing you will notice is the presence of a new file `manifest.json`, with the following initial content:

```
{
  "name": "Chrome pack",
  "version": "1",
  "manifest_version": 2,
  "icons": {"128": "dart_icon.png"},
  "app": {
    "background": {
      "scripts": ["background.js"]
    }
  }
}
```

We don't have to change anything here, but you can add a "description" tag if you like.

+   4. The `background.js` file indicates which page the application should start with:

```
chrome.app.runtime.onLaunched.addListener(function(launchData) {
  chrome.app.window.create('chrome_pack.html', {
    'id': '_mainWindow', 'bounds': {'width': 800, 'height': 600 }
  });
});
```

+   5. The `chrome_pack.html` file has a `<p>` tag with the ID `text_id` and references the script `chrome_pack.dart`. This script first has to import the Chrome package:

```
import 'dart:html';
import 'package:chrome/chrome_app.dart' as chrome;

int boundsChange = 100;
var txtp = querySelector("#text_id");
int n = 0;
```
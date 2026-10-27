# How it works...

3. Add the shadow DOM script to the startup HTML file, as follows:

```
<script src="packages/shadow_dom/shadow_dom.min.js"></script>
```

4. Use pub build either from the command line or from Dart Editor.

Step 2 will make sure that Angular can work with Shadow DOM in all browsers (for more information on Shadow DOM, refer to the How it works section in the Setting up an angular app recipe in this chapter).

> Instead of shadow_dom.min.js, you can use shadow_dom.debug.js while debugging.

Step 3 will generate all deployable files in your app's directory in a subfolder named build/web. For example, if we apply this to the angular_view app, we get a JavaScript file, angular_view.dart.js, of about 1.7 MB to start up together with angular_view.html. The following screenshot gives us an overview of the angular_view project:

![](bbox=[0.279, 0.433, 0.721, 0.785])

Overview of the project layout
It accomplishes this using declarative programming to build UI and wire software components so that you can concentrate on your application's logic and not on DOM manipulation. It uses a templating system with a number of so-called directives (starting with ng-) to specify customizable and reusable HTML tags and expressions that control the behavior of certain elements: in effect, you extend HTML with custom elements and attributes. Google uses Angular Dart to build internal applications. Each recipe explored in this chapter exposes a major component of Angular. The most up-to-date documents can be found at https://docs.angulardart.org/.

# Setting up an Angular app

This recipe is a preliminary step necessary for every other recipe in this chapter. It shows you how to make the Angular Dart functionality available to your app.

# How to do it...

+   1. There is no `angular` template yet in Dart Editor, so start your app from a web application (mobile friendly), and call it, for example, `angular_setup`. Clear the sample code from the `html` and `dart` files.
+   2. Add `angular` to the dependencies in `pubspec.yaml`. Saving will start the `pub get` procedure.
+   3. Also add `js` and `shadow_dom` to `pubspec.yaml`.
+   4. Add the shadow DOM script to the `html` file:
+   ```
+   <script
+     src="packages/shadow_dom/shadow_dom.min.js"></script>
+   ```
+   5. Also, include the Angular transformer:
+   ```
+   transformers:
+     - angular
+   ```
+   6. Provide the `ng-app` attribute in the `<html>` element.
+   7. Add the following statement to the top of your main Dart script:
+   ```
+   import 'package:angular/angular.dart';
+   import 'package:angular/application_factory.dart';
+   ```
+   8. In `main()`, insert the code `applicationFactory().run();`.
+   9. Provide the `ng-cloak` attribute in the `<body>` element.
+   10. Add the following section to your CSS file:
+   ```
+   [ng-cloak], .ng-cloak {
+     display: none !important;
+   }
+   ```
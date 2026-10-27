
## Handling Web Applications

In general, if you want to access a certain URL `urlxyz` through OAuth2 and you already have an auth token, use the following code:

```
var request = new HttpRequest();
request.onLoad.listen(...);
request.open(method, urlxyz);
auth.authenticate(request).then((request) => request.send());
```

### There's more...

The Dart team has made the oauth2 client library, which allows you to obtain OAuth2 authorization from a non-Google server. With it, a user is authenticated for your app, without having to store passwords on your website. After the user has been authenticated, your application has an oauth2 token for that user, which can be used to access other services. To start working with it in the code, add oauth2 to your pubspec.yaml dependencies and let the pub in Dart Editor install it, or invoke pub get on the command line. Then, add the following to your script:

```
import 'package:oauth2/oauth2.dart';
```

The way to go about this is a bit more involved, but with what you have learned now, you will be able to grasp the example code given at http://pub.dartlang.org/packages/oauth2.

To authenticate via OAuth2 from Facebook, Windows Live, or Google in a server-side application, use the pub package by Christophe Hurpeau.

### See also

- For more detailed information on OAuth2 and Google, see the article *Using OAuth2 to access Google APIs* at `https://developers.google.com/accounts/docs/OAuth2?csw=1`
- If you want to learn more about OAuth2, refer to `http://oauth.net/2/`

## Talking with JavaScript

If we take into account the enormous amount of JavaScript code and libraries that exist, and are still being developed, it is very important that we have a simple way to use JavaScript code from within Dart applications, in particular to get access from Dart to the JavaScript code that is running in the same web page. The earliest attempts used `window.postMessage`, and then a package called js was built. Because of the huge importance of this topic, the Dart team now has provided us with a core library, `dart:js`, to interoperate with JavaScript. This provides better performance, reduces the size of the compiled JavaScript file, and makes it also easier to use. Once `dart:js` is ready, the package js has been rewritten to use `dart:js` under the covers.

### How to do it...

Take a look at the project `js_interop`, as explained in the following steps:

- 1. To start using `dart:js` in our project, we have to import it in our code:
  
  ```dart
  import 'dart:js';
  ```

- 2. In `js_interop.html`, we declare a Dart script, and the JavaScript program `js_interop.dart` will look into the code of `interact.js`:
  
  ```html
  <script type="application/dart" src="js_interop.dart"></script>
  <script type="application/javascript"
    src="interact.js"></script>
  ```

- 3. The `interact.js` file contains the following code: a variable `jsvar`, a class `Person` with the properties name and gender, and the methods greeting and sayHello:
  
  ```javascript
  var jsvar = "I want Dart";

  function Person(name, gender) {
      this.name = name;
      this.gender = gender;
      this.greeting = function(otherPerson) {
          alert('I greet you ' + otherPerson.name);
      };
  }

  Person.prototype.sayHello = function () {
      alert ('hello, I am ' + this.name );
  };
  ```

- 4. First, we get the contents of a JavaScript variable:
  
  ```dart
  var dart = context['jsvar'];
  print(dart);  // I want Dart
  ```

- 5. Then, we make a `Person` object:
  
  ```dart
  var pers1 = new JsObject(context['Person'], ['An', 'female']);
  var pers2 = new JsObject(context['Person'], ['John', 'male']);
  ```

- 6. We access and set the properties using the following code:

```dart
print(pers1['name']); // An
print(pers2['gender']); // male
pers1['gender'] = 'female';
print(pers2['gender']); // female
```

- 7. We call the methods on the Person object:

```dart
pers1.callMethod('sayHello', []);
pers2.callMethod('greeting', [pers1]);
```

The preceding steps display alert windows with the messages **hello, I am An** and **I greet you An.**

- 8. Now we get the global object in JavaScript (normally a window) via `context`, and display an alert window with `callMethod`:

```dart
context.callMethod('alert', ['Hello from Dart!']);
```

- 9. Use `jsify` to create a JavaScript object and array:

```dart
var jsMap = new JsObject.jsify({'a': 1, 'b': 2});
print(jsMap); // [object Object]
var jsArray = new JsObject.jsify([1, 2, 3]);
print(jsArray); // [1, 2, 3]
```

### How it works...

The `dart:js` library provides Dart access to JavaScript objects in web applications, not in server applications. More specifically, it exposes wrapped or proxy versions of any JavaScript objects you access. This enables Dart to safely sandbox JavaScript away and prevents its problems from leaking into the Dart application. You can get and set properties and call JavaScript functions and methods on JavaScript objects, while conversions between Dart and JavaScript are taken care of as far as possible. At this moment, the bridge is not fully bidirectional; JavaScript has no access to Dart objects, but it can call Dart functions.

> Inclusion of the `script` tag in the HTML `<script src="packages/browser/interop.js"></script>` code is no longer needed.

The main type of object is `JsObject` with which we can reach out to JavaScript objects; in other words, we create a Dart proxy object to the JavaScript object. To get the global object in JavaScript (which is mostly window), use the top-level getter function `context`; this is used in step 8. However, `context` is also used to get the values of JavaScript variables, as shown in step 4.

You can create JavaScript objects as shown in step 5. Use the JsObject() constructor. This takes the name of a JavaScript constructor function and the list of arguments that it needs as arguments. As shown in step 6, we can use the [] index operator to get the value of properties and [] = to set them; instead of a numerical index, we use the property name string as the key. The seventh and eighth step demonstrate that we can call a JavaScript method on an object with callMethod, taking the name of the method and the list of its arguments as parameters. Finally, in step 9, we see that JsObject.jsify turns a Dart map into a JavaScript object using the keys as properties; the same method also turns a Dart list into a JavaScript array.

### There's more...

To be able to compare, we will now show the same code but rewritten with the js package. In js_interop2.html, we have the same JavaScript, but running together with the Dart script js_interop2.dart. We add the js package to our pubspec.yaml file as js:any, and let pub get do its magic. To make the package available to our Dart script, we add the following code to js_interop2.dart:

```
import 'package:js/js.dart' as js;
```

Rewriting the Dart code from js_interop.dart gets us the following output:

```
void main() {
  // getting a variable:
  var dart = js.context['jsvar'];
  print(dart); // I want Dart
  // making objects:
  var pers1 = new js.Proxy(js.context.Person, ['An', 'female']);
  var pers2 = new js.Proxy(js.context.Person, ['John', 'male']);
  // accessing and setting properties:
  print(pers1.name); // prints the whole object: [An, female]
  pers1.name = 'Melissa'; // change name property
  print(pers1.name); // Melissa
  // calling methods:
  pers1.sayHello.call(); // window: hello, I am Melissa
  pers2.greeting.call(pers1); // window: I greet you Melissa
  // getting the global object in JavaScript via context
  js.context.alert('Hello from Dart via JavaScript');
  // using jsify:
  var jsMap = js.map({'a': 1,'b': 2});
  print(jsMap); // [object Object]
  var jsArray = js.array([1, 2, 3]);
  print(jsArray); // [1, 2, 3]
}
```

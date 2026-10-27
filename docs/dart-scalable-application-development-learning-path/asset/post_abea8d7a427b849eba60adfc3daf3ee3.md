
```dart
var Title = new HeadingElement.h1();
Title.text = "Quake Chart";
document.body.nodes
  ..clear()
  ..add(Title)
  ..add(container);

LineChart chart = new LineChart(container);
chart.draw(table, options);
```

Once the dataset is ready, the HTML elements for the page can be put together, and the `LineChart` object that is created is given a reference to the object to add it on the page; in this case, a `div` element container (refer to the `modern_chart` package documentation for complete details).

The output of the previous code is shown in the following chart:

![](img/52c05b53a56abd53584e4fed738ed509_235_0.png)

You may find working in Dartium to be an odd experience. It is a good browser, but you may prefer your daily web browser, such as Internet Explorer, Firefox, or Chrome, with your carefully-managed settings and collection of extensions. Using Dart-to-JavaScript (dart2js) before testing on other browsers interrupts the edit and refresh cycle. Ideally, Dart would run in any web browser.

> Dartium is still going to be around for a long time; however, there is a project underway by the Dart team called the Dart development compiler. This new compiler has two goals, the first being to allow a smooth edit and refresh development experience on all browsers. A modular (incremental) compilation is used to aid a fast edit/refresh cycle. Find out more about the compiler project at https://github.com/dart-lang/dev_compiler.
>
> The second goal of the compiler is to create a human-readable JavaScript format so that the output can easily be debugged. It initially supports a subset of the Dart language that is statically type checked. The type system has a different approach to dart2js; it allows a more direct and readable mapping of the Dart language to JavaScript. If you are developing in Firefox, then you will want to be able to debug your Dart code in it so that you do not have to step through the obfuscated code.

## Exporting to CSV

CSV is a format that just keeps going thanks to its easy import into spreadsheets. The implementation of this report is found in the `report_csv.dart` file, and it retrieves the 50 most recent entries using the following URL:

```
http://127.0.0.1:8080/api/quake/v1/recent/50
```

> CSV is a text format, and the fields are cleared of any comma characters so as not to have extra delimiters.

Let's consider the following code:

```dart
performExport(MouseEvent event) async {
  String json = await HttpRequest.getString(jsonSrc);
  List<String> items = JSON.decode(json);
  String csvexport = "Type, Time, Place, Magnitude\r\n";

  items.forEach((String featureJSON) {
    Map feature = JSON.decode(featureJSON);
    String type = feature['properties']['type'];
```

## Module 2

## **Mastering Dart**

*Master the art of programming high-performance applications with Dart*

## 1. Beyond Dart's Basics

Dart is a very young computer language with many interesting features. Dart is a class-based, object-oriented language with optional types, and it can help you write very powerful programs. In this chapter, we will cover the following topics:

- Modularity and a namespace
- Functions and closures in different scopes
- Classes and mixins
- Methods and operators

### Modularity and a namespace

Complex things are the foundation of our world. To understand the complexity of the things around us, it is necessary to understand the parts that make them up. The evolution of complex things is due to functional and behavioral modularity. Functional modularity is the composition of smaller independent components with clear boundaries and functions. Behavioral modularity is mainly about traits and attributes that can evolve independently.

*Modularity* is nothing new. Earlier, product manufacturers figured out ways to increase the output and quality of the product, while still managing to reduce the cost pressures. They accomplished this through modularity. Modular design can be seen in automotive industry, buildings, and many other industries. Henry Ford introduced the notion of modularity in his assembly line with standardized and interchangeable parts. As a result, he reduced the production cycles and costs to achieve the mass production of his automobiles. A lot of these concepts are still used by many companies today.

The following is an implicitly named library in which all the resources from dart:html are made available within the scope of our library with the prefix dom:

```dart
/**
 * Implicitly named library.
 * The dart:core library is automatically imported.
 */
import 'dart:html' as dom;
```

```dart
/**
 * Get [Element] by [id].
 */
dom.Element getById(String id) => dom.querySelector('#$id');
```

The library namespace makes sense only in the Dart environment.

> The code that is compiled to JavaScript loses all the library information.

Dart implements encapsulation through privacy. Each member or identifier of a library has one of the two levels of access: private or public. Private members are visible only inside the library in which they are declared. Conversely, members with public access are visible everywhere. The difference between them is the underscore prefix (`_`), as shown in the following code:

```dart
// Animation library.
library animation;

// Class publicly available everywhere.
class Animation {
  // ...
}

// Class visible only inside library.
class _AnimationLibrary {
  // ...
}

// Variable publicly available everywhere.
var animationSpeed;
```

The preceding code shows an animation library with two classes and one variable. The `Animation` class and the `animationSpeed` variable are public, and therefore visible outside the library. The `_AnimationLibrary` class is private and it can be used only in the library.

Now we hide the Animation class, but all the other public members in the namespace animation are still available, as seen in the following screenshot:

![](img/52c05b53a56abd53584e4fed738ed509_246_0.png)

As you can see, the members of the imported library become invisible. This happens because the library exports members from the public namespace. It is possible to re-export the imported library with the `export` statement if necessary. This `export` statement can be managed with `show` and `hide` as it was for the `import` statement, as shown in the following code:

```dart
library animation.css;

import 'animation.dart' as animation;
export 'animation.dart' show Animation;

class CssAnimation extends animation.Animation {
  // ...
}
```

The preceding code shows the `animation.css` library. We export the `Animation` class as part of the library namespace. Let's take a look at how we can use them:

![](img/52c05b53a56abd53584e4fed738ed509_246_1.png)

There are the exported `Animation` and original `CssAnimation` classes available for use in our main code. Without the `export`, the `Animation` class would be inaccessible in the main code.

### Functions and closures in different scopes

I like Dart because everything is an object. Functions are first-class citizens because they support all the operations that are generally available to other types. This means each function has the following properties:

- They can be named by a variable
- They can be passed as an argument to a function
- They can be returned as the result of a function
- They can be stored in data structures
- They can be created in any scope

Let's see where and how we can use functions as usual or as first-class citizens.

#### Naming functions with a variable

Naming functions by variable means that we can create a reference to a function and assign it to a variable, as shown in the following code:

```dart
library function_var;

// Returns sum of [a] and [b]
add(a, b) {
  return a + b;
}

// Operation
var operation;

void main() {
  // Assign reference to function [add]
  operation = add;
  // Execute operation
  var result = operation(2, 1);
  print("Result is ${result}");
}
```

Here is the result of the preceding code:

```
Result is 3
```

We have the `add` function and the `operation` variable. We assign the reference of the `add` function to a variable and call the variable as a function later.

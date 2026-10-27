
## What is the impact on performance?

Now we should talk about the performance impact of using different approaches across several modern web browsers. The algorithm must perform all the following actions:

- It should create 10000 DIV elements
- Each element should be added into the same DIV container
- Each element should be updated with one style
- All elements must be removed one by one

This algorithm must be implemented in the following solutions:

- The pure jQuery solution in JavaScript
- The jQuery solution calling via `JProxy` and `dart:js` from Dart
- The pure Dart solution based on `dart:html`

We implemented this algorithm on all of them, so we have a chance to compare the results and choose the champion. The following HTML code has three buttons to run independent tests, three paragraph elements to show the results of the tests, and one DIV element used as a container. The code is as follows:

```html
<div>
  <button id="run_js" onclick="run_js_test()">Run JS</button>
  <button id="run_jproxy">Run JProxy</button>
  <button id="run_dart">Run Dart</button>
</div>

<p id="result_js"></p>
<p id="result_jproxy"></p>
<p id="result_dart"></p>

<div id="container"></div>
```

The JavaScript code based on jQuery is as follows:

```javascript
function run_js_test() {
    var startTime = new Date();
    process_js();
    var diff = new Date(new Date().getTime() -
        startTime.getTime()).getTime();
    $('#result_js').text('jQuery took ' + diff +
        ' ms to process 10000 HTML elements.');
}

function process_js() {
    var container = $('#container');
    // Create 10000 DIV elements
    for (var i = 0; i < 10000; i++) {
        $('<div>Test</div>').appendTo(container);
    }
    // Find and update classes of all DIV elements
    $('#container > div').css("color","red");
    // Remove all DIV elements
    $('#container > div').remove();
}
```

The main code registers the click event listeners and calls the function run_dart_js_test. The first parameter of the run_dart_js_test function must be a function which we will investigate. The second and third parameters are used to pass the selector of the result element and the test title:

```dart
void main() {
    querySelector('#run_jproxy').onClick.listen((event) {
        run_dart_js_test(process_jproxy, '#result_jproxy', 'JProxy');
    });
    querySelector('#run_dart').onClick.listen((event) {
        run_dart_js_test(process_dart, '#result_dart', 'Dart');
    });
}

run_dart_js_test(Function fun, String el, String title) {
    var startTime = new DateTime.now();
    fun();
    var diff = new DateTime.now().difference(startTime);
    querySelector(el).text = '$title took ${diff.inMilliseconds} ms to process 10000 HTML elements.';
}
```

Here is the Dart solution based on JProxy and dart:js:

```dart
process_jproxy() {
    var container = $('#container');
    // Create 10000 DIV elements
    for (var i = 0; i < 10000; i++) {
        $('<div>Test</div>').appendTo(container.object);
    }
    // Find and update classes of all DIV elements
    $('#container > div').css("color","red");
    // Remove all DIV elements
    $('#container > div').remove();
}
```

Finally, a pure Dart solution based on dart:html is as follows:

```dart
process_dart() {
  // Create 10000 DIV elements
  var container = querySelector('#container');
  for (var i = 0; i < 10000; i++) {
    container.appendHtml('<div>Test</div>');
  }
  // Find and update classes of all DIV elements
  querySelectorAll('#container > div').forEach((Element el) {
    el.style.color = 'red';
  });
  // Remove all DIV elements
  querySelectorAll('#container > div').forEach((Element el) {
    el.remove();
  });
}
```

All the results are in milliseconds. Run the application and wait until the web page is fully loaded. Run each test by clicking on the appropriate button. My results of the tests on Dartium, Chrome, Firefox, and Internet Explorer are shown in the following table:

| Web browser | jQuery framework | jQuery via JProxy | dart:html library |
| Dartium | 2173 | 3156 | 714 |
| Chrome | 2935 | 6512 | 795 |
| Firefox | 2485 | 5787 | 582 |
| Internet Explorer | 12262 | 17748 | 2956 |

Now, we have the absolute champion—the Dart-based solution. Even the Dart code compiled into JavaScript code to be executed in Chrome, Firefox, and Internet Explorer works quicker than jQuery (four to five times) and much quicker than dart:js and JProxy class-based solutions (four to ten times).

## Summary

Let me finish the story of Dart to JavaScript interoperation to highlight our expertise.

The core set of Dart libraries includes `dart:js` to help you interoperate between the Dart and JavaScript code. The `dart:js` library converts the original JavaScript objects, functions, and collections with the help of `JsObject`, `JsFunction`, and `JsArray`. It supports automatic type conversion in both directions. Dart supports a small subset of types, transferring directly from JavaScript types. All other JavaScript types are converted to Dart types with the help of a proxy. Dart Map and Iterable collections could be translated into JavaScript collections with the `jsify` constructor of `JsObject`.

We compared `jQuery`, `JProxy`, and `dart:js` with the Dart code based on the `dart:html` solution to identify who is quicker than the others. The `dart:html` library-based solution is the unbeatable champion and hero of this chapter.

In the next chapter, we will talk about how i18n and l10n access can be embedded into our code to help design and develop web applications that enable easy localization for different cultures, regions, and languages.

## 8 Internationalization and Localization

If you are planning to work with multiple languages, you need to add internationalization support to your web applications. We will see how the i18n and l10n access can be embedded in our code to help design and develop web applications that enable easy localization for different cultures, regions, and languages. The topics that will be covered in this chapter are as follows:

- The key principles
- The Intl library
- How to internationalize a web application
- How to extract messages
- How to use Google Translator Toolkit
- How to use translated messages

### The key principles

The development of globalized software is not a simple task. In general, the standard development process to create globalized software includes the following steps:

- Internationalization that covers designing and developing web applications
- Localization that covers translating and customizing web applications for a specific locale

We will start designing and developing globalized software with the Intl library from the intl package available on the pub manager and follow some rules that will help us to easily translate and customize our application.

### Executable code versus User Interface

All executable code must be separated from the programming code that implements the **User Interface (UI)**. Also, code that describes the UI elements and the layout of the UI elements must be kept separate from the code that implements and manages them.

### Numbers and dates

Various cultures have different ways to represent numbers and dates. You must avoid converting numbers and dates into strings directly.

![](img/52c05b53a56abd53584e4fed738ed509_416_0.png)

> Converting numbers and dates must be done with special formatters.

### Messages

Often, messages that contain individual pieces of text are used together to create complete sentences. In the process of localization, these pieces of text might go together in a different order. Using the message method of the Intl class allows you to display messages as simple expressions.

### Measuring units and currencies

Measuring units such as meters and miles and currencies such as USD and Euro are ubiquitous and depend on the locale. The NumberFormat class contains special constructors for the quick creation of frequently used patterns that can be very handy to measure units and currencies.

### Text input and layout

The size of the text on the screen is one of the biggest problems that affect programmers who develop globalized software. The main reason for this is that any assumption about the width of the text, the direction of its flow, and its position on the screen, if incorrect, can hamper well-structured layouts in a flash. Using the BidiFormatter class helps manage messages that contain text in both text directionalities.

### Formatting date and time

The locale determines how the date and time must be displayed. The DateFormat class has a big set of naming patterns that is useful to avoid mistakes and display data in the correct format.

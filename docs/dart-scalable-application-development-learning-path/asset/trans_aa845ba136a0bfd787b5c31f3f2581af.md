

## 关于对话框

应用程序的标准关于框包含少量信息和一个指向网站的链接。此对话框在视觉上与警报框完全相同，只是增加了超链接，并且由于这涉及更改对话框界面元素，因此将在名为 AboutDialog 的新类中实现：

![](img/52c05b53a56abd53584e4fed738ed509_51_0.png)

这是通过 TextEditor 对象的 showAbout 方法创建的：

```dart
void showAbout(MouseEvent event) {
    AboutDialog textEditorAbout = new AboutDialog(AppTitle,
    "TextEditor for the Web", "http://www.packtpub.com", "Homepage",
    300, 200);
    textEditorAbout.show();
}
```

AboutDialog 框继承自 Dialog 类并通过 super 调用 Dialog 类的构造函数，该构造函数调用构造函数以形成对象的[content]

## The About dialog box

The standard About box for an application contains a small amount of information and a link to a website. This dialog is visually identical to the Alert box apart from the addition of the hyperlink, and as this involves changing the dialog interface elements, this will be implemented in a new class called AboutDialog:

![](img/52c05b53a56abd53584e4fed738ed509_51_0.png)

This is created from the showAbout method of the TextEditor object:

```dart
void showAbout(MouseEvent event) {
    AboutDialog textEditorAbout = new AboutDialog(AppTitle,
    "TextEditor for the Web", "http://www.packtpub.com", "Homepage",
    300, 200);
    textEditorAbout.show();
}
```

The AboutDialog box inherits from the Dialog class and calls the constructor of the Dialog class via super, which calls the constructor to form the foundation of the object:

```dart
AboutDialog(String titleText, String bodyText, this.linkUrl, this.linkText, int width, int height)
    : super(titleText, bodyText, width, height, true, false) {
```

The elements for the box are created and added to the nodes member of the content DivElement:

```dart
content..nodes.insert(0, new BRElement())
  ..nodes.insert(0, new BRElement())
  ..append(new BRElement())
  ..append(new BRElement())
  ..append(link)
  ..style.textAlign = "center";
```

The cascade operator keeps the code compact and focused on setting up the different components of the dialog.

## Using the confirmation dialog box

This dialog presents OK and CANCEL options, and calls a specified function if the former option is clicked. This is used in the TextEditor class clearEditor method:

![](img/52c05b53a56abd53584e4fed738ed509_52_0.png)

This is implemented by using the base Dialog class directly:

```dart
void confirm(String dlgTitle, String prompt, int w, int h, Function action) {
    Dialog dlg = new Dialog(dlgTitle, prompt, w, h);
    dlg.show(action);
}
```

The application can configure and display this dialog with a single line:

```dart
confirm(AppTitle, "Are you sure you want to clear the text?",
400, 120,
performClear);
```

A function is passed to the show method that is performed if the user presses the OK button.

## Counting words using a list

Word count is an important feature for students, technical writers, and competition entrants. Our customers see this as a required feature, too.

In the event handler, we want to calculate the word count and throw the dialog on screen for the user to see. This is a simple, short-running task, so the implementation can exist directly in the event handler. As the output is simple, the alert dialog can be used.

To determine the word count, Dart's List data structure and String classes can be used. The first step is to remove any punctuation characters that may affect our count. This is declared in custom_dialogs.dart as an unmodifiable const string as it can be used for other word-based features:

```dart
const String punctuation = ",.-!\"";
```

This is used in the showWordCount method of the TextEditor class.

Once the punctuation is removed, the string can be split into a List. Dart supports generics, so the types of generic data structure can be optionally specified, in this case with <String>. The list of words is then filtered to clean out any non-word entries remaining.

Once we have obtained the list of words, the final word count can be obtained by using the length property of the list.

## The Word Frequency feature

Our customers would like a feature to help with writing quality. Overuse of particular words can make a text repetitive and hard to read. To determine the frequency of word usage, Dart's Map (sometimes called an associative array or dictionary) data structure class, and String features can be used:

![](img/52c05b53a56abd53584e4fed738ed509_54_0.png)

Depending on the number of different words used, the content areas may be allowed to scroll, as the dialog cannot grow to the size of any data.

Generics are used again to specify the types for the key and value; in this case, String for the key and int for the value:

```dart
Map<String, int> wordFreqCounts = {};
String out = "";

for (var character in punctuation.split(''))
    text = text.replaceAll(character, " ");
text = text.toLowerCase();

List<String> words = text.split(" ");
words
  ..removeWhere((word) => word == " ")
  ..removeWhere((word) => word.length == 0)
  ..forEach((word) {
    if (wordFreqCounts.containsKey(word))
      wordFreqCounts[word] = wordFreqCounts[word] + 1;
    else wordFreqCounts[word] = 1;
  });

wordFreqCounts
  .forEach((k, v) => out += ("<tr><td>$k</td><td>$v</td></tr>"));
```

The processing of the text is similar to the word count, where a list of individual words is built up. Once the list is filtered, forEach is used to iterate through the list and count the occurrences of each word. The forEach method is used again to build the output HTML, with the map's key and value parameters being passed to the function.

## Understanding the typing of Dart code

The types used so far for the Map and List help the design-time tools, such as the Dart Analyzer and WebStorm, which allow us to write code by providing the code completion and validating assignments.

When run in Dartium with the checked mode enabled (the default setting), types are checked at runtime. In the final environment, most likely a web browser such as Firefox or Internet Explorer, the typing information is not used and does not affect the execution speed of the application.

Dart coding recommendations advise using types on all public-facing code. For example, a class in a package can use private var variables, but methods require and return typed data. This aids developers calling the code in understanding what is required, and helps create better documentation.

## The file download feature

It can be very useful to have an actual file, so we will add a feature to let the user download and save the contents of the editor to their local device. This is possible with the custom data attributes feature in HTML5, which supports text as one of the formats:

```dart
void downloadFileToClient(String filename, String text) {
  AnchorElement tempLink = document.createElement('a');
  tempLink
    ..attributes['href'] =
    'data:text/plain;charset=utf-8,' + Uri.encodeComponent(text)
    ..attributes['download'] = filename
    ..click();
}
```

The process is straightforward: an anchor element is created and its target is set as the encoded text data. The click event is then fired and the download proceeds as if it were being downloaded from a web server.

## The clock feature

Everyone has deadlines, so keeping an eye on the clock is important. The dart:async package can help with this task by using a Timer class to trigger and update. This can be carried out in one line in main.dart file of the TextEditor project:

```dart
new Timer.periodic(new Duration(seconds: 1),
    (timer) => querySelector("#clock").text =
        (new DateFormat('HH:mm')).format(new DateTime.now())
);
```

The Timer constructor being called here (Timer.periodic) is a specially named constructor; in this case, the periodic version is being used. There are numerous variations of Timer objects. Named constructors allow the classes to have instances easily created for a particular purpose as they do not require additional initialization configuration before they are ready to use.

A periodic Timer will fire an event every duration period; in this case, one second. The remainder of the line declares the function that is called. Dart has a shorthand of => for functions that are a single line (which are named arrow functions), thus avoiding the use of curly brackets. The querySelector function is used to get a reference to the element on the web page with an ID of clock where the time will be displayed.

The current time is obtained and formatted into hours and minutes for display. As the clock will run for the lifetime of the application and does not require any interaction, no variable is required to store a reference to the object.

That was a busy line of code, and very powerful, but we are not finished examining it yet! There has been no extra thread or process created. This is not a user-initiated event. We will take a look at the what and when of the Dart task queue.

## Executing Dart code

The internal architecture of the Dart VM has a single-threaded execution model that is event-driven. In Dart terminology, this is called an isolate. The execution of a Dart program places each event (for example, a function call) into one of two queues. The VM then takes the next event and executes it.

The first queue is the main event queue, and the second is called the microtask queue. This second queue is a higher priority, but is reserved, as the name implies, for short, small tasks, such as creating a new event or setting a value:

![](img/52c05b53a56abd53584e4fed738ed509_57_0.png)

This execution detail is hidden from the Dart developer for the most part. However, it is important to understand that execution of Dart code is highly asynchronous. The aim of this is to allow applications to continue to be responsive, while still performing longer tasks such as calculations or accessing remote resources.


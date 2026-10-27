```
}

@override
String toHtml() {
  return "<h1>$content</h1>";
}

}
```

If a method is not provided an implementation, then the Dart analyzer will warn you of 'Missing concrete implementation'.

The other core page elements follow the same pattern. To create a report, only a few lines of code are required, which are as follows:

```
var rep = new Report('Sample');
rep..addSection(new Title("Sample Report"))
    ..addSection(new Paragraph("This is a paragraph"))
    ..addSection(new Pagebreak())
    ..addSection(new Paragraph("This is a paragraph too"));
```

The Report object can be found in the reports_base.dart file in the Reports project, with its most important generate method being generated. This method iterates all the page element objects to build up the HTML report that is stored in the output field, as follows:

```
bool generate() {
  output = new StringBuffer();
  allContent.forEach((content){
    output.write( content.toHtml() );
  });
  generatedTimestamp = new DateTime.now();
  return true;
}
```

A StringBuffer object is used to build up the HTML report. This class is more efficient than appending a number of strings.

Strings are stored internally as an immutable format in Dart (as in most programming languages), and updating a string really entails building an entirely new string object. The StringBuffer object only creates a String object when the toString method is called.
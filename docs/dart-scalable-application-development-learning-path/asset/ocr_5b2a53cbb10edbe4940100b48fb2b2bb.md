This is a straightforward method call with the HTML string for the page, so why is a second parameter required? This is a security measure to ensure that the user-supplied content does not claim any undesired HTML string or script when being rendered in the display. Consider the following code snippet:

```
class ReportSanitizer implements NodeTreeSanitizer {
  void sanitizeTree(Node node) {}
}
```

The ReportSanitizer class is a **No Operation (NOP)** implementation, as the HTML is known to be from a trusted source.

# # Report classes

A report is made of a series of objects in a list, and the page elements are to be based on a similar interface so that the new page elements can be added to the package without having to change the display or export functions.

The page elements are implemented in the `report_elements.dart` file, which is part of the reports package project.

The core class will be called `Element`, and as it is to be used as an interface for other classes, it will be declared as follows:

```
abstract class Element {
  void setContent(var content);
  String toHtml();
}
```

This class is declared as abstract and provides no implementation for the methods.

To demonstrate this, we will consider the `Title` page element class, which implements the element (the methods have the `@override` annotation, which provides feedback to code analysis tools and other developers that the method is an implementation of a base class) as follows:

```
class Title implements Element {

  String content;

  Title(this.content) {}

  @override
  void setContent(var content) {
    String input = content.toString();
    content = input;
```
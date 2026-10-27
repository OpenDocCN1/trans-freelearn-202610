2. When the following code is added to submit.dart, clicking on the no button for a longer duration makes the Dart site appear:

```
import 'dart:html';

void main() {
  querySelector('#form1').onSubmit.listen(submit);
}

submit(Event e) {
  e.preventDefault();
  // code to be executed when button is clicked
}
```

# How it works...

In the first step, when the submit button is pressed, the browser sees that the method is POST. This method collects the data and names from the input fields and sends it to the URL specified in action to be executed, which only shows the Dart site in our case.

To prevent the form from posting the data, make an event handler for the onSubmit event of the form. In this handler code, e.preventDefault(); as the first statement will cancel the default submit action. However, the rest of the submit event handler (and even the same handler of a parent control, should there be one) is still executed on the client side.

# Dynamically inserting rows in an HTML table

When displaying data coming from a database, you often don't know how many data records there will be. Our web page and the HTML table in it have to adapt dynamically. The following recipe describes how to do this.

# How to do it...

Look at the html_table application. The web page contains two <table> tags:

```
<table id="data"></table>
<table id="jobdata"></table>
```
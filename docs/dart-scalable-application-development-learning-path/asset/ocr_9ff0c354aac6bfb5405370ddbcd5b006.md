In step 3, you could call a linter instead of an ordinary build by replacing this line, as shown in the following code:

```
main(args) {
  lint(entryPoints: ['web/index.html'], options: parseOptions(args));
}
```

This will display more extensive syntax or usage warnings in your code.

# Binding and repeating over a list

In this recipe, we show you how the data of a list can be displayed in a Polymer component. So we perform data binding from the code to the UI here as well as in the following recipe. You can find the code for this recipe in the project databinding_list.

# How to do it...

+   1. The script starts from web\index.html, where a component with the name pol-list is imported through the line:
+   <link rel="import"href="pol_list.html">
+   
+   From this, we know that the component is defined in pol_list.html, and the code behind it is in a file named pol_list.dart. For a discussion of the other tags, see the previous recipe.

+   2. We define a list of companies that we want to display in the file pol_list.dart:

```
import 'dart:html';
import 'package:polymer/polymer.dart';

@CustomTag('pol-list')
class Pollist extends PolymerElement {
  final List companies = toObservable(['Google', 'Apple', 'Microsoft', 'Facebook']);

  Pollist.created() : super.created() {
    companies.add('HP');
  }

  addcompanies(Event e, var detail, Node target) {
    companies.add('IBM');
    companies.add('Dell');
  }
}
```
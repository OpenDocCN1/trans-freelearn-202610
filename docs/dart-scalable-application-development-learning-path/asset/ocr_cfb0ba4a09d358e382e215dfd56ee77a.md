The input controls are one TextInputElement and two TextAreaElements. The placeholder property, available in modern web browsers, will be used to guide the user on what to enter, and, as a bonus, they are convenient space savers, as we do need to add extra elements to label the inputs:

```
String className = name.value;
String fieldsSrc = "";
String methodsSrc = "";

List<String> classFields = fields.value.split("\n");
List<String> classMethods = methods.value.split("\n");

classFields.forEach((field) => fieldsSrc += "    var $field;\n");
classMethods.forEach((method) => methodsSrc += "    $method() {};\n");
```

The text is retrieved from the controls and split by line into lists. The method forEach is then used to traverse the list and build the source code.

# Constructing the class

The template of a simple class in the GenClassDialog makeClass method has placeholders for the generated source code of the fields and methods. This class code template is declared as a multi-line string using three double-quote characters. Dart supports string interpolation, which helps when putting values of variables into strings:

```
result = """  // The $className class.
class $className {

$fieldsSrc

    $className() {}

$methodsSrc
}
""";
```

Variables can be embedded in strings prefixed by a $ sign. These are then replaced with the actual value of the variable when the string is used.

It is also possible to insert more complicated code, such as an objects property, "The word is ${text.length} characters long". Note the use of curly brackets around the expression being evaluated.
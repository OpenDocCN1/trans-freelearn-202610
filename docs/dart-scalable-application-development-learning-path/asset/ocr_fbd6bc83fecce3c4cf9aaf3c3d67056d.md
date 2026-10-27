# Structuring, Testing, and Deploying an Application

An alternative structure for an app with both client and server components can be placed on top of the previous structure:

+   - `client`
+   - `server`
+   - `core` (or shared)

The `https://www.dartlang.org/tools/pub/package-layout.html` link on the Dart site contains some additional information.

# See also

+   - You might also want to read the *Publishing and deploying your app* recipe in this chapter

# Using a library from within your app

As indicated in the previous recipe, every kind of app can contain a `lib` folder, which at the very least contains the model classes. These model classes are very important because they form the backbone of your project, so they must be accessible in your entire application. You can do this by placing them at the top in a `lib` folder, or even better in the lib/model. This central position will also make them stand out and easy to find for other readers of your code.

# How to do it...

Take a look at the structure of the `bank_terminal` project. The model classes `Person` and `BankAccount` are placed in the `lib\model` folder. Give your project a name in the `pubspec.yaml` file:

```
name: bank_terminal
```

Then, use the same name for the library script you created in the `lib` folder `bank_terminal.dart`, which contains the following code:

```
library bank_terminal;

import 'dart:convert';

part 'model/bank_account.dart';
part 'model/person.dart';
```
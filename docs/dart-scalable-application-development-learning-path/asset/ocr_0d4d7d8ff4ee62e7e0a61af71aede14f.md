# How it works...

The `dart2js` file utilizes a tree-shaking feature; only code that is necessary during execution is retained, that is, functions, classes, and libraries that are not called are excluded from the produced `.js` file. The minification process further reduces the size by replacing the names of variables, functions, and so on with shorter names and moving code around to use a few lines.

# There's more...

Be careful when you use reflection.

# More Information Section 1

Using reflection in the Dart code prevents tree shaking. So only import the `dart:mirrors` library when you really have to. In this case, include an `@MirrorsUsed` annotation, as shown in the following code:

```
library mylib;

@MirrorsUsed(targets: 'mylib')
import 'dart:mirrors';
```

In the previous code, all the names and entities (classes, functions, and so on) inside of `mylib` will be retained in the generated code to use reflection. So create a separate library to hold the class that is using mirrors.

> Make sure your deployment web server uses `gzipping` to perform real-time HTTP compression.

# See also

+   You might want to consult the *Using Reflection* recipe in *Chapter 4, Object Orientation.*

# Making a system call

A fairly common use case is that you need to call another program from your Dart app, or an operating system command. For this, the abstract class `Process` in the `dart:io` package is created.
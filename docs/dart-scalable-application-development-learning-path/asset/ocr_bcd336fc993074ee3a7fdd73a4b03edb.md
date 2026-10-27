# Adding a local package reference

To reference the library, go to the TextEditor project's pubspec.yaml and open it in the editor. Click on the source tab to see the plain yaml text file. There is currently no graphical interface for the local reference, so it has to be added by hand:

```
dependencies:
browser: any
intl: any
simple_dialog:
  path: ..\simple_dialog
```

The path is relative to the root project directory, and, as this is a local filesystem reference, the path separator must match the operating system, so we use \ (backslash) for Windows and / (forward slash) for Linux and Mac.

The package is then referenced in editor.dart with a package prefix:

```
import 'dart:convert';
import 'dart:html';
import 'package:simple_dialog/simple_dialog.dart';
```

It operates in exactly the same way as a remotely installed package, with the added advantage that it automatically refreshes if changes are made.

![](bbox=[0.21, 0.56, 0.26, 0.6])

> To avoid any problems with relative paths (which make many people's heads hurt!), if possible, keep projects in the same directory so that the relative path is always the same. Alternatively, full paths can be used, although this may introduce problems when the code is on different computers.
> To verify that the path is right, look in the Project tab, expand the Packages folder, and the package should be listed with its source.
> Path dependencies are useful for local development, but will not work when sharing projects with the wider world.
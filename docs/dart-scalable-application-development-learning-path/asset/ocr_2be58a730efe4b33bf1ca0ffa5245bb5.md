```
```dart
String toString() => removalBuildLabel;
}

@Temporary('Build1')
class TempWidgetTxt {}

@Temporary('Build2')
class TempWidgetWithGFX {}

void main() {
  ClassMirror classMirror1 = reflectClass(TempWidgetTxt);
  ClassMirror classMirror2 = reflectClass(TempWidgetWithGFX);

  List<InstanceMirror> metadata1 = classMirror1.metadata;
  print(metadata1.first.reflectee);

  List<InstanceMirror> metadata2 = classMirror2.metadata;
  print(metadata2.first.reflectee);
}
```
```

The Temporary constructor allows a label to be set. This field must be final (a single-assignment variable or field) if this class is to be used as an annotation. The main function calls `reflectClass` to obtain the details of the classes and then accesses this metadata and stores it in a list. The toString method allows the metadata to be the output via `print`:

Build1
Build2

Annotations are used extensively in the core Dart APIs, and in packages, they are used as the basis of frameworks, for example, web applications. They are not an obscure feature of the core SDK.

# # Translating the user interface text

My first ever professional software release did not involve any creation of code. It was a language update release for an existing package, and it was just as satisfying to mail out the CD-ROM (this was a while ago!) as something I had coded from scratch. Thankfully, it made me famous in Norway!

The translation of a software application is critical for its acceptance in some markets. This is even more true in the world of web applications. Dart has a package called `intl` that is used to help bring your applications to a global audience.
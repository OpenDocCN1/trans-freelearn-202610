# How to do it...

The program is launched from the command line in the `bin` folder (or in Dart Editor with a Managed Launch with Script arguments `file1.txt file2.txt file.txt`) as `dart concat.dart file1.txt file2.txt file.txt`, where `file1.txt` and `file2.txt` are the files to be concatenated (there can be two or more files) into `file.txt`. The following is the code to perform this:

```
```dart
import 'dart:io';
import 'package:args/args.dart';

ArgResults argResults;
File output;

void main(List<String> arguments) {
  final parser = new ArgParser();
  argResults = parser.parse(arguments);
  final outFile = argResults.rest.last;
  List<String> files = argResults.rest.sublist(0, argResults.rest.length - 1);
  if (files.isEmpty) {
    print('No files provided to concatenate!');
    exit(1);
  }
  output = new File(outFile);
  if (output.existsSync()) {
    output.delete();
  }
  concat(files);
}

concat(List<String> files) {
  for (var file in files) {
    var input = new File(file);
    try {
      var content = input.readAsStringSync();
      content += "\n";
      output.writeAsStringSync(content, mode: FileMode.APPEND);
    } catch (e) {
      print("An error $e occurred");
    }
  }
}
```
```
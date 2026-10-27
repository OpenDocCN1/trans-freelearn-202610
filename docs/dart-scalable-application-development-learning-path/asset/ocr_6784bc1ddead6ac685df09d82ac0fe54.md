# Starting the Text Editor

You want your team members to be able to build and use your new Uber package without dependency horrors! Likewise, you want the package you download off the Internet to "just work" and reduce the effort required to write your application.

To look after all these packages, Dart has a tool called pub. This is a Swiss Army knife tool that helps create, manage, and deploy your programs and packages. The main home for Dart packages is the website https://pub.dartlang.org, where numerous open source libraries are published.

> 💡 If you are curious to know the origin of the name, "pub" is British slang for a bar where people drink and play a game of darts.

# A look at Pubspec

The `pubspec.yaml` has a curious file extension—YAML stands for **Yet Another Markup Language**.

```
name: 'TextEditor'
version: 0.0.1
description: A web based text editor.
environment:
  sdk: '>=1.0.0 <2.0.0'
dependencies:
  browser: any
```

From this, it is clear to see pubspec.yaml is Dart's equivalent of a project or solution file. This holds the typical dependency details and meta-information. The Pubspec file is fully detailed at https://www.dartlang.org/tools/pub/pubspec.html. Although it is an extensive and powerful file, the main interaction you will probably have with it is adding new dependencies.

# Putting Dart into the web page

The file `index.html` is a regular web page. The rest of the file consists of a few simple `<div>` elements and, as you might expect, a `<textarea>` tag:

```
<!DOCTYPE html>
<html>
<head>
    <title>TextEditor</title>
    <link rel="stylesheet" href="styles/main.css">
</head>
```
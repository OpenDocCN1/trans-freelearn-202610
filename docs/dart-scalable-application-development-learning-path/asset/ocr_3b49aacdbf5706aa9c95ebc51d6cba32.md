WebStorm has a number of powerful features to help write our web applications:

+   - Auto-completion of code: properties and methods are listed as you type
+   - A static analyzer that scans as you type for possible problems
+   - Refactoring tools to help rename entities and extract methods
+   - Quick fixes to correct common errors easily
+   - Navigation via a code tree and finding usages of a function
+   - A code reformatter to keep the source code in a tidy format
+   - A powerful debugger

# Alternative development environments

If you wish to use another IDE, there are plugins for IntelliJ IDEA and Eclipse. If you prefer a simple text editor, there are plugins for Sublime, Emacs, and Vim. These are all listed on the DartLang download page.

If you are an avid Visual Studio user, there is a community project underway to bring Dart to that environment. DartVS can be downloaded from the Visual Studio Gallery. It directly uses the DartAnalyzer from the SDK to report errors, warnings, and hints inside the IDE. Support for intellisense and other features is underway, too.

The Atom Editor (https://atom.io/) is an alternative for those who prefer a lightweight editor to a fully fledged development environment. The plugin, named dartlang (https://atom.io/packages/dartlang) provides code-completion, syntax highlighting, and access to some pub commands.

# Help starting a project

To help create your new project, Dart has a tool called Stagehand that creates the scaffolding from a high quality project template of your choice. It takes care of common tasks such as folder structure, common libraries, and style sheets. It even has its own website (http://stagehand.pub/).

In WebStorm, creating a new project presents you with a range of powerful options, allowing you to set the locations of the Dart SDK and choose a project type for sample content. The project type gives a range of skeleton applications and is powered by the Stagehand application that is part of the SDK:
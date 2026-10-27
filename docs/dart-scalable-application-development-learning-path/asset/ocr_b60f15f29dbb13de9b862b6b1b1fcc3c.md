Then, you will want to enhance the structure of the project as follows:

| Folder | Project type | Files |
|---|---|---|
| at top-level | All | README.md, LICENSE, CHANGELOG, and AUTHORS files |
| doc | All | getting_started.md, todo.txt |
| example | All | Example scripts showing how to use the app |
| lib | Web | Folders for src, view, and model |
| src | Server | Folders for src and model |
| test | All | Unit test scripts |

# How it works...

The templates from Dart Editor provide you with a basic structure, but usually you'll want to add some folders as specified previously to provide a recognizable and professional structure where you can easily find what you want to look at, for example, which tests are included with the project. As we will see in the next recipe, in order to use application libraries, they have to be placed in the lib folder.

# There's more...

The README file (readme.md, which is in the markdown syntax; refer to http://en.wikipedia.org/wiki/Markdown) and the CHANGELOG file are shown in the pub repository on the page of your package, so their content is important.

Optional folders are as follows:

+   - mock: This contains classes to simulate certain behaviors of your app in testing environments, for example, a text file instead of a real database
+   - tool: This contains tooling scripts needed in the project such as a build script, test runners, and so on
+   - benchmark: When the performance is critical, this folder can contain examples to test it
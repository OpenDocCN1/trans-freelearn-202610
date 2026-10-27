In the next chapter, we will build on this foundation for some flashier features and graphical displays.

> **Downloading the example code**
> 
> You can download the example code files for all Packt books you have purchased from your account at `http://www.packtpub.com`. If you purchased this book elsewhere, you can visit `http://www.packtpub.com/support` and register to have the files e-mailed directly to you.

# ## Exploring the Web project structure

Go to the WebStorm welcome page and click on **Open**. Select the folder where you extracted the sample code and choose the `Chapter One` folder, then the sub-folder named `Start`.

A good first step when opening any Dart project is to ensure all dependencies are present and up to date. To achieve this, select `pubspec.yaml` and right-click to show the context menu. Click on the `Pub: Get Dependencies` item. The dependencies will then be resolved and fetched, if required.

| File / Folder | Description |
|---|---|
| `packages` | A link to the packages (or libraries) that are being used by this project. In this case, you will see the 'browser' package. |
| `pubspec.lock` | Controls the specific version of the packages being used. |
| `pubspec.yaml` | Meta information on our project (name, description, and so on) and a list of packages being used. |
| `web/styles/main.css` | The standard cascading style sheet file. |
| `web/index.html` | The entry point for our web application. |
| `main.dart` | Last but not least, a Dart file! |

# ## Unwrapping packages

From the preceding list, there is clearly a lot of structure and meta files for packages in even a simple Dart project. The philosophy of the language is that libraries are small, focused, and likely to be from different sources. They are also numerous and update on different schedules, so version controls are important—dependencies are hungry and must be fed.


### 工作原理...

在步骤 2 中，我们看到当按钮被点击时，ID 为 `show` 的段落会显示出来，这是因为事件处理器中的 `$('show').style.display = 'inline';` 这一行。同样，通过 `$('findme')` 可以获取到 ID 为 `findme` 的 `<div>` 元素的引用。该代码使用了自动节点查找（Polymer 的一个特性）来获取每个 HTML 元素的引用。自定义元素（即 shadow DOM 内部！）中带有 `id` 属性的每个节点，都可以使用语法 `$('ID')` 通过其 ID 引用。

> 此示例要求 Polymer dart 0.8.0 或更高版本。

## 实现 Polymer 应用的国际化

如果你希望 Web 应用能够根据 Web 客户端用户的语言，以多种语言显示信息，该怎么办？本方法将向你展示如何实现这一点。你可以在项目 `pol_intl` 中找到相关代码。

#### 操作步骤...

1. 通过 `pubspec.yaml` 将 pub 包中的 `intl` 包添加到你的应用中。

2. 脚本以 `web/pol_intl.html` 开头，其中通过以下代码行导入名为 `localized_text` 的组件：

   ```html
   <link rel="import" href="localized_text.html">
   ```

   由此可知，该组件定义在 `localized_text.html` 中，其背后的代码位于名为 `localized_text.dart` 的文件中。关于其他标签的讨论，请参阅第一个方法。

3. 组件结构在 `localized_text.html` 中概述如下：

   ```html
   <link rel="import" href="packages/polymer/polymer.html">
   <polymer-element name="localized-text">
   <template>
   <p>{{startMsg}}</p>

   <select value="{{selectedLocale}}">
   <option value="en_US">English</option>
   <option value="fr">French</option>
   <option value="nl_NL">Dutch</option>
   <option value="de">German</option>
   <option value="it">Italian</option>
   <option value="jp">Japanese</option>
   </select>

   </template>
   <script type="application/dart"
   src="localized_text.dart"></script>
   </polymer-element>
   ```

4. `localized-text` 的代码定义在 `localized_text.dart` 中：

   ```dart
   import 'package:polymer/polymer.dart';
   import 'package:intl/intl.dart';
   import 'messages_all.dart';

   @CustomTag('localized-text')
   class LocalizedText extends PolymerElement {
     @observable String selectedLocale;
     @observable String startMsg;

     LocalizedText.created() : super.created() {
       updateLocale(Intl.defaultLocale);
     }

     void selectedLocaleChanged() {
       initializeMessages(selectedLocale).then(
         (succeeded) => updateLocale(selectedLocale));
     }

     void updateLocale(localeName) {
       Intl.defaultLocale = selectedLocale;
       startMsg = start();
     }

     start() => Intl.message("Please choose your language and then start the tour.",
       name: 'startMsg',
       desc: "Starting the tour",
       args: [], // needed if arguments.
       examples: {"" : 0});
   }
   ```

运行应用时，你将看到以下截图：

![](img/52c05b53a56abd53584e4fed738ed509_845_0.png)

将 intl 与 Polymer 结合使用

#### 工作原理...

`intl` 包在步骤 1 中导入；它由 Dart 团队维护，提供了国际化和本地化功能，例如消息翻译、复数与性别、日期/数字格式化和解析，以及双向文本。在步骤 3 中，提供了一个语言下拉列表供你选择语言。所选选项的值会绑定到变量 `selectedLocale`。其余代码的目的是将 `startMsg` 的内容翻译成该语言。

在步骤 4 中，当选择一种语言时，会触发 `selectedLocaleChanged()` 方法。该方法以 `selectedLocale` 为参数调用 `updateLocale` 方法，后者将该值设置为 `defaultLocale`，并将 `startMsg` 设为 `start()` 函数的结果。这会为我们要翻译的消息执行 `Intl.message`，并以所选语言返回该消息。所有需要本地化的消息都被编写为函数，返回 `Intl.message` 调用的结果。

在内部，`intl` 的工作方式如下：

- 需要翻译的消息存储在名为 `intl_messages.json` 的文件中，包含它们的名称和默认语言的消息。这些消息是从程序源码中提取的，可以手动提取，也可以通过运行 `dart extract_json.dart localized_text.dart` 来生成 JSON 文件。
- 根据该文件，生成不同的 `translation_locale.json` 文件；每种要使用的语言对应一个文件。这些文件包含一个映射，其中包含语言环境、每条消息的名称和翻译。例如，以下是意大利语版本 `translation_it.json`：

  ```json
  {"_locale" : "it",
      "startMsg" : "Scegliere la lingua e poi iniziare il tour."}
  ```

然后，运行 `dart generate_from_json.dart localized_text.dart translation_fr.json translation_nl.json`。这将生成 `messages_all.dart`、`messages_fr.dart`、`messages_nl.dart` 等文件。

现在在主 Dart 脚本中导入 `messages_all.dart`，机制就准备好了。如果消息数量较少，你可以直接使用 `messages_` 文件本身，而不必从 JSON 文件生成它们。

#### 更多内容...

关于 `intl` 的更详细信息，请参阅其文档：http://www.dartdocs.org/documentation/intl/0.11.3/index.html#intl。

如果你不想让用户选择语言，而是想从浏览器提供的信息中获取语言，则可以使用以下代码：

```dart
import "package:intl/intl_browser.dart";
...
findSystemLocale().then(runTheRestOfTheProgram);
```

## 11 使用 Angular Dart

本章包含以下方法：

- 设置 Angular 应用
- 使用控制器
- 使用组件
- 使用格式化器作为过滤器
- 创建视图
- 使用服务
- 部署你的应用

### 简介

在本章中，我们将使用 Angular Dart 来构建客户端 Web 应用。Angular Dart (https://github.com/angular/angular.dart) 是 AngularJS 到 Dart 的移植版本。AngularJS，或简称为 Angular（参阅 www.angularjs.org），是一个由 Google 维护的流行开源 JavaScript 框架，用于开发单页动态 Web 应用。它的目标是创建具备**模型-视图-控制器（MVC）**或**模型-视图-视图模型（MVVM）**能力的基于 Web 的应用，从而使开发和测试都更加容易。

它通过声明式编程来构建 UI 并连接软件组件，从而使你可以专注于应用逻辑，而不是 DOM 操作。它使用一个带有许多所谓指令（以 ng- 开头）的模板系统，来指定可定制且可复用的 HTML 标签和表达式，以控制特定元素的行为：实际上，你可以用自定义元素和属性来扩展 HTML。Google 使用 Angular Dart 构建内部应用。本章探讨的每个方法都会展示 Angular 的一个主要组件。最新文档可以在 https://docs.angulardart.org/ 找到。

### 设置 Angular 应用

本方法是本章中所有其他方法所必需的预备步骤。它将展示如何让 Angular Dart 功能在你的应用中可用。

#### 操作步骤...

1. Dart Editor 中还没有 `angular` 模板，因此请从 Web 应用（移动友好）启动你的应用，并将其命名为，例如，`angular_setup`。清除 `html` 和 `dart` 文件中的示例代码。

2. 在 `pubspec.yaml` 的依赖中添加 `angular`。保存后将启动 `pub get` 过程。

3. 同时将 `js` 和 `shadow_dom` 添加到 `pubspec.yaml`。

4. 将 shadow DOM 脚本添加到 `html` 文件中：

   ```html
   <script
     src="packages/shadow_dom/shadow_dom.min.js"></script>
   ```

5. 同时，包含 Angular 转换器：

   ```yaml
   transformers:
     - angular
   ```

6. 在 `<html>` 元素中提供 `ng-app` 属性。

7. 在主 Dart 脚本的顶部添加以下语句：

   ```dart
   import 'package:angular/angular.dart';
   import 'package:angular/application_factory.dart';
   ```

8. 在 `main()` 中插入代码 `applicationFactory().run();`。

9. 在 `<body>` 元素中提供 `ng-cloak` 属性。

10. 将以下部分添加到你的 CSS 文件中：

    ```css
    [ng-cloak], .ng-cloak {
      display: none !important;
    }
    ```


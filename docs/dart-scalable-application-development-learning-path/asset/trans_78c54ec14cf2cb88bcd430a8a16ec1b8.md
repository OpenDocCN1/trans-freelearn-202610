

首先，我们在 `pubspec.yaml` 文件的依赖部分添加 `intl` 包。然后，我们决定尝试命名约定和国际化方法的组合，以提供在设置显示名称时最大的灵活性，因为这里有意义的显示名称非常重要。要创建组件的显示名称，我们将执行以下操作：

- 使标题标签的所有层级都以 Head 后缀结尾
- 所有标签都以 Lbl 后缀结尾
- 以 Btn 后缀结尾需要提交的输入元素

每个引用元素必须使用被引用元素的标识符名称结合前面提到的后缀。因此，现在我们准备国际化注册表单。

表单中包含字符串消息的所有元素必须具有唯一的标识符。我们将使用以下代码从注册表单中删除所有文本消息：

```html
<h1 id="formHead"></h1>
<form>
  <table>
    <tr>
      <td><label for="firstName" id="firstNameLbl"></label></td>
      <td><input type="text" id="firstName" name="firstName"></td>
    </tr>
    <tr>
      <td><label for="lastName" id="lastNameLbl"></label></td>
      <td><input type="text" id="lastName" name="lastName"></td>
    </tr>
    <tr>
      <td><label id="genderLbl"></label></td>
      <td>
        <input type="radio" name="sex" value="male">
        <span id="maleLbl"></span>
        <input type="radio" name="sex" value="female">
        <span id="femaleLbl"></span>
      </td>
    </tr>
    <tr>
      <td colspan="2"><input type="submit" id="registerBtn"></td>
    </tr>
    <tr>
      <td colspan="2"><span id="copyrightLbl"></span></td>
    </tr>
  </table>
</form>
```

我们需要在 Dart 代码中为每个被翻译的消息创建一个函数。在这种情况下，你必须为以下代码选择命名约定：

```dart
formHead() => "Registration Form";
firstNameLbl() => "First name:";
lastNameLbl() => "Last name:";
genderLbl() => "Gender:";
maleLbl() => "Male";
femaleLbl() => "Female";
registerBtn() => "Register";
copyrightLbl() => "Copyright 2014 דויש דיגיטל";
```

我们将使用 querySelector 函数找到通过唯一标识符翻译的所有元素，并将翻译后的消息分配给它们适当的属性，如以下代码所示：

```dart
querySelector("#formHead").text = formHead();
querySelector("#firstNameLbl").text = firstNameLbl();
querySelector("#lastNameLbl").text = lastNameLbl();
querySelector("#genderLbl").text = genderLbl();
querySelector("#maleLbl").text = maleLbl();
querySelector("#femaleLbl").text = femaleLbl();
(querySelector("#registerBtn") as InputElement)
  .value = registerBtn();
BidiFormatter bidiFormatter = new BidiFormatter.UNKNOWN();
querySelector("#copyrightLbl").text =
  bidiFormatter.wrapWithUnicode(copyrightLbl());
```

现在，对创建的每个函数应用 Intl.message 方法，如以下代码所示：

```dart
formHead() => Intl.message("Registration Form",
    name:"formHead",
    desc: "Registration Form title");
firstNameLbl() => Intl.message("First name:",
    name: "firstNameLbl",
    desc: "First Name label");
lastNameLbl() => Intl.message("Last name:",
    name: "lastNameLbl",
    desc: "Last Name label");
genderLbl() => Intl.message("Gender:",
    name: "genderLbl",
    desc: "Gender label");
maleLbl() => Intl.message("Male",
    name:"maleLbl",
    desc: "Male label");
femaleLbl() => Intl.message("Female",
    name: "femaleLbl",
    desc: "Female label");
registerBtn() => Intl.message("Register",
    name: "registerBtn",
    desc: "Registration Button name");
copyrightLbl() => Intl.message("Copyright 2014 דויש דיגיטל",
    name: "copyrightLbl",
    desc: "Copyright label");
```

如果你在网页浏览器中打开页面，它现在看起来就像原来的页面一样。现在你知道了如何使用 `Intl` 类翻译消息，是时候讨论本地化了。

## 提取消息

现在，我们已将所有消息与 UI 分离，并且准备进行翻译。我们需要使用 intl 包中的一个特殊程序 `extract_to_arb.dart` 从源代码中将其提取到外部文件，如以下代码所示：

```
pub run intl:extract_to_arb --output-dir=web web/registration_form.dart
```

前面的程序会在指定的 web 目录内生成 `intl_messages.arb` 文件。该文件包含 ARB 格式中的所有消息。ARB 是基于 JSON 的本地化资源格式。此格式提供以下好处：

- **简洁性**：此格式基于 JSON，简单且人类可读
- **可扩展性**：在此格式中，可以在不影响现有工具和使用的情况下添加词汇
- **直接可用性**：应用程序可以直接从此格式访问资源，而无需将其转换为其他形式

你可以在 `https://code.google.com/p/arb/wiki/ApplicationResourceBundleSpecification` 提供的规范中了解更多关于 ARB 格式的信息。

之后，你可以将 ARB 格式的文件发送给人工翻译。由于我们的表单相当简单，我们可以使用 Google Translator Toolkit 自己将文件翻译成其他语言。

## 使用 Google Translator Toolkit

以下步骤将向你展示如何使用 Google Translator Toolkit：

1. 打开你的网页浏览器并导航到 `https://translate.google.com/toolkit`。点击 **UPLOAD** 按钮上传文件，如以下屏幕截图所示：

   ![](img/52c05b53a56abd53584e4fed738ed509_430_0.png)

2. 你将被带到另一个网页，在那里你可以添加要翻译的内容。你可以选择要翻译的 `intl_messages.arb` 文件：

   ![](img/52c05b53a56abd53584e4fed738ed509_430_1.png)

3. 选择你希望消息被翻译成的适当翻译语言，然后点击 **Next** 按钮：

   ![](img/52c05b53a56abd53584e4fed738ed509_430_2.png)

4. 在下一页，你可以选择其中一个准备翻译的供应商，或点击 **No, thanks** 按钮自行翻译消息。 resulting 新文件将出现在准备翻译的文件列表中，如以下屏幕截图所示：

   ![](img/52c05b53a56abd53584e4fed738ed509_431_0.png)

5. 现在点击文件列表中的文件名，Google Translator Toolkit 将在编辑器中打开它：

   ![](img/52c05b53a56abd53584e4fed738ed509_431_1.png)

此方法应在先使用指定的 `localeName` 方法之前调用。让我们更改代码以使德语区域设置默认可用。我们需要做的就是将 `messages_all.dart` 文件导入到我们的项目中，并在 `main` 方法中添加 `initializeMessages`，如以下代码所示：

```dart
import 'messages_all.dart';
...
void main() {
  initializeMessages('de').then((_) {
    Intl.defaultLocale = 'de';
    querySelector("#formHead").text = formHead();
    querySelector("#firstNameLbl").text = firstNameLbl();
    querySelector("#lastNameLbl").text = lastNameLbl();
    querySelector("#genderLbl").text = genderLbl();
    querySelector("#maleLbl").text = maleLbl();
    querySelector("#femaleLbl").text = femaleLbl();
    (querySelector("#registerBtn") as InputElement)
      .value = registerBtn();
    BidiFormatter bidiFormatter = new BidiFormatter.UNKNOWN();
    querySelector("#copyrightLbl").text =
      bidiFormatter.wrapWithUnicode(copyrightLbl());
  });
}
```

在前面的代码中，我们在初始化消息时指定了德语区域设置作为默认值。现在，打开浏览器中的 `index.html` 文件以查看表单在德语区域设置中的正确翻译：

![](img/52c05b53a56abd53584e4fed738ed509_433_0.png)

## 总结

在本章中，我们讨论了基于 Dart 语言的项目国际化和本地化的一些重要方面。全球化软件的开发包括国际化，涵盖设计和开发 Web 应用程序，以及本地化，包括为特定区域设置翻译和定制 Web 应用程序。

pub 管理器中的 `intl` 包帮助你以相当直接的方式为服务器端和客户端设计和开发应用程序。所有可执行代码必须与实现 UI 的编程代码分离。你必须避免直接将数字和日期转换为字符串，因为不同文化对数字和日期的呈现方式不同。

`NumberFormat` 类包含特殊构造函数，可快速创建常用的模式，这对于测量单位和货币非常有用。`BidiFormatter` 类帮助管理包含双向文本的消息，具有 BiDi 包装、自动方向性估计和字符转义。`DateFormat` 类有一套巨大的命名模式，有助于避免错误并以正确格式显示数据。消息的国际化基于通过命名本地化版本的消息进行查找，并返回翻译后的消息，可能与指定的参数列表插值。

`Intl` 类的 `plural` 方法可以帮助使用括号将单数和复数形式组合成一个字符串。`Intl` 类的 `gender` 方法为基于性别的选择提供开箱即用的支持。`Intl` 类的 `select` 方法帮助根据可用选择以不同方式格式化消息。`Intl` 类的 `message` 方法可以将消息表示为简单的表达式，并具有使用属性插值的可能性。

在下一章中，我们将向你展示如何正确组织客户端到服务器的通信。你将找到关于使用客户端到服务器通信选择 Dart 类的可能重要问题的答案。


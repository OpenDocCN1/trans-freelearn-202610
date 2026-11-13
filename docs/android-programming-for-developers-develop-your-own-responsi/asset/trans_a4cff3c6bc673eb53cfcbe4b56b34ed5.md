

我们可以使用 switch 块来处理 RadioButton 点击事件，为每个可能被按下的 RadioButton 提供 case，并使用 rb.getId() 方法作为 switch 块的表达式。下面的代码展示了我们讨论过的 onCheckedChanged 方法的全部内容：

```
// Get a reference to the RadioButton that is currently checked
RadioButton rb = (RadioButton) group.findViewById(checkedId);

// Switch based on the 'friendly' id
switch (rb.getId()) {
    case R.id.radioButton1:
        // Do something here
        break;

    case R.id.radioButton2:
        // Do something here
        break;

    case R.id.radioButton3:
        // Do something here
        break;
}
// End switch block
```

在下一个可运行的微型应用中亲眼看到该代码块的实际运行效果，并真正按下按钮，这会让事情变得更加清晰。

## A switch widget

Switch 小部件（不要与 Java 关键字小写的 switch 混淆）与 Button 小部件类似，只不过它具有两种可读取和响应的状态。switch 小部件的一个明显用途是显示和隐藏内容。还记得在我们第 10 章“一切都是类”的 Java Meet UI 应用中，我们使用按钮来显示和隐藏 TextView。每次显示/隐藏 TextView 时，我们都会更改按钮上的 Text 属性，以便清楚如果再次点击会发生什么。对用户来说可能更合理，对我们程序员来说也更直接的做法，是使用 Switch，如下图所示：

![](img/35d01831c19688a44e50bce7c489007a_372_0.png)

下面的代码假设我们已经有一个名为 mySwitch 的对象，它引用了布局中的 Switch 对象。我们可以像在第 10 章“一切都是类”的 Java Meet UI 应用中所做的那样显示和隐藏 TextView。为了监听并响应点击，我们再次使用匿名类。不过这次，我们使用的是 CompoundButton 版本的 OnCheckedChangeListener，而不是 RadioGroup。

我们需要重写 onCheckedChanged 方法，该方法带有一个名为 isChecked 的布尔参数。isChecked 变量简单来说，false 表示关闭，true 表示打开。

下面是一种更直观的替换文本显示/隐藏代码的方法：

```
mySwitch.setOnCheckedChangeListener(
 new CompoundButton.OnCheckedChangeListener() {
 public void onCheckedChanged(
 CompoundButton buttonView, boolean isChecked) {
 if(isChecked){
 // Currently visible so hide it
 txtValue.setVisibility(View.INVISIBLE);
 }else{
 // Currently hidden so show it
 txtValue.setVisibility(View.VISIBLE);
 }
 }
 }
);
```

如果匿名类代码看起来仍有点奇怪，请不用担心，因为我们使用得越多，就会越来越熟悉。现在当我们查看 CheckBox 时，我们还会再次这样做。

## CheckBox

我们在第 5 章“实际布局”的表单布局中使用了 CheckBox。然而，与 EditText 类似，我们并没有探讨如何实际从中获取数据。对于 CheckBox，我们只需在某个时刻（例如，当表单的提交按钮被点击时）检测其状态（选中或未选中）。下面的代码让我们一窥这种情况是如何发生的，同样，使用了一个充当监听器的内部类：

```
myCheckBox.setOnCheckedChangeListener(
    new CompoundButton.OnCheckedChangeListener() {

    public void onCheckedChanged(
    CompoundButton buttonView, boolean isChecked) {

    if (myCheckBox.isChecked()) {
    // It's checked so do something
    } else {
    // It's not checked do something else
    }

    }
);
```

在前面的代码中，我们假设 myCheckBox 已经声明并初始化，然后我们使用与 Switch 完全相同类型的匿名类来检测和响应点击。

## WebView

该小部件可以作为整个应用的容器。我们可以将完整网页的代码放在 res 文件夹中，然后从应用内部使用 JavaScript 语言来操作这些网页。将整个应用的功能包含在网页中，仅由 Android 应用进行展示，并可能通过 webView 添加一些额外功能，这是很常见的。

然而，采用这种方式的应用很少能达到真正原生 Android 应用那样专业的外观和手感，因为本书中您学习的 UI 功能远比常规 Web 编程所能实现的更强大、更灵活。

我们还可以在应用中使用 webView 向用户展示网页，但方式是让他们不必离开我们的应用来打开网页浏览器。这段代码在 WebView 中加载 Google：

```
WebView webView = (WebView) findViewById(R.id.webView);
webView.loadUrl("http://google.com");
```

和往常一样，前面的 Java 代码假设我们在布局 XML 中有一个 id 属性设置为 webView 的 WebView 小部件。

使用此小部件时，我们需要让用户知道应用将访问 Internet，并在用户安装应用时征得他们的许可。我们通过在 AndroidManifest.xml 文件中添加以下一行来实现：

```
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
```

我们将在下一个微型应用中实际这样做。

## Date & Time

Android 设备通常带有时钟和日历。不过，有时我们可能希望在应用中显示时钟或让用户选择日期。从调色板中，我们可以选择一系列有用的日期和时间小部件。带有删除线的小部件已弃用，通常不再使用：

| 日期和时间 |
|---|
| TextClock | AnalogClock | DigitalClock | Chronometer | DatePicker |
| TimePicker | CalendarView |

在下一个应用中，我们将使用 TextClock 小部件向您展示其一些功能。

有了所有这些新信息，让我们开发一个应用，以更深入地使用 Android 小部件，超出我们目前所做的范围。不过，我们将先快速跑个偏，讨论 Android 权限，尤其是关于（在编写本书时）最新版本的 Marshmallow。

## Android permissions and Marshmallows

当我们的应用想要做某事，这件事可能会对用户的设备或用户隐私造成潜在危害时，Android 要求我们以特定方式处理，通常是在执行某些操作之前让应用向用户请求权限。

2015 年底 Android Marshmallow 推出后，我们处理事物的方式发生了改变——但仅适用于新的 Marshmallow 设备。

因此，当前的情况是我们需要处理两种可能的情形：Marshmallow 和 Marshmallow 之前。

### Pre-Marshmallow permissions

在 Marshmallow 之前，如果我们希望应用做某件事，这件事可能会造成潜在危害，例如连接 Internet（我们的下一个应用将这样做）、获取 GPS 位置、拍照、访问通讯录等等，我们需要在 AndroidManifest.xml 文件中添加一行代码。

然后，当用户从 Google Play 安装我们的应用时，会弹出一个小警告窗口，告诉他们我们的应用想要执行哪些潜在有害的操作。用户必须同意他们信任我们的应用，否则安装将被中止。

其工作原理如下步骤所示：

- 1. 我们想要添加潜在有害的功能 x。
- 2. 我们在 AndroidManifest.xml 中为功能 x 添加权限。
- 3. 用户从 Google Play 下载应用，并被警告我们的应用使用了功能 x。
- 4. 用户决定是否允许我们的应用使用功能 x。如果用户允许，应用将被安装；如果用户不允许，则不会安装。

### 注意

作为开发者或用户，都无需再考虑该权限

但随后 Marshmallow 出现并改变了一切。

### Marshmallow permissions

Marshmallow 在应用安装时不会请求权限。它会在即将使用需要权限的特定功能时请求权限。这对用户来说可能是件好事，也可能是件坏事，取决于你的看法。

一个潜在的好处是，用户可以安装带有（例如）两个权限的应用：访问其通讯录的权限和拨打电话的权限。这个假设的例子可能是一个通讯录应用，它允许用户添加联系人并点击联系人来拨打电话。

在 Marshmallow 下，用户将能够向其中一个功能授予权限，例如，访问通讯录的权限以允许他们管理联系人。他们可以拒绝允许应用拨打电话的权限，并确信该应用永远不会对巨额电话账单负责。而使用 Marshmallow 之前的权限，则是全有或全无。

一个缺点是，用户会更容易安装包含他们可能不赞同的功能的应用，并且在浪费一些时间，可能错过 Google Play 提供的 15 分钟退款窗口后才意识到这一点。然而，我相信 Google 在设计这个系统时只考虑了用户安全。

这对我们作为开发者意味着，我们不仅需要为 Marshmallow 之前的设备向 `AndroidManifest.xml` 文件添加权限，还需要在所有正确的位置添加代码，以便在运行时处理 Marshmallow 权限。

情况因 Marshmallow 将某些权限视为 *dangerous*，而将其他权限视为 *normal* 而变得更加复杂。这将为我们的代码增加显著的复杂性。


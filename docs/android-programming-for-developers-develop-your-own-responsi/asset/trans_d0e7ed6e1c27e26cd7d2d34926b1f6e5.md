
通过在 PC 上按下 Ctrl + F11 键盘组合或在 Mac 上按下 Ctrl + fn + F11，可将其旋转为横向模式。

## 提示

撰写本文时，使用 Ctrl + F11 更改模拟器方向时，模拟器似乎会崩溃。因此，如果在让模拟器以横向模式显示时遇到任何问题，只需将其重新配置为以横向模式启动即可。你可以通过点击工具栏中的 AVD 图标，然后点击 Nexus 5 AVD 旁边的编辑（铅笔图标），并选择 Landscape 来完成此操作。点击 Finish 后，即可准备运行应用。

尝试勾选单选按钮，查看时钟上的时区变化。向 EditText 中输入不同的值，然后单击按钮，即可查看它如何获取文本并在自身上显示。

![](img/35d01831c19688a44e50bce7c489007a_394_0.png)

通过不同组合的选中和未选中复选框来更改图像的外观，并使用开关小部件隐藏和显示网页。

让我们继续了解另一种可以显著增强我们 UI 的方式。

## 摘要

在本章中，我们探索了大量小部件，了解了如何在不使用任何 XML 布局的情况下在 Java 代码中实现小部件，使用我们的第一个匿名类来处理对小部件的点击，并将我们所有新掌握的小部件技能应用到一个小型应用中。

在下一章中，我们将看到一个全新的 UI 元素，它不能直接从调色板拖放，但我们仍将获得 Android API 的大量帮助。接下来是对话框。我们还将着手开发目前为止最重要的应用——Note To Self 备忘录、待办事项和个人备忘应用。

## 第 12 章。与用户对话

在本章中，我们将了解如何向用户展示弹出式对话框窗口。然后，我们可以将所学知识应用到我们的第一个应用 Note To Self 的第一阶段中。在本章及接下来的五个章节中，你将学习新的 Android 和 Java 功能，我们将使用你新获得的知识每次都增强 Note To Self 应用。

在每个章节中，我们还将构建一系列与主应用分离的小型应用。因此，本章为你准备了以下内容：

- 如何实现带有弹出式对话框的简单应用
- 如何添加 DialogFragments 以启动 Note To Self 应用
- 如何将 Strings 和图形等资源添加到项目中
- 如何首次使用 Android 命名约定使我们的代码更易读
- 如何实现更复杂的对话框以捕获用户输入

让我们开始吧。

### 对话框窗口

通常，我们希望在弹出式窗口中向用户显示一些信息，或者询问用户是否确认某项操作。这被称为 **对话框** 窗口。如果你快速浏览调色板，可能会惊讶地发现完全没有提到对话框。

Android 中的对话框比简单的小部件甚至布局更高级。它们是可以包含自身布局和小部件的类。

在 Android 中创建对话框窗口的最佳方法是使用 `Fragment` 类。Fragment 是 Android 中广泛且重要的主题，我们将花费本书下半部分的大部分时间来探索和使用它们。

不过，为我们的用户创建一个整洁的弹出式对话框进行交互，是了解 Fragment 的绝佳入门，并且一点也不复杂。

### 对话框演示小型项目 – 链式调用简介

我们之前提到，在 Android 中创建对话框的最佳方法是使用 Fragment 类。在 Android 中，还有一种创建对话框的方法，可以说简单一些。但这个稍微简单一点的 Dialog 类的问题是，它在 Activity 生命周期中受到的支持不够好。甚至有可能 Dialog 意外导致应用崩溃。

如果你正在编写一个具有一个固定方向布局且需要一个简单弹出式对话框的应用，有人可能会认为应该使用更简单的 Dialog 类。但是，由于我们致力于构建具有高级功能的现代、专业应用，因此忽略该类对我们有益。更具体地说，我们将使用 DialogFragment 类。

要启动小型项目，请在 Android Studio 中创建一个新的空白项目，并将其命名为 Dialog Demo。和往常一样，该项目的完整代码位于下载包的 Chapter 12/Dialog Demo 文件夹中。

通过右键单击包含 MainActivity.java 文件的包名称文件夹，创建一个新类。在“新建”下选择 Java 类，并将其命名为 MyDialog。单击 OK 以创建该类。

首先要做的是更改类声明以扩展 DialogFragment。完成后，你的新类将如下所示：

```java
public class MyDialog extends DialogFragment {
...
}
```

现在，让我们逐步向此类添加代码，并查看每个步骤中发生了什么。

首先，我们重写 onCreateDialog 方法，该方法将在我们稍后通过 MainActivity 中的代码显示对话框窗口时被调用。

然后，在 onCreateDialog 方法中，我们接触到一个新类。我们声明并初始化一个 AlertDialog.Builder 类型的对象，该对象需要一个对 MainActivity 的引用，该引用通过其构造函数传递。这就是为什么我们将 getActivity() 用作参数的原因。getActivity 方法是 Fragment 类的一部分（因此 DialogFragment 也是），它返回创建 DialogFragment 的 Activity 的引用。在此例中，它是我们的 MainActivity 类。添加此代码以准备好使用 builder 引用变量，然后我们将看看可以用它做什么：

```java
@Override
public Dialog onCreateDialog(Bundle savedInstanceState) {
    // Use the Builder class because this dialog has a simple UI
    AlertDialog.Builder builder =
    new AlertDialog.Builder(getActivity());
}
```

现在，我们可以使用 builder 对象来完成其余工作。接下来的三段代码中有一点点奇怪。如果你快速浏览接下来的三段代码，会注意到明显缺少分号；。这表明，对于编译器来说，这三段代码实际上只是一行。

这里发生的事情，我们在不太明显的情况下实际上之前见过。当我们创建 Toast 消息并在末尾添加 .show() 方法时，我们正在进行 **链式调用**，也就是说，我们正在按顺序对同一个对象调用多个方法。这等同于编写多行代码；只是这样更清晰、更简洁。

在使用链式调用的三个代码块的第一个中，我们调用 builder.setMessage，它设置用户在对话框中将看到的主消息。还要注意的是，在链式方法调用的各个部分之间插入注释是可以的，因为编译器会完全忽略它们。

然后，我们使用 .setPositiveButton 方法向对话框添加一个按钮，第一个参数将按钮上的文本设置为 OK。第二个参数是一个匿名 DialogInterface.OnClickListener 类，用于处理对按钮的点击。请注意，我们不会向 onClick 方法添加任何代码。

添加此代码，当然，没有任何分号：

```java
// Dialog will have "Make a selection" as the title
builder.setMessage("Make a selection")
```

接下来，我们在同一个 builder 对象上调用另一个方法。这次是 setNegativeButton 方法。同样，两个参数将 Cancel 设置为按钮的文本，并指定一个匿名类来监听点击。为了此演示的目的，我们不会在重写的 onClick 方法中采取任何操作。

在调用 setNegativeButton 之后，我们终于看到了标记代码行结束的分号。然后我们编写 return builder.create()。这将把控制权返回给 MainActivity（最初调用 onCreateDialog 的地方），即我们新的、完全配置好的对话框窗口。我们很快就会看到这段调用代码。

在前面的代码块之后直接添加以下代码，并在 onCreateDialog 方法内：

```java
// A "Cancel" button that does nothing
.setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
    public void onClick(DialogInterface dialog, int id) {
        // Nothing happening here either
    }
});
// Create the object and return it
return builder.create();
};// End onCreateDialog
```

现在，我们有了扩展 DialogFragment 的 MyDialog 类；我们所要做的

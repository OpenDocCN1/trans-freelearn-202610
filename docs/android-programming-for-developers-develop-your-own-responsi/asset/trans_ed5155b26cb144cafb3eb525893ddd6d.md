
为此，需要声明一个 MyDialog 对象，将其实例化，并调用其 createDialog 方法。

让我们向布局中添加一个按钮。导航到 **layout_main.xml** 选项卡，然后切换到 **Design** 选项卡。将按钮拖放到布局上，并确保其 id 属性设置为 button。

现在，转到 **MainActivity** 选项卡，我们将使用匿名类处理对该按钮的点击事件。由于布局中只有一个按钮，这样做似乎很合理。

请注意，在下面的代码中，匿名类的类型与我们在第 10 章 Everything's a Class 中为其实现接口的那个类的类型完全相同。将此代码添加到 onCreate 方法中。

请注意，在这样做之前，还要注意代码中唯一发生的事情是：onClick 方法创建了一个 MyDialog 的新实例并调用其 show 方法，这将不出所料地显示我们在 MyDialog 类中配置的对话框窗口。show 方法需要一个对 FragmentManager 的引用（我们通过 getFragmentManager 获取它）；这是用于跟踪和控制 Activity 中所有 Fragment 实例的类。我们还传入一个 ID（我们传入 123）。有关 FragmentManager 的更多详细信息将在我们更深入地研究 Fragments 时揭晓，该内容在第 18 章 Design Patterns, Fragments, and the Real World 中讨论：

```
/*
Let's listen for clicks on our regular Button.
We can do this with an anonymous class.
*/

Button button = (Button) findViewById(R.id.button);

button.setOnClickListener(
    new View.OnClickListener() {
    @Override
    public void onClick(View v) {
    // We only handle one button
    // So no switching required
    MyDialog myDialog = new MyDialog();
    myDialog.show(getFragmentManager(), "123");
    // This calls onCreateDialog
// Don't worry about the strange looking 123
// We will find out about this in chapter 18
}
}
);
```

现在，我们可以运行应用程序，欣赏在主布局中点击按钮时出现的新对话框窗口。请注意，点击对话框窗口中的任何按钮都会关闭它。这是默认行为。下面的截图展示了我们的对话框窗口在运行中的样子：

![](img/35d01831c19688a44e50bce7c489007a_402_0.png)

接下来，在本章中，我们将创建两个实现对话框的类，作为 Note To Self 应用的第一阶段。我们将看到，对话框窗口可以拥有几乎我们选择的任何布局，我们不必依赖 Dialog.Builder 类在本小应用中提供的简单布局。

## 关于 Note To Self 应用

欢迎来到本书中我们将实现的三个主要应用中的第一个。在进行这些项目时，我们将比处理小应用更专业地对待它们。我们将使用 Android 命名规范、字符串资源和合适的封装。

有时，当你尝试学习新的 Android/Java 主题时，这些东西可能有些大材小用，但对于真实世界的项目来说，它们非常有用且重要。尽早开始使用它们是件好事。然后，它们会变得像本能一样，我们的应用质量也会从中受益。以下是关于字符串资源和 Android 命名规范的快速回顾。

## 使用命名规范和字符串资源

在第 3 章 Exploring Android Studio 中，我们讨论了在布局文件中使用字符串资源而非硬编码文本。这样做有几个好处，但也稍微有点啰嗦。

由于这是我们的第一个真实世界项目，现在是用正确的方法做事的好时机，这样我们可以获得这方面的经验。如果你想快速回顾字符串资源的好处，请参阅第 3 章 Exploring Android Studio。

命名规范是用于为代码中的变量、方法和类命名的约定或规则。在整本书中，我们比较宽松地应用了 Android 命名规范。由于这是我们的第一个真实世界应用，我们在应用这些命名规范时会稍微严格一些。最重要的是，当变量是类的成员时，我们会在其前面添加小写前缀 m。

### 提示

你可以在 https://source.android.com/source/code-style.html 上找到更多关于 Android 命名规范和代码样式的信息。

总的来说，在整本书中，在学习新事物或构建小应用时，我们对命名规范和字符串资源会比较宽松，但在构建主要应用时会比较严格。整本书中有三个这样的应用（如果包含第 18 章 Design Patterns, Fragments, and the Real World，则为四个），其中 Note To Self 是第一个。

## 如何获取 Note To Self 代码

完成的完整应用（包括所有代码和资源）可以在下载包中的 Chapter 17/Note to self 文件夹中找到。由于我们将在接下来的六个章节中实现此应用，在几乎每个章节结束时查看部分完成的可运行应用也可能很有用。部分完成的可运行应用及其所有相关代码和资源可以在各自的文件夹中找到：

- Chapter 12/Note to self
- Chapter 14/Note to self
- Chapter 15/Note to self
- Chapter 16/Note to self
- Chapter 17/Note to self

第 16 章 UI Animations 的可运行项目与第 17 章 Sound FX and Supporting Different Versions of Android 的新代码合并在一起。

请注意，每个项目都是一个独立的可运行项目，并且每个项目都包含在其自身唯一的 Java 包中。这样做是为了让你能轻松查看应用运行情况，就像完成某个特定章节后那样。在复制和粘贴代码时，要小心不要包含包名，因为它很可能与你的不同，代码将无法编译。

如果你跟随本书并打算从头到尾构建 Note To Self，我们只需创建一个名为 Note To Self 的项目。不过，如果你想在任何时候进行一些复制和粘贴，仍然可以查看每个章节的项目代码文件。只需不要从文件顶部复制包指令，并且请注意，在说明中的极少数地方，你会被要求删除/替换之前章节中的某一行代码。

因此，即使你复制粘贴的代码多于输入的代码，也请务必完整阅读说明，并查看书中的代码以了解可能有用的额外注释。

在每个章节中，代码将像你已经完全完成上一章一样呈现，并在必要时显示来自之前章节的代码作为我们新代码的上下文。

每个章节并不仅仅专注于 Note To Self 应用；你还将学习其他通常相关的事物，并构建几个小应用。因此，当我们进行 Note To Self 实现时，我们在技术上将为此做好准备。

## 我们将做什么以及何时做

值得提前了解我们将在何时实现应用的不同部分：

- 在本章中，我们将研究资源、对话框和 Note 类。我们将收集并添加到项目中整个项目期间所需的所有字符串和图像资源。图像资源非常重要，出于版权原因，它们不包含在下载包中。但是，它们可以免费从 Android 开发者网站下载。我们还将创建两个扩展 DialogFragment 的类，并创建自定义布局文件，使用户能够添加新笔记或查看现有笔记。我们还将看到对话框窗口如何与 MainActivity 类通信。此外，我们还将实现一个简单的类，用于表示笔记中包含的数据结构。

- 在第 13 章 Handling and Displaying Arrays of Data 中，我们将完成几个小应用；这将为我们在第 14 章 Handling and Displaying Notes in Note To Self 中为 Note To Self 添加新功能做好准备。

- 在第 14 章 Handling and Displaying Notes in Note To Self 中，我们将使用新掌握的数据处理技能以及 Android API 中的几个漂亮的新类，使应用中所有需要显示在滚动列表中的数据能够显示。

- 在第 15 章 Android Intent and Persistence 中，如果这些数据在用户退出应用或关闭手机时消失，那对用户来说毫无用处。你将学习并使用两种策略来永久持久化数据。一种策略用于应用设置屏幕中的简单数据，另一种用于实际应用数据：笔记。

- 在第 16 章 UI Animations 中，我们将添加漂亮的动画，例如当用户将笔记标记为重要时闪烁笔记。

- 在第 17 章 Sound FX and Supporting Different Versions of Android 中，我们将为应用添加一个简单的声音，以及从应用中删除笔记的功能。

那么，我们的新应用的用户究竟能做什么？

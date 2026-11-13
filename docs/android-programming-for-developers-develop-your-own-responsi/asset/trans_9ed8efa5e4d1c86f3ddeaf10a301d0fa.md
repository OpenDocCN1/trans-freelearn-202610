

最后，在以下代码块中，我们获取每个 UI 小部件的引用，并将 txtTitle 和 textDescription 的文本属性设置为 mNote 的相应成员变量（mNote 已在 sendNoteSelected 中初始化）。

将我们刚才讨论的代码添加到 onCreateDialog 方法中：

```
AlertDialog.Builder builder = new
AlertDialog.Builder(getActivity());

LayoutInflater inflater = getActivity().getLayoutInflater();
View dialogView = inflater.inflate(R.layout.dialog_show_note,
    null);

TextView txtTitle = (TextView)
dialogView.findViewById(R.id.txtTitle);

TextView txtDescription = (TextView) dialogView.findViewById
(R.id.txtDescription);

txtTitle.setText(mNote.getTitle());
txtDescription.setText(mNote.getDescription());

ImageView ivImportant = (ImageView) dialogView.findViewById
(R.id.imageViewImportant);

ImageView ivTodo = (ImageView) dialogView.findViewById
(R.id.imageViewTodo);
ImageView ivIdea = (ImageView) dialogView.findViewById
(R.id.imageViewIdea);
```

接下来的代码也在 onCreateDialog 方法中。它检查要显示的笔记是否重要，然后相应地显示或隐藏 ivImportant ImageView。我们对 ivTodo 和 ivIdea 做完全相同的操作。

在上一代码块之后添加此代码，该代码块仍在 onCreateDialog 方法中：

```
if (!mNote.isImportant()){
    ivImportant.setVisibility(View.GONE);
}

if (!mNote.isTodo()){
    ivTodo.setVisibility(View.GONE);
}

if (!mNote.isIdea()){
    ivIdea.setVisibility(View.GONE);
}
```

现在我们只需要在用户单击 **OK** 按钮时关闭对话框窗口。这是通过匿名类完成的，就像我们之前多次看到的那样。onClick 方法只需调用 dismiss 方法即可实现。

在上一代码块之后，在 onCreateDialog 方法中添加此代码：

```
Button btnOK = (Button) dialogView.findViewById(R.id.btnOK);

builder.setView(dialogView).setMessage("Your Note");

btnOK.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        dismiss();
    }
});

return builder.create();
```

我们已经准备好两个对话框窗口可以投入使用了。现在，我们只需要向 MainActivity 添加一些代码来完成任务。

## 显示我们的新对话框

在 MainActivity 声明后立即添加一个新的临时成员变量。这不会出现在最终应用中，因为它只是为了让我们可以测试我们的对话框：

```
// Temporary code
Note mTempNote = new Note();
```

现在，添加此方法，以便我们可以从 DialogNewNote 类接收新笔记：

```
public void createNewNote(Note n){
// Temporary code
mTempNote = n;
}
```

现在，要将笔记发送到 DialogShowNote 方法，我们需要在 layout_main.xml 布局文件中添加一个带有按钮 ID 的按钮。

为了明确此按钮的用途，请将其文本属性更改为“显示笔记”，将按钮拖动到 layout_main.xml 上，并按我们刚才描述的方式对其进行配置。

为了澄清，这是一个仅用于测试目的的临时按钮，不会出现在最终应用中。

现在，在 onCreate 方法中，我们将设置一个匿名类来处理对临时按钮的点击。onClick 中的代码执行以下操作：

- 1. 创建一个名为 dialog 的新 DialogShowNote 实例。
- 2. 在 dialog 上调用 sendNoteSelected 方法，将我们的 Note 对象 mTempNote 作为参数传递。
- 3. 最后，我们调用 show，使我们的新对话框活跃起来。

将我们刚才描述的以下代码添加到 onCreate 中：

```
// Temporary code
Button button = (Button) findViewById(R.id.button);
button.setOnClickListener(new View.OnClickListener() {
@Override
public void onClick(View v) {
    // Create a new DialogShowNote called dialog
    DialogShowNote dialog = new DialogShowNote();

    // Send the note via the sendNoteSelected method
    dialog.sendNoteSelected(mTempNote);

    // Create the dialog
    dialog.show(getFragmentManager(), "123");
}
});
```

现在，我们可以通过点击按钮召唤我们的 DialogShowNote 对话框窗口。

## 向操作栏添加 + 图标

接下来，我们将向操作栏添加 + 图标，并将其连接以显示我们的 DialogNewNote 对话框。

在 menu 文件夹中，打开 menu_main.xml 文件，并在关闭的 </menu> 标签之前添加突出显示的代码，如以下代码所示以提供上下文：

```
<!-- Our add icon will have its own button -->
<item android:id="@+id/action_add"
    android:icon="@drawable/ic_add_white_24dp"
    android:title="@string/action_add"
    app:showAsAction="ifRoom" />
</menu>
```

这将把我们之前在收集资源时下载的图标添加到操作栏中。

最后，回到 MainActivity.java，在 return 语句之前将此代码添加到 onOptionsItemSelected 方法中。当点击 + 图标时，这将显示一个新对话框来添加新笔记：

```
if (id == R.id.action_add) {
    DialogNewNote dialog = new DialogNewNote();
    dialog.show(getFragmentManager(), "");
    return true;
}
```

现在我们可以运行应用了。点击 + 图标并添加一条笔记。然后，点击“显示笔记”按钮以在对话框窗口中查看它。请注意，如果添加第二条笔记，它将覆盖第一条，因为我们只有一个 Note 对象。

## 摘要

在本章中，我们使用 FragmentDialog 类查看并实现了带有对话框窗口的常见用户界面设计。

当我们启动 Note To Self 应用并实现可以从用户那里捕获信息的更复杂的对话框时，我们更进一步。我们看到 DialogFragment 基本上使我们能够在对话框中拥有任何喜欢的 UI。

在下一章中，我们将通过探索 Java 数组及其近亲 ArrayList 来解决用户只能有一条笔记这一明显问题。

## 第 13 章. 处理和显示数组数据

在本章中，您将了解 Java 数组，它们允许我们以有组织且高效的方式处理潜在的大量数据。我们还将使用与 Java 数组关系密切的 ArrayList，并了解它们之间的区别。

一旦我们熟悉处理大量数据，我们将看到 Android API 提供了什么，可以帮助我们轻松地将新掌握的数据处理技能连接到用户界面，而无需费力。

本章的主题如下：

- 一个随机的小插曲
- 使用数组处理大量数据
- 简单的数组示例
- 动态数组示例
- 多维数组示例
- ArrayLists
- 增强型 for 循环
- ListViews 和 BaseAdapter

首先，让我们看看一个随机的小插曲

### 一个随机的小插曲

有时在我们的应用中，我们会需要一个随机数。随机数有很多可能的用途，例如，当我们的应用想要显示每日随机提示、一个必须在场景之间进行选择的游戏，或者一个提出随机问题的测验时。

Random 类是 Java API 的一部分，与我们的 Android 应用完全兼容。

让我们看看如何创建随机数，本章稍后我们将将其付诸实践。所有艰苦的工作都由 Random 类完成。首先，我们需要创建一个 Random 类型的对象：

```
Random randGenerator = new Random();
```

然后，我们使用新对象的 nextInt 方法在某个范围内生成随机数。

此行代码使用我们的 Random 对象生成随机数，并将结果存储在 ourRandomNumber 变量中：

```
int ourRandomNumber = randGenerator.nextInt(10);
```

我们为范围输入的数字从 0 开始。因此，上一行将生成 0 到 9 之间的随机数。如果我们想要 1 到 10 之间的随机数，只需使用以下内容：

```
ourRandomNumber ++;
```

我们还可以使用 Random 对象通过 nextLong、nextFloat 和 nextDouble 获取其他类型的随机数。

本章稍后，我们将使用一个快速地理测验应用将 Random 类付诸实践。


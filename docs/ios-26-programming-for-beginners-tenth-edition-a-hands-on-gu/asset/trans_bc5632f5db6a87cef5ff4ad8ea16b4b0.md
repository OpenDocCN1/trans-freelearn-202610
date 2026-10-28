
- 5. 构建并运行你的应用，然后轻点文本字段。如果软件键盘没有出现，请从模拟器的 I/O 菜单中选择 Keyboard | Toggle Software Keyboard：

![](img/0972ba626734a3986e095c788777818e_353_0.png)

图 16.12：模拟器 I/O 菜单，选中 Keyboard | Toggle Software Keyboard

- 6. 使用软件键盘在文本字段或文本视图中输入一些文本：

![](img/0972ba626734a3986e095c788777818e_353_1.png)

图 16.13：显示软件键盘的模拟器

```
let textViewText = bodyTextView.text ?? ""
saveButton.isEnabled = !textFieldText.isEmpty &&
!textViewText.isEmpty
}
```

private 关键字表示此方法仅可在该类内部访问。

- 5. 在 `textFieldShouldReturn(_:)` 方法之后添加如下 `UITextFieldDelegate` 方法：

```
func textFieldShouldReturn(_ textField: UITextField) -> Bool {
    textField.resignFirstResponder()
    return true
}
func textFieldDidEndEditing(_ textField: UITextField) {
    updateSaveButtonState()
}
```

此方法在文本字段放弃第一响应者状态后调用 `updateSaveButtonState()`。

- 6. 在 `textView(_:shouldChangeTextIn:replacementText:)` 方法之后添加如下 `UITextViewDelegate` 方法：

```
func textView(_ textView: UITextView, shouldChangeTextIn range: NSRange, replacementText text: String) -> Bool {
    if (text == "\n") {
        textView.resignFirstResponder()
    }
    return true
}
func textViewDidEndEditing(_ textView: UITextView) {
    updateSaveButtonState()
}
func textViewDidChange(_ textView: UITextView) {
    updateSaveButtonState()
}
```

至此，`JournalEntryDetailViewController` 文件已创建，其中包含 `JournalEntryDetailViewController` 类声明。现在，你需要为点击 Journal List 屏幕上的表格视图单元格时呈现的视图控制器场景设置标识。请按以下步骤操作：

- 1. 在 Project navigator 中点击 Main storyboard 文件，并在文档大纲中选择 Entry Detail Scene。
- 2. 点击 Identity inspector 按钮，然后在 Custom Class 下将 Class 设置为 `JournalEntryDetailViewController`：

![](img/0972ba626734a3986e095c788777818e_358_0.png)

图 16.16：Entry Detail Scene 的 Identity inspector 设置

太棒了！在下一节中，你将把 Entry Detail Scene 中的用户界面元素连接到 `JournalEntryDetailViewController` 类中的 outlet。这样，`JournalEntryDetailViewController` 实例将能够显示日志条目的详细信息。

## 将 UI 元素连接到 JournalEntryDetailViewController 类

目前，Journal Entry Detail 屏幕的 `JournalEntryDetailViewController` 实例无法与其内部的 UI 元素通信。现在，你将在 `JournalEntryDetailViewController` 类中创建 outlet，并将 Entry Detail Scene 中相应的 UI 元素分配给每个 outlet。请按以下步骤操作：

- 1. 在 Project navigator 中，点击 `JournalEntryDetailViewController` 文件，并删除花括号之间的所有代码，只保留 `viewDidLoad()` 方法。
- 2. 在左花括号后向 `JournalEntryDetailViewController` 类中添加以下属性：

```
// MARK: - Properties
@IBOutlet var dateLabel: UILabel!
@IBOutlet var titleLabel: UILabel!
@IBOutlet var bodyTextView: UITextView!
@IBOutlet var photoImageView: UIImageView!
```

- 3. 点击 Main storyboard 文件，并在文档大纲中选择 Entry Detail Scene。
- 4. 点击 Connections inspector 按钮，并从 `dateLabel` outlet 拖到 Entry Detail 场景中的第一个标签：

![](img/0972ba626734a3986e095c788777818e_359_0.png)

图 16.17：Connections inspector 显示 dateLabel outlet

- 5. 从 `titleLabel` outlet 拖到 Entry Detail 场景中的第二个标签：

![](img/0972ba626734a3986e095c788777818e_359_1.png)

图 16.18：Connections inspector 显示 titleLabel outlet

- 6. 从 `bodyTextView` outlet 拖到 Entry Detail 场景中的文本视图：

![](img/0972ba626734a3986e095c788777818e_360_0.png)

图 16.19：Connections inspector 显示 bodyTextView outlet

- 7. 从 `photoImageView` outlet 拖到 Entry Detail 场景中的图像视图：

![](img/0972ba626734a3986e095c788777818e_360_1.png)

图 16.20：Connections inspector 显示 photoImageView outlet

> 请记住，如果出错，你可以点击 x 按钮断开连接，然后再次从 outlet 拖到 UI 元素上。

现在，你已成功将 **Entry Detail 场景**中的 UI 元素连接到 `JournalEntryDetailViewController` 类中的 outlet。在下一节中，你将实现代码，以便当用户点击 Journal List 屏幕上的表格视图单元格时显示 `JournalEntry` 实例的详细信息。

## 显示日志条目的详细信息

到目前为止，你已经实现了 `JournalEntryDetailViewController` 类，并将此类中的 outlet 连接到 **Entry Detail 场景**中的标签、文本视图和图像视图。当用户点击 Journal List 屏幕上的表格视图单元格时，你将根据数据源获取对应的 `JournalEntry` 实例，并将其传递给 `JournalEntryDetailViewController` 实例，以便在 Journal Entry Detail 屏幕上显示。为此，你将在 `JournalListViewController` 类中实现 `prepare(for:sender:)` 方法。请按以下步骤操作：

- 1. 在 Project navigator 中，点击 **Main** storyboard 文件，并点击连接 **Journal** 场景和 **Entry Detail** 场景的 segue。点击 Attributes inspector 按钮，然后在 **Storyboard Segue** 下将 **Identifier** 设置为 `entryDetail`：

![](img/0972ba626734a3986e095c788777818e_361_0.png)

图 16.21：Attributes inspector 显示 Identifier 设置为 entryDetail

稍后你将使用此标识符来识别从 Journal List 屏幕转到 Journal Entry Detail 屏幕所用的 segue。

我们来分解一下：

```
guard segue.identifier == "entryDetail" else { return }
```

这段代码用于检查是否正在使用正确的 segue；如果不是，方法将退出。

```
guard let journalEntryDetailViewController = segue.destination as?
JournalEntryDetailViewController,
let indexPath = tableView.indexPathForSelectedRow else {
    fatalError("Could not get indexPath")
}
```

这段代码检查目标视图控制器是否是 `JournalEntryDetailViewController` 的实例，获取用户点击的表格视图单元格，并获取该单元格的索引路径。

```
let selectedJournalEntry = journalEntries[indexPath.row]
```

此语句从 `journalEntries` 数组中获取对应的 `JournalEntry` 实例。

```
journalEntryDetailViewController.selectedJournalEntry =
selectedJournalEntry
```

此语句将 `JournalEntry` 实例赋值给目标视图控制器的 `selectedJournalEntry` 属性。

现在，你已添加了代码，当从 Journal List 屏幕转到 Journal Entry Detail 屏幕时，会将用户点击的表格视图单元格对应的日志条目传递给 `JournalEntryDetailViewController` 实例。在下一节中，你将修改 `JournalEntryDetailViewController` 类以显示日志条目的详细信息。

## 修改 JournalEntryDetailViewController 以显示所选日志条目的详细信息

当转到 Journal Entry Detail 屏幕时，与用户点击的表格视图单元格对应的 `JournalEntry` 实例将被赋值给 `JournalEntryDetailViewController` 实例的 `selectedJournalEntry` 属性。你将在 `JournalEntryDetailViewController` 类中添加代码来访问此属性并显示日志条目的详细信息。请按以下步骤操作：

- 1. 在 Project navigator 中，点击 `JournalEntryDetailViewController` 文件，并按如下所示修改 `JournalEntryDetailViewController` 类的 `viewDidLoad()` 方法，以显示日志条目的详细信息：

```
override func viewDidLoad() {
    super.viewDidLoad()
    dateLabel.text = selectedJournalEntry?.date.formatted(
        .dateTime.day().month(.wide).year()
    )
    titleLabel.text = selectedJournalEntry?.title
    bodyTextView.text = selectedJournalEntry?.body
    photoImageView.image = selectedJournalEntry?.photo
}
```

如你所见，之前传递给此视图控制器的 `JournalEntry` 实例的属性被用来填充用户界面元素。请注意，`date` 属性需要先格式化为字符串，然后才能赋值给 `dateLabel.text` 属性。

- 2. 构建并运行你的项目，然后点击一个表格视图单元格。与该表格视图单元格对应的日志条目的详细信息将显示在 Journal Entry Detail 屏幕上：


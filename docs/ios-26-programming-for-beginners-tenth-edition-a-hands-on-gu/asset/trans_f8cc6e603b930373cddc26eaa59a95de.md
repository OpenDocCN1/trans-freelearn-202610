

```swift
let photo2 = UIImage(systemName: "cloud")
let photo3 = UIImage(systemName: "cloud.sun")
guard let journalEntry1 = JournalEntry(rating: 5, title: "Good", body: "Today is a good day", photo: photo1) else {
    fatalError("Unable to instantiate journalEntry1")
}
guard let journalEntry2 = JournalEntry(rating: 0, title: "Bad", body: "Today is a bad day", photo: photo2) else {
    fatalError("Unable to instantiate journalEntry2")
}
guard let journalEntry3 = JournalEntry(rating: 3, title: "OK", body: "Today is an OK day", photo: photo3) else {
    fatalError("Unable to instantiate journalEntry3")
}
return [journalEntry1, journalEntry2, journalEntry3]
}
```

该扩展包含一个 `createSampleJournalEntryData()` 方法，该方法使用来自 Apple SFSymbols 库的符号创建三个 `UIImage` 实例，创建三个 `JournalEntry` 实例，将它们添加到一个数组中并返回该数组。`static` 关键字表示它是在 `JournalEntry` 类型上的方法，而不是 `JournalEntry` 实例方法。

要了解有关类型方法和实例方法的更多信息，请参阅此链接：https://docs.swift.org/swift-book/documentation/the-swift-programming-language/methods/。

您现在已完成 `JournalEntry` 类的实现。您还添加了一个静态方法来生成三个示例日记条目。在下一节中，您将修改 `JournalListViewController` 类以使用此方法返回的数组来填充表格视图。

按照以下步骤操作：

- 1. 在项目导航器中，右键单击 View 文件夹并选择 New File from Template....
- 2. iOS 应该已经选中。选择 Cocoa Touch Class，然后单击 Next：

    ![](img/0972ba626734a3986e095c788777818e_328_0.png)

    图 15.15：选择新文件模板的屏幕

    使用 Cocoa Touch Class 模板将允许您设置超类并自动为将要创建的类插入样板代码。

- 3. 将出现“为新文件选择选项”屏幕：

    ![](img/0972ba626734a3986e095c788777818e_329_0.png)

    图 15.16：为新文件选择选项的屏幕

    按如下方式配置类：

    - Class: JournalListTableViewCell
    - Subclass of: UITableViewCell
    - Also create XIB: 未选中
    - Language: Swift

    完成后单击 Next。

- 4. 单击 Create，一个名为 JournalListTableViewCell 的新文件将被添加到项目的 View 组中。在其中，您将看到以下代码：

```swift
import UIKit
class JournalListTableViewCell: UITableViewCell {
    override func awakeFromNib() {
        super.awakeFromNib()
        // Initialization code
    }

    override func setSelected(_ selected: Bool, animated: Bool) {
        super.setSelected(selected, animated: animated)
        // Configure the view for the selected state
    }
}
```

- 5. 删除 `JournalListTableViewCell` 类声明中的所有代码，如下所示：

```swift
class JournalListTableViewCell: UITableViewCell {
}
```

- 6. 要创建对应于 journalCell 表视图单元格的子视图的三个属性，请在 `JournalListTableViewCell` 类声明的大括号之间输入以下代码：

```swift
// MARK: - Properties
@IBOutlet var photoImageView: UIImageView!
@IBOutlet var dateLabel: UILabel!
@IBOutlet var titleLabel: UILabel!
```

- 7. `JournalListTableViewCell` 类的实现已完成。现在您将把此类分配为 journalCell 表视图单元格的标识。单击项目导航器中的 Main storyboard 文件并在文档大纲中单击 Journal Scene 下的 journalCell：

    ![](img/0972ba626734a3986e095c788777818e_330_0.png)

    图 15.17：文档大纲显示 journalCell 已选中

- 8. 单击 Identity inspector 按钮。在 Custom Class 部分，将 Class 设置为 JournalListTableViewCell。这将把 JournalListTableViewCell 实例设置为 journalCell 的自定义表视图子类。完成后按 Return：

    图 15.18：Identity inspector 显示 journalCell 的 Class 设置

您刚刚声明并定义了 `JournalListTableViewCell` 类，并将其分配为 journalCell 表视图单元格的自定义表视图单元格子类。在下一节中，您将把此类连接到 journalCell 表视图单元格中的图像视图和标签，以便您可以控制它们的显示内容。

## 连接 journalCell 中的 outlets

为了管理 Journal List 屏幕中表视图单元格的显示内容，您将使用 Connections inspector 将 journalCell 表视图单元格中的图像视图和标签连接到 `JournalListTableViewCell` 类中的 outlets。按照以下步骤操作：

- 1. 在文档大纲中选中 journalCell 时，单击 Connections inspector 按钮以显示其 outlets。

    图 15.19：Connections inspector 显示 journalCell 的 outlets

- 2. 从 photoImageView outlet 拖动到表视图单元格中的图像视图：

    ![](img/0972ba626734a3986e095c788777818e_332_0.png)

    图 15.20：Connections inspector 显示 photoImageView outlet

- 3. 从 dateLabel outlet 拖动到表视图单元格中的顶部标签。

    ![](img/0972ba626734a3986e095c788777818e_332_1.png)

    图 15.21：Connections inspector 显示 dateLabel outlet

- 4. 从 titleLabel outlet 拖动到表视图单元格中的底部标签：

    ![](img/0972ba626734a3986e095c788777818e_332_2.png)

    图 15.22：Connections inspector 显示 titleLabel outlet

请记住，如果您犯了错误，可以单击 x 按钮来断开连接，然后再次从 outlet 拖动到 UI 元素。

Main storyboard 文件中的 journalCell 表视图单元格现在已使用自定义表视图子类 `JournalListTableViewCell` 设置。表视图单元格的图像视图和标签的 outlets 也已创建并分配。现在，您将能够在应用运行时将 `photoImageView`、`dateLabel` 和 `titleLabel` outlets 设置为在每个单元格中显示照片、日期和标题。

在下一节中，您将更新 `JournalListViewController` 类中的表视图数据源方法，以提供要在表视图中显示的表视图单元格数量，以及为每个单元格提供日记条目照片、日期和标题。

## 更新 JournalListViewController 中的数据源方法

`JournalListViewController` 类中的数据源方法当前设置为显示 10 个表视图单元格，每个单元格包含一个显示笑脸的图像视图和两个标签。您将更新它们以从 `SampleJournalEntryData` 实例获取要显示的单元格数量以及要放入每个单元格的数据。按照以下步骤操作：

- 1. 在项目导航器中单击 JournalListViewController 文件。

- 2. 重新排列 `JournalListViewController` 类中的代码，使 `tableView` outlet 和 `viewDidLoad()` 方法位于表视图委托方法之前：

```swift
class JournalListViewController: UIViewController,
UITableViewDataSource, UITableViewDelegate {
@IBOutlet var tableView: UITableView!
override func viewDidLoad() {
    super.viewDidLoad()
}
func tableView(_ tableView: UITableView,
numberOfRowsInSection section: Int) -> Int {
```

- 3. 在属性声明之前添加一个 MARK 语句，如下所示：

```swift
// MARK: - Properties
@IBOutlet var tableView: UITableView!
```

- 4. 在 `viewDidLoad()` 方法之前添加一个 MARK 语句，如下所示：

```swift
// MARK: - View controller lifecycle
override func viewDidLoad() {
```

- 5. 在表视图数据源方法之前添加一个 MARK 语句，如下所示：

```swift
// MARK: - UITableViewDataSource
func tableView(_ tableView: UITableView, numberOfRowsInSection
```

- 6. 在 `unwindNewEntryCancel(segue:)` 之前添加一个 MARK 语句，如下所示：

```swift
// MARK: - Methods
@IBAction func unwindNewEntryCancel(segue: UIStoryboardSegue) {
```

- 7. 验证 `JournalListViewController` 中的代码如下所示：

```swift
class JournalListViewController: UIViewController,
UITableViewDataSource, UITableViewDelegate {
    // MARK: - Properties
    @IBOutlet var tableView: UITableView!
    // MARK: - View controller lifecycle
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    // MARK: - UITableViewDataSource
    func tableView(_ tableView: UITableView,
    numberOfRowsInSection section: Int) -> Int {
        10
    }
    func tableView(_ tableView: UITableView, cellForRowAt
    indexPath: IndexPath) -> UITableViewCell {
        tableView.dequeueReusableCell(withIdentifier:
        "journalCell", for: indexPath)
    }
    // MARK: - Methods
    @IBAction func unwindNewEntryCancel(segue:
```


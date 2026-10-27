

在某个时间点，WAL 文件中的数据必须转移回主数据库。此过程称为 **checkpoint**。通常，SQLite 会在 WAL 文件达到一定大小时（默认情况下为 1,000 页）或在最后一次数据库连接关闭时自动执行 checkpoint：

![](img/0b17a17d4cb56b1c25763a396dd83117_124_0.png)

Figure 4.8 – A checkpoint transfers data from the WAL file back into the main database

## Tip

需要知道的是，当所有与数据库的连接都已干净关闭时，SQLite 会执行自动 checkpoint。因此，如果你在使用 DB Viewer 之类的工具时启用了 WAL 关闭数据库，WAL 文件的内容将被转移到主数据库，并且 WAL 文件将被删除。

![](img/0b17a17d4cb56b1c25763a396dd83117_127_0.png)

Figure 4.11 – Using a hex viewer to display the contents of an SQLite file

通过分析数据库头以了解页面大小以及空闲页面和空闲块的位置，检查员可以使用自己选择的工具通过指定偏移量和长度来突出显示文件中的特定页面，从而有效地将该页面与剩余数据隔离开来。然后可以通过以字符串格式显示其内容来分析该页面。

以下步骤描述了手动解析 SQLite 数据库页面的过程：

- 1. 读取数据库头并识别页面大小和页面数量。
- 2. 计算每个页面的偏移位置。
- 3. 分析页面头。
- 4. 如果是活动页面，请在未分配空间或空闲块中搜索任何有用的证据。如果是空闲页面，则分析整个页面。
- 5. 对数据库中的每个页面重复此过程。

同样的原则也适用于 WAL 文件；由于文件格式有详细记录，检查员可以使用十六进制查看器打开 WAL，计算每个帧的偏移量，隔离每个单独的帧，并通过将其转换为 UTF-8 字符串来查看数据以识别任何可读字符串。

作为示例，我们将创建一个名为 test.db 的新数据库，其中包含一个名为 Fruits 的表。然后，我们将向数据库中填充几条记录，删除一行数据，并使用十六进制查看器分析文件。为简单起见，数据库将不使用预写日志。

你可以使用自己选择的软件或 sqlite3 命令行工具创建数据库。然后，运行以下查询：

```
CREATE TABLE "Fruits" (
    "ROWID"    INTEGER,
    "Fruit"    TEXT,
    PRIMARY KEY("ROWID" AUTOINCREMENT)
);
INSERT INTO "Fruits" VALUES (1,'Apple');
INSERT INTO "Fruits" VALUES (2,'Peach');
INSERT INTO "Fruits" VALUES (3,'Orange');
INSERT INTO "Fruits" VALUES (4,'Banana');
DELETE FROM Fruits WHERE ROWID == 1;
```

正如以下屏幕截图所示，表中现在包含三行，如预期那样：

![](img/0b17a17d4cb56b1c25763a396dd83117_128_0.png)

Figure 4.12 – The table contains three rows of data

我们检查的第一步是使用十六进制查看器打开文件并分析 100 字节的头，以从数据库中提取任何有用的信息：

bytes, and copy the data to a new window in your editor. From now on, the offsets will be relative to the page:

![](img/0b17a17d4cb56b1c25763a396dd83117_130_0.png)

Figure 4.14 – The page header

现在我们已将第二页加载到十六进制编辑器中，下一步涉及检查页面头：

- - 在偏移量 `01` 处，我们可以读取第一个空闲块的位置，即 `0x0FF6` 或十进制格式的 `4086`。
- - 偏移量 `04` 处的值表示页面上的活动单元格数量，`3`。
- - 通过查看偏移量 `05`，我们可以找到第一个非空数据单元格的位置，即 `0x0FD6` 或 `4054`。
- - 最后，头与第一个数据单元格之间的空间是未分配空间。

有了这些信息，我们可以突出显示从偏移量 4086 开始并延伸到页面底部的空闲块；十六进制查看器的右窗格将数据显示为 UTF-8 字符串：

![](img/0b17a17d4cb56b1c25763a396dd83117_131_0.png)

Figure 4.15 – The free block shows previously deleted data

正如你所见，之前从数据库中删除的名为 Apple 的记录仍然存在于文件中，在第二页的空闲块的右侧。

## Carving for strings

数据雕刻是搜索文件中特定字符串或字节的过程；检查员首先确定要搜索的字符串或模式，然后使用十六进制查看器或命令行工具在文件中搜索该字符串或模式。此过程是手动解析 SQLite 数据库的绝佳替代方案，因为它允许调查人员立即验证给定记录是否可以在文件中找到，包括已删除数据通常隐藏的位置。

strings 工具是一个命令行实用程序，可在二进制文件中执行字符串雕刻并将结果打印到屏幕上。显示 SQLite 数据库中可以找到的所有字符串与运行以下命令一样简单：

```
strings sms.db
```

此工具可以与其他命令（如 grep）结合使用，以搜索特定字符串或模式。以下命令将在我们之前示例中使用的数据库中搜索 Apple 单词：

```
strings test.db | grep Apple
Apple
```

正如你所见，Apple 字符串被打印到屏幕上，这证实了它在 SQLite 数据库中被找到。这个简单的工具可以成为显示数据库中存储的所有字符串的救命技巧，包括已删除的数据！

## Recovering deleted records using FQLite

手动从 SQLite 数据库中恢复已删除数据的任务可能令人望而生畏；幸运的是，大多数商业取证工具都内置了此功能，但也有其他选择。截至撰写本文时，FQLite 可能是最完整的开源 SQLite 恢复工具，原因如下：

- - 它是用 Java 编写的，因此你可以在任何操作系统上运行它。
- - 它支持从所有数据库页面恢复已删除记录，包括空闲块和未分配空间。
- - 它内置支持 WAL 文件。
- - 恢复的记录可以以 CSV 格式导出。

要下载 FQLite，请访问 https://www.staff.hs-mittweida.de/~pawlaszc/fqlite/ 或前往位于 https://github.com/pawlaszczyk/fqlite 的 GitHub 仓库。

我们现在将简要介绍从 sms.db SQLite 数据库中恢复已删除数据的过程：

- 1. 启动 FQLite 并通过转到 File | Open Database 选择要尝试恢复的数据库。
- 2. FQLite 现在将处理文件并分析数据库，搜索已删除记录。你可以通过查看屏幕底部的日志面板来跟踪进度。
- 3. FQLite 完成后，从左侧面板中选择要检查的数据库或 WAL 文件。
- 4. FQLite 将显示一个包含数据库中找到的所有数据的表，包括已删除记录：

![](img/0b17a17d4cb56b1c25763a396dd83117_133_0.png)

Figure 4.16 – FQLite recovers deleted records from an SQLite database

到目前为止，你应该对 SQLite 的工作原理、调查人员如何通过命令行工具检查数据库以及数据恢复的最常见技术有了良好的理解。我们现在将更进一步，查看另一种流行的数据结构。

## Working with property lists

虽然 Protobufs 在 iOS 设备上并不常见，但仍有一些应用程序使用它们来存储数据，因此调查人员应至少对如何处理这些文件有一个基本的了解。

由于 protobuf 以二进制格式存储，因此它们不可读；protoc 命令行工具允许你显示文件的内容，并让你大致了解数据的结构。

## Tip

protoc 工具是 protocol buffers 包的一部分，可以从位于 https://github.com/protocolbuffers/protobuf 的 GitHub 仓库下载。

安装 protoc 工具后，可以通过运行以下命令将 protobuf 的内容打印到屏幕上：

```
protoc --decode_raw < [protobuf_blob_file]
```

使用协议缓冲区的应用程序示例包括 Apple **Notes**、Apple **Maps** 和 **Spotify** 应用程序。

现在我们对工件的外观以及如何手动分析它们有了更好的理解，下一个部分将提供 iOS 设备上最常见的证据位置的详尽列表，以及我们可以期望获得的见解。

## Locating common artifacts

与应用程序相关的工件对于 iOS 调查至关重要，但设备设置、日志和用户生成的内容也很重要。在本章的最后一部分中，我们将研究 iOS 文件系统中的一些常见位置，检查员可以快速查看以收集更多信息：


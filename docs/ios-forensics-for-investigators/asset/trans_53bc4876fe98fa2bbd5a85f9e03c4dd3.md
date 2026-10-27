

![](img/0b17a17d4cb56b1c25763a396dd83117_223_0.png)

Figure 8.5 – Extracting message data from sms.db

首先需要记住的是，自 **iOS 11** 起，Apple 开始将时间戳存储为 18 位数字，而此前消息时间戳仅为九位数字长。这意味着 sms.db 数据库很可能同时包含九位和 18 位时间戳。我们的查询首先验证时间戳的长度，并据此显示消息发送的日期和时间。然后，我们提取 text 列和 service 列，后者指示消息是传统 SMS 还是 iMessage。account 和 account_login 列显示用户发送或接收消息时使用的电话号码或电子邮件地址。chat_identifier 列存储用户正在通信的对方的电话号码或电子邮件地址。最后，Date Read、Message Direction 和 Last Read 列不言自明。

在涉及 SMS 和 iMessage 取证的调查中，不应忽视 Drafts 和 Attachments 文件夹，因为这些可能涉及 WhatsApp 语音通话和视频通话。这些数据库也包含在本地（iTunes）备份中。

- **Telegram** 是一款跨平台即时通讯应用，兼容包括桌面电脑在内的大量设备。聊天和消息同时存储在本地设备和 Telegram 服务器上。还可选择创建秘密聊天，其中数据永远不会存储在应用服务器上。Telegram 证据不包含在本地或 iCloud 备份中，只能通过完整文件系统获取获得。证据和日志可找到在 `/private/var/mobile/Containers/Data/Application/<APP_GUID>/` 和 `/private/var/mobile/Containers/Shared/AppGroup/<APP_GUID>/`。聊天和消息，包括秘密聊天，可在 `db_sqlite` 数据库中找到。

- **Signal** 是一款适用于移动设备和桌面电脑（包括 Linux）的安全加密消息应用。Signal 上的所有通信，如消息、群组聊天、照片和文件传输，均采用端到端加密。Signal 对话不会存储在其服务器上，不会在本地备份中提供，也无法将数据备份到 iCloud。Signal 的数据库以加密形式存储在本地设备上。访问这些证据的唯一方法是执行完整文件系统获取并解密钥匙串以访问加密密钥，进而解密数据库。日志文件和用户偏好设置存储在 `/private/var/mobile/Containers/Data/Application/<APP_GUID>/`。加密数据库 `signal.sqlite` 可在 `/private/var/mobile/Containers/Shared/AppGroup/<APP_GUID>/` 找到。

在本节中，我们简要讨论了第三方消息应用，并了解了这些应用如何以及在何处存储对话数据。在本章的最后一部分，我们将学习如何尝试恢复已删除的消息。

### Recovering deleted messages

在移动设备调查中最常见的任务之一是尝试恢复用户删除的数据，例如聊天和消息，因为这些可能包含无价的证据。

在本章中，我们了解到 SMS 消息和来自第三方应用的聊天都存储在 SQLite 数据库中；这意味着恢复已删除聊天的能力有效地取决于能否从 SQLite 数据库中恢复任何记录。

在第 4 章《Working with Common iOS Artifacts》中，我们讨论了恢复已删除记录的几种选项，例如：

- 使用十六进制查看器解析数据库
- 字符串 carving
- 分析预写日志 Write Ahead Log (WAL) 文件
- 使用 FQLite 等取证工具从空闲块和未分配空间中恢复数据

所有这些解决方案都可以有效恢复已删除消息，但前提是记录从数据库中清除后数据库未被 vacuum。

**Tip**

需要注意的是，要尝试数据恢复，数据库应通过完整文件系统获取提取，而不是逻辑获取，因为后者将包含一个在备份过程中从头重建的已 vacuum 数据库副本。研究还表明，自 iOS 11 以来，sms.db 数据库会定期进行 vacuum，因此 SMS 消息和 iMessages 可能在检查员有机会获取设备之前就被清除。

在下一节中，我们将介绍一种可用于恢复已删除消息的开源工具。

### Detecting deleted messages using Mirf

有时恢复已删除消息根本不可能；然而，在这种情况下，即使无法恢复其内容，也可能检测到用户删除了消息这一事实。

聊天和消息以单个记录的形式存储在 SQLite 表中；每个记录通过一个唯一的、递增的标识符进行识别，称为 **primary key**。例如，在以下屏幕截图中，该截图包含从 WhatsApp ChatStorage.sqlite 数据库提取的数据，单个消息使用存储在 Z_PK 列中的标识符引用：

![](img/0b17a17d4cb56b1c25763a396dd83117_227_0.png)

Figure 8.6 – The Z_PK column stores a unique identifier for each message

通过分析 Z_PK 列中的值，检查员可以查找任何缺失的记录：在前一个示例中，与值 5 关联的记录丢失。这足以说明一条消息已被删除。即使无法恢复实际消息，也可以通过查看前后记录的时间戳来推断消息发送或接收的大致时间范围。值得庆幸的是，这一过程可以通过使用名为 **Missing Record Finder for SQLite Databases (Mirf)** 的开源 Python 脚本实现自动化，该脚本可通过发现间隙来帮助检测是否删除了任何数据行。

![](img/0b17a17d4cb56b1c25763a396dd83117_229_0.png)

Figure 8.7 — Running Mirf to detect deleted messages

在这种情况下，识别出一条缺失记录，包括 UTC 时区的日期范围和缺失记录编号。

### Summary

在本章中，我们了解了 iOS 如何在设备上存储电子邮件和消息，以及调查人员可以期望找到哪些证据。

首先，我们介绍了 Apple Mail 应用并发现了电子邮件的存储位置。我们分析了相关的 SQLite 数据库以提取电子邮件元数据，并学习了如何解析 EML 文件以查看电子邮件正文。

本章后半部分，我们通过查看 iOS 如何在 sms.db 数据库中存储 SMS 和 iMessage 数据以及如何使用 SQL 查询提取所有消息及其元数据，专注于消息取证。然后，我们介绍了第三方消息应用的主题，并重点关注其中三个：WhatsApp、Telegram 和 Signal。我们了解了可以从这些应用中提取哪些数据以及最相关的 SQL 数据库。

最后，在本章的最后一节中，我们讨论了尝试恢复已删除消息的不同选项，例如使用名为 Mirf 的开源工具，该工具可检测 SQL 表中记录之间的间隙。

在下一章中，我们将学习有关照片、视频和音乐取证的所有内容。

## Chapter 9: Photo, Video, and Audio Forensics

在本书的前几章中，我们专注于识别和分析存储在系统生成证据中的移动证据，例如数据库、PLIST 和日志文件。然而，任何类型的调查通常还涉及分析设备上找到的用户生成内容，例如媒体文件。

现代 iOS 设备可能包含数万个媒体文件，每个文件都关联着对调查至关重要的独特元数据。在本章中，我们将学习如何识别和分析照片、视频和音频文件等多媒体内容。

我们将从媒体取证的介绍开始，讨论照片、视频和音频录音在 iOS 设备上的存储位置。然后，我们将学习如何分析媒体元数据以获得有意义的见解，无论是从文件本身还是从相应的数据库。在本章的最后一部分，我们将学习如何分析用户行为，以了解用户通过 Apple 应用（如 Music 和 Safari）以及第三方应用（如 Spotify 或 Netflix）访问或观看了哪些媒体。

在本章中，我们将涵盖以下主题：

- Introducing media forensics
- Analyzing photos and videos
- Introducing EXIF metadata
- Analyzing user viewing activity

### Introducing media forensics


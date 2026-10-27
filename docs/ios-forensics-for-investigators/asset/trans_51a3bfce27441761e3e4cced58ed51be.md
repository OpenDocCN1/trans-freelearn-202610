

| Path | Description |
|---|---|
| installd/Library/MobileInstallation/LastBuildInfo.plist | OS version |
| mobile/Library/Preferences/com.apple.springboard.plist | Order of apps |
| mobile/Library/Preferences/com.apple.mobilegestalt.plist | Device name |
| mobile/Library/Preferences/com.apple.Preferences.plist | Disk usage |
| mobile/Library/Preferences/com.apple.purplebuddy.plist | Language |
| mobile/Library/Preferences/com.apple.commcenter.shared.plist | Phone number |
| mobile/Library/Preferences/com.apple.corerecents.recentsd.plist | iCloud data |
| preferences/SystemConfiguration/com.apple.networkidentification.plist | Network data |
| preferences/SystemConfiguration/NetworkInterfaces.plist | WiFi data |
| preferences/SystemConfiguration/preferences.plist | Cellular + WiFi |

表 4.2 – 常见的 iOS 设备 artifacts

现在我们知道了在哪里查找设置和日志，接下来我们将快速了解一下用户生成的数据，例如消息或通话存储在哪里。表 4.3 说明了一些最受欢迎的文件。像往常一样，路径均相对于 /private/var/：

| Path | Description |
|---|---|
| mobile/Library/Mail/Recents.db | Recent SMS and email metadata |
| mobile/Media/Recordings/Recordings.db | Audio recordings |
| mobile/Media/PhotoData/Photos.sqlite | Photo categorization |
| mobile/Library/CallHistoryDB/CallHistory.storedata | Call log |
| mobile/Library/SMS/sms.db | SMS/MMS/iMessage |
| mobile/Library/Safari/History.db | Browsing history |
| mobile/Media/DCIM/* | Photos |
| mobile/Library/Accounts/Accounts3.sqlite | Account details |

表 4.3 – 常见的 iOS 用户 artifacts

需要理解的是，这些表格仅包含一些最受欢迎的 artifacts，但还有很多、很多其他可能对调查有价值的文件。检查员应始终深入查看整个文件系统，以从数据中获得所有可用的见解。

## Summary

在本章中，我们学习了在 iOS 设备上发现的最常见类型的 artifacts 以及如何分析这些文件。

首先，我们介绍了数据库并学习了 SQLite 如何将数据组织成表、列和行。然后，我们深入研究了 SQLite 的内部架构，以了解记录在更低层级是如何组织的以及已删除记录可以在哪里找到。我们讨论了尝试恢复已删除数据的可能选项，例如通过十六进制查看器手动分析数据库或使用诸如 FQLite 之类的工具。

在章节的后面部分，我们谈到了属性列表和协议缓冲区，它们也可以以 XML 或二进制格式在 iOS 设备上找到。为了可读，这些文件首先需要解码。

最后，在本章的最后一部分，我们概述了一些 artifacts 可能存在于的常见位置，并列出了在每次调查中都应考虑的一些基本文件。在下一章中，我们将讨论行为模式取证。

## Chapter 5: Pattern-of-Life Forensics

在上一章中，我们了解了可以在 iOS 设备上找到的不同类型的 artifacts，例如 SQLite 数据库和 plists，以及如何手动分析这些文件。在本章中，我们将利用这些知识来处理从取证角度来看最有趣的数据库，例如 **KnowledgeC** 数据库，它是行为模式取证的首选解决方案。

行为模式数据关乎设备所有者在日常生活中进行的习惯。对于智能手机而言，这包括在任何给定时间使用了哪些应用以及使用了多长时间，设备何时被解锁，电池温度是多少，以及用户正在浏览哪个网页。

我们将从定义行为模式取证开始，以便更好地理解在 iOS 设备调查中可能遇到的哪种数据。然后，我们将讨论时间戳以及如何在不同时区之间转换。然后，我们将深入研究 KnowledgeC、电源日志和设备事件，并学习如何识别用户交互。最后，我们将介绍 Apollo，这是一款开源工具，大大简化了定位和审查行为模式 artifacts 的过程。

在本章中，我们将涵盖以下主题：

- Introducing pattern-of-life forensics
- Working with timestamps
- Logs, events, and user interaction
- Introducing Apollo

### Introducing pattern-of-life forensics

如果你从未有机会仔细查看 iOS 设备的行为模式数据，你可能会对在正常智能手机使用过程中收集、存储和处理的数据量和种类感到惊讶。通常，这些数据被操作系统用来为用户提供个性化推荐，例如 Siri 的建议。

行为模式取证到底是什么？这些数据全部关乎 **user behavior** 和 **device usage**，它使调查人员能够回答以下问题：

- 用户使用设备做什么？
- 他们访问了什么？
- 他们与谁进行了通信？

从取证角度来看，分析行为模式数据非常有用，不仅可以识别在给定时间段内设备上执行的所有活动，还可以构建设备所有者的画像；通过分析 iOS 收集的数据，调查人员将深入了解所有者的习惯，例如用户醒来时使用哪些应用，设备最常连接哪些 Wi-Fi 网络，网页历史记录和通知。构建可被视为正常的活动画像使调查人员能够立即检测到可能对调查感兴趣的 **patterns of abnormal behavior**。

令人惊讶的是，行为模式数据在取证界得到的关注很少，主要是因为从设备获取相关 SQLite 数据库的挑战；这些数据在设备正常使用期间无法访问，只能通过 **full filesystem acquisition** 访问。显然，这限制了可以从中提取此类数据的设备范围，但 **checkm8 exploit** 和其他依赖公共软件漏洞的 **agent-based acquisitions** 等技术使调查人员能够从大多数 iOS 设备中访问用户活动和行为。

### Meaningful SQLite databases

以下是与行为模式取证相关的最常见数据库列表。我们将在本书后面对每个数据库进行深入研究：

- 用户和设备活动的主要来源是 `KnowledgeC.db` SQLite 数据库，它可以在 macOS 和 iOS 设备上找到。在 iOS 上，它位于 `/private/var/mobile/Library/CoreDuet/Knowledge/`，只能通过完整文件系统获取来访问。该数据库通常包含数十万条与设备事件和用户活动相关的记录。

- `interactionC.db` SQLite 数据库跟踪设备所有者与其联系人之间的最近交互，例如短信或通话。目前尚不清楚该数据库的保留期是多少，因为不同设备之间的数据不一致，但一般来说，调查人员会找到最近六个月的条目。虽然 `interactionC.db` 不包含对话的实际内容，但它包含可以与其他 artifacts 相关联的元数据。数据库可以在 `/private/var/mobile/Library/CoreDuet/People/` 找到。

- 位于 `/private/var/containers/Shared/SystemGroup/UUID/Library/BatteryLife/` 的 `CurrentPowerLog.PLSQL` 文件跟踪与电池寿命相关的数据，例如使用情况和温度。

- 另一个可以帮助调查人员了解设备在特定时间点运行了哪些进程的有趣数据库是 `DataUsage.sqlite` 数据库，它位于 `/private/var/wireless/Library/Databases/`。该数据库存储每个进程的网络使用统计信息。




![](img/0b17a17d4cb56b1c25763a396dd83117_72_0.png)

**图 2.17 – 选择完整文件系统（checkm8）选项**

3. 接下来，设备必须关机。关机后，你需要进入 **恢复模式**，然后从那里将设备置于 **DFU 模式**。屏幕上的说明相当准确，但基本上，设备关机后，你应按住 **Home** 按钮同时连接设备。这会将设备置于 **恢复模式**。从那里，按住 **电源** 按钮和 **Home** 按钮 10 秒，然后松开 **电源** 按钮，同时继续按住 **Home** 按钮几秒。此操作可能因设备型号而异。一旦设备进入 DFU 模式，**继续** 按钮将亮起：

![](img/0b17a17d4cb56b1c25763a396dd83117_73_0.png)

图 2.18 – UFED 将显示如何进入恢复模式和 DFU 模式的说明

4. 此时，UFED 将利用 Checkm8 漏洞对设备进行利用。这可能需要几分钟。整个过程在设备的易失性内存中执行，因此设备本身不会发生任何更改，并且在采集过程结束时将恢复正常启动：

![](img/0b17a17d4cb56b1c25763a396dd83117_73_1.png)

图 2.19 – 利用设备可能需要几分钟

5. 一旦设备被成功利用，采集阶段将开始，设备将显示进度指示器：

![](img/0b17a17d4cb56b1c25763a396dd83117_74_0.png)

图 2.20 – 采集进行中

6. 完整文件系统将被镜像并归档为 DAR 文件，其类似于 TAR 归档：

![](img/0b17a17d4cb56b1c25763a396dd83117_75_0.png)

图 2.21 – 提取结果将归档为 DAR 文件

采集过程结束时，设备将自动正常重启。生成的 DAR 归档可以通过导入到 Cellebrite Physical Analyzer 或你选择的工具进行分析。

### 基于代理的完整文件系统采集

Elcomsoft iOS Forensic Toolkit 是少数支持基于代理采集的工具之一。由于此方法需要将应用程序（代理）侧载到设备上，因此你需要一个 Apple ID。在 Windows 版本中，你需要一个 Apple Developer ID，而在 macOS 上，你可以使用传统账户：

1. 开始时，启动工具包并检查设备是否被正确识别：

图 2.22 – Elcomsoft iOS Forensic Toolkit 主屏幕

2. 从菜单中选择选项 1，这将把代理安装到设备上。你将被要求提供 Apple ID 凭据，并且需要通过双重身份验证。然后，代理将使用你的 Apple ID 进行签名并安装到设备上：

```
Toolkit.command

|---------------------------------------------------------------|
|                                                               |
|               Elcomsoft iOS Forensic Toolkit 7.02            |
|                                                               |
|                     (c) 2011-2021 Elcomsoft Co. Ltd.         |
|                                                               |
|---------------------------------------------------------------|

[Please enter Apple ID (enrolled in the Apple Development Program is preferred): ]
[Please enter Apple ID password (normal, not app-specific): ]
Device with udid 7cf85b3179a6c136ae723636e6467a27888ab928 has been detected.
Device Name: iPhone
Device Model: iPhone7,2
OS Version: iPhone OS 12.4.5
Serial Number: F78PNPSKGSMT
Build Version: 16G161

Input 2fa 6-digit code sent to your devices OR type 0 to show another options: 868276
Individual team Effe Lab (59C356955L) will be used for installing an application on the device.
Please note signing the application by this team profile will require approval on the device
Certificate signing request will be submitted to the account.
Device will be enrolled in the Apple Developer Program.
Application info will be uploaded to the account.
Install: CreatingStagingDirectory (5%)
Install: ExtractingPackage (15%)
Install: InspectingPackage (20%)
Install: TakingInstallLock (20%)
Install: PreflightingApplication (30%)
Install: InstallingEmbeddedProfile (30%)
Install: VerifyingApplication (40%)
Install: CreatingContainer (50%)
Install: InstallingApplication (60%)
Install: PostflightingApplication (70%)
Install: SandboxingApplication (80%)
Install: GeneratingApplicationMap (90%)
Process has been completed.
Please launch the Acquisition agent on the iOS device now and keep it running in the foreground.
Press 'Enter' to continue
```

图 2.23 – 将代理安装到设备上

3. 一旦代理成功安装，启动设备上的应用程序。现在，要提取钥匙串，从工具包中选择选项 2。

4. 钥匙串提取过程完成后，你可以继续提取完整文件系统。通常，你会想要选择选项 4，这将提取用户分区：

![](img/0b17a17d4cb56b1c25763a396dd83117_78_0.png)

图 2.24 – 使用代理采集完整文件系统

5. 文件系统镜像将归档为 TAR 文件。过程完成后，选择选项 5 以卸载代理。

至此，我们的文件系统采集完成！

基于 Checkm8 的采集和基于代理的采集都是一个完整的突破，因为它们允许对整个文件系统进行取证可靠的提取，包括与第三方应用相关的所有文件。正如你所学到的，完整文件系统相比逻辑提取将包含更多数据。在接下来的章节中，我们将深入研究这些文件，并学习如何分析它们以获得有意义的证据。

### 总结

在本章中，我们学习了采集方法之间的差异以及在 iOS 提取中可以期望找到的数据类型。我们学习了 Checkm8 漏洞的工作原理，如何利用此漏洞访问设备的文件系统，以及它的局限性。然后，我们讨论了基于代理的采集，并学习了哪些 iOS 版本支持此采集。

越狱是 iOS 世界中一个引人入胜的话题；我们讨论了其中两个最受欢迎的越狱工具，checkra1n 和 unc0ver，以及它们的兼容性。然后，我们学习了如何使用 checkra1n 越狱设备。

最后，我们学习了如何使用 Cellebrite UFED 和 Elcomsoft iOS Forensic Toolkit 执行逻辑采集，并探讨了执行完整文件系统采集的两种不同选项。

请记住，在本章中，我们专注于 **首次解锁后（AFU）** 采集方法，因此取证人员必须完全访问设备。在接下来的章节中，我们将研究 **首次解锁前（BFU）** 采集方法，以了解在锁定设备中可以找到哪些类型的痕迹。

在下一章中，我们将讨论取证工具，哪些是最受欢迎的，以及如何使用这些工具分析提取的数据。

## 第 3 章：使用取证工具

在上一章中，我们学习了有关不同采集技术的所有知识，以及如何使用取证工具从 iOS 设备中提取数据。在本章中，我们将学习用于数据分析的最受欢迎的工具，以及它们的特性和局限性。

尽管使用取证工具进行数据分析并非严格必需，因为理论上可以手动检查文件，但使用这些工具具有许多优势。与手动解析痕迹相比，使用取证工具最明显的好处是时间：如果使用正确，这些工具是巨大的时间节省者，因为它们被编程为自动搜索相关文件、查找模式、分析已知数据结构，并从常见痕迹中提取有意义的见解。使用适当的工具可确保不会遗漏宝贵的痕迹，尽管调查人员应始终验证此类工具的输出并了解其局限性。没有完美的取证工具——每个工具都有其缺陷！大多数工具允许调查人员检查第三方应用程序以及系统应用程序，并且还可以配置为对这些应用程序自动执行自定义数据雕刻。

在本章中，我们将涵盖以下主题：

- 了解取证工具
- 使用 Cellebrite Physical Analyzer
- 使用 Magnet AXIOM
- 使用开源工具

### 了解取证工具

一旦 iOS 设备及其存储的证据被成功扣押、保存并通过取证可靠的方式提取


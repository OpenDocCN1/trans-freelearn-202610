

# msinfo32

从 Windows 机器获取大量信息的第一项技术，围绕着一个自 Windows XP 时代起就内置在 Windows 操作系统中的工具展开：msinfo32.exe。

要调用此工具，我们只需按下 Windows 键 + R 打开运行对话框，输入 msinfo32：

![](img/c8ffa9959721097701584fcc95101362_307_0.png)

图 7.20 – 启动 msinfo32

按回车键。这将调出 msinfo32.exe —— 系统信息应用程序：

![](img/c8ffa9959721097701584fcc95101362_307_1.png)

图 7.21 – Microsoft 系统信息实用工具

如你所见，系统信息工具包含硬件、软件、组件和 Internet 设置等部分。你可以点击浏览所有子部分，甚至可以搜索特定信息。

以下区域对我们尤其重要。

## 系统摘要

系统信息工具打开时的初始视图提供了有关计算机的详细信息：

![](img/c8ffa9959721097701584fcc95101362_308_0.png)

图 7.22 – msinfo32 – 系统摘要

此页面包含操作系统信息、BIOS 详细信息、硬件摘要和环境变量。

## 系统驱动程序

如果我们导航到 **软件环境 | 系统驱动程序**，可以看到系统中存在的所有驱动程序：

| 系统摘要 | 名称 | 描述 | 文件 | 类型 | 已启动 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **硬件资源** | \| | | | | | | | | |
| **组件** | \| | | | | | | | |
| **软件环境** | \| | | | | | | | |
| 系统驱动程序 | \| | | | | | | | |
| 签名驱动程序 | \| | | | | | | | |
| 环境变量 | \| | | | | | | | |
| 打印任务 | \| | | | | | | | |
| 网络连接 | \| | | | | | | | |
| 正在运行的任务 | \| | | | | | | | |
| 已加载模块 | \| | | | | | | | |
| 服务 | \| | | | | | | | |
| 程序组 | \| | | | | | | | |
| 启动程序 | \| | | | | | | | |
| OLE 注册 | \| | | | | | | | |
| Windows 错误报告 | \| | | | | | | | |
| Internet 设置 | \| | | | | | | | |
| | | | | | | | | |
| | ide | 软盘控制器驱动程序 | C:\windows\system32\drivers\iide.sys | 内核驱动程序 | 否 |
| | fastfat | FastFAT | c:\windows\system32\drivers\fastfat.sys | 文件系统驱动程序 | 否 |
| | dmload | dmload | c:\windows\system32\drivers\dmload.sys | 内核驱动程序 | 是 |
| | dmio | 逻辑磁盘管理器驱动程序 | c:\windows\system32\drivers\dmio.sys | 内核驱动程序 | 是 |
| | dmboot | dmboot | c:\windows\system32\drivers\dmboot.sys | 内核驱动程序 | 否 |
| | disk | 磁盘驱动程序 | c:\windows\system32\drivers\disk.sys | 内核驱动程序 | 是 |
| | compbatt | Microsoft 复合电池驱动程序 | c:\windows\system32\drivers\compbatt.sys | 内核驱动程序 | 是 |
| | cmibatt | Microsoft 交流适配器驱动程序 | c:\windows\system32\drivers\cmibatt.sys | 内核驱动程序 | 是 |
| | cdrom | CD-ROM 驱动程序 | c:\windows\system32\drivers\cdrom.sys | 内核驱动程序 | 是 |
| | cd21 | Cd21 | c:\windows\system32\drivers\cd21.sys | 文件系统驱动程序 | 是 |
| | cdudaud | Cduaduo | c:\windows\system32\drivers\cduaduo.sys | 内核驱动程序 | 否 |
| | ccbd2k | ccbd2k | c:\windows\system32\drivers\ccbd2k.sys | 内核驱动程序 | 否 |
| | beep | Beep | c:\windows\system32\drivers\beep.sys | 内核驱动程序 | 是 |
| | audstub | 音频存根驱动程序 | c:\windows\system32\drivers\audstub.sys | 内核驱动程序 | 是 |
| | atmarp | ATM ARP 客户端协议 | c:\windows\system32\drivers\atmarp.sys | 内核驱动程序 | 否 |
| | alapi | 标准 IDE/ESDI 硬盘控制... | c:\windows\system32\drivers\alapi.sys | 内核驱动程序 | 是 |
| | asycmaca | RAS 异步媒体驱动程序 | c:\windows\system32\drivers\asycmaca.sys | 内核驱动程序 | 否 |
| | agm440 | Intel A3P 总线过滤器 | c:\windows\system32\drivers\agm440.sys | 内核驱动程序 | 是 |
| | afd | AFD | c:\windows\system32\drivers\afd.sys | 内核驱动程序 | 是 |
| | acpiec | ACPIEC | c:\windows\system32\drivers\acpiec.sys | 内核驱动程序 | 否 |
| | aepi | Microsoft ACPI 驱动程序 | c:\windows\system32\drivers\aepi.sys | 内核驱动程序 | 是 |
| | vmvmmid1 | 内存控制驱动程序 | /*?AC_Virtual_ACPI_Common File*/vmware\vdrv | 内核驱动程序 | 是 |
| | aibddsk | Aibddsk | 不可用 | 内核驱动程序 | 否 |
| | abp450v5 | abp450v5 | 不可用 | 内核驱动程序 | 否 |
| | aodpl160m | aodpl160m | 不可用 | 内核驱动程序 | 否 |
| | aha154x | Aha154x | 不可用 | 内核驱动程序 | 否 |
| | aic79ud2 | aic79ud2 | 不可用 | 内核驱动程序 | 否 |
| | aic79x | aic79x | 不可用 | 内核驱动程序 | 否 |
| | aide | Aide | 不可用 | 内核驱动程序 | 否 |
| | amimnt | amimnt | 不可用 | 内核驱动程序 | 否 |
| | asc | asc | 不可用 | 内核驱动程序 | 否 |
| | asc3550p | asc3550p | 不可用 | 内核驱动程序 | 否 |
| | asc3550 | asc3550 | 不可用 | 内核驱动程序 | 否 |
| | atdisk | Atdisk | 不可用 | 内核驱动程序 | 否 |
| | cd20mnt | cd20mnt | 不可用 | 内核驱动程序 | 否 |
| | charger | Charger | 不可用 | 内核驱动程序 | 否 |
| | cmdide | cmdide | 不可用 | 内核驱动程序 | 否 |

图 7.23 – msinfo32 – 系统驱动程序

我通常按 **文件**（路径）对视图进行排序，查看是否有安装在任何奇怪位置的驱动程序。例如，内核驱动程序从 `C:\windows\temp` 文件夹启动就非常可疑。恶意软件有时会注册内核驱动程序以在系统上实现持久化，然后将已安装/已启动的驱动程序列表作为常规检查进行比对。

## 服务

已配置为在系统启动或用户登录时自动启动的进程（应用程序）称为服务。我们可以通过导航到软件环境 | 服务来调出当前配置的服务列表：

![](img/c8ffa9959721097701584fcc95101362_310_0.png)

恶意软件通常会创建服务作为感染过程的一部分，以便在系统上建立持久化，使恶意软件随计算机启动而启动。将一段时间内或与类似系统对比的已安装和正在运行的服务快照进行比较，通常能发现恶意软件感染。

## 正在运行的进程

进程（或任务）是映射到内存中的应用程序（程序文件），它们运行代码并执行任务。因此，如果我们查看进程，就可以了解系统正在执行的任务。要获取系统信息工具抓取信息时正在运行的进程列表，我们可以转到软件环境 | 正在运行的任务：

![](img/c8ffa9959721097701584fcc95101362_311_0.png)

图 7.25 – msinfo32 – 正在运行的进程（任务）

同样，通常会将此列表按文件（路径）排序，以识别进程启动的异常位置。

将不同时间段或类似系统之间的运行进程转储进行对比，可以发现恶意软件或其他不需要的软件的感染。这对于 ICS 系统尤其适用，因为 ICS 系统中的计算机往往长时间运行相同的应用程序集。运行进程集的变化会显得格外醒目。

## 启动程序

除了服务之外，Windows 还提供了多种在系统启动或用户登录时自动启动应用程序的方法。系统信息工具会显示通过其中某些方法自动启动的应用程序。我们可以通过导航到 **软件环境 | 启动程序** 来查看这些应用程序：

![](img/c8ffa9959721097701584fcc95101362_311_1.png)

图 7.26 – msinfo32 – 启动程序

此视图显示程序名称、启动命令、程序使用的用户帐户以及用于自动启动程序的启动方法。同样，抓取启动程序的快照并在一段时间内或与类似系统进行对比，可以发现各种恶意行为。

## 导出报告

为了创建此信息的记录，以便日后进行比较，我们可以通过 文件 | 导出 菜单选项导出系统信息报告。请确保在导出前已选中系统摘要位置，因为这可以保证所有信息都被导出：

![](img/c8ffa9959721097701584fcc95101362_312_0.png)

图 7.27 – msinfo32 – 导出报告

这样，我们就有了 HMI-1 计算机系统信息和系统状态的记录。你应该将其安全地存储在网络上，以便妥善保管和轻松检索。


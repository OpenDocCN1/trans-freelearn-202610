

# 你好，Xcode

欢迎阅读 *《iOS 26 编程入门》*。我希望这本书能作为在 App Store 上创建和发布 iOS 26 应用的有用入门指南。

在本章中，你将下载并在 Mac 上安装 **Xcode**。然后，你将探索 Xcode 用户界面。之后，你将创建你的第一个 **iOS 应用** 并在 **模拟器** 中运行它。最后，你将在 **iOS 设备** 上运行你的应用。

到本章结束时，你将知道如何创建 iOS 应用，如何在模拟器中运行它，以及如何在 iOS 设备上运行它。

本章将涵盖以下主题：

- 从 App Store 下载并安装 Xcode
- 探索 Xcode 用户界面
- 在模拟器中运行你的应用
- 在 iOS 设备上运行你的应用

5. 安装完 Xcode 后，启动它。你会看到一个许可协议屏幕。点击 Agree：

![](img/0972ba626734a3986e095c788777818e_30_0.png)

图 1.2：许可协议屏幕

6. 系统会提示你输入 Mac 管理员的用户名和密码。输入后，点击 OK：

![](img/0972ba626734a3986e095c788777818e_30_1.png)

图 1.3：提示输入管理员用户名和密码

7. 你会看到一个显示可用开发平台的屏幕。现在你只需要 macOS 和 iOS。勾选 iOS 26.0，取消勾选其他所有选项，然后点击 Download & Install：

![](img/0972ba626734a3986e095c788777818e_31_0.png)

# Xcode

版本 26.0 (17A324)

选择你想要开始使用的组件：

平台支持

+ macOS 26.0 内置
+ iOS 26.0 10.07 GB
+ watchOS 26.0 3.83 GB
+ tvOS 26.0 4.83 GB
+ visionOS 26.0 6.98 GB

其他组件

+ 预测代码补全模型 2 GB

下载并安装

还将安装所需的系统组件。

图 1.4：开发平台屏幕

8. 如果看到提示重新启动 Xcode 以使用更新后的框架的提示，点击 **Relaunch Xcode.**

9. 如果看到 **What’s New in Xcode** 屏幕，点击 **Continue.**

10. 你会看到 **Welcome to Xcode** 屏幕。点击左侧窗格中的 **Create New Project...**：

![](img/0972ba626734a3986e095c788777818e_32_0.png)

图 1.5：欢迎使用 Xcode 屏幕

11. Xcode 将自动开始下载 **iOS 26.0 Simulator**。请注意，在此过程完成之前，你将无法在模拟器上运行任何应用：

![](img/0972ba626734a3986e095c788777818e_32_1.png)

图 1.6：模拟器下载进度条

12. 你会看到如下的新建项目屏幕。在 **Choose a template for your new project** 部分，选择 **iOS**。然后选择 **App**，并点击 **Next**：

![](img/0972ba626734a3986e095c788777818e_33_0.png)

图 1.7：新建项目屏幕

![](img/0972ba626734a3986e095c788777818e_35_0.png)

*图 1.9：保存对话框*

15. 你会看到如下的 **Provide Author Information** 对话框：

![](img/0972ba626734a3986e095c788777818e_35_1.png)

*图 1.10：源代码控制首选项屏幕*

如果你的 Mac 采用 Apple Silicon，物理设备部分将显示文字 **My Mac (Designed for iPad)**，因为 Apple Silicon Mac 可以运行 iOS 应用。否则，将显示 **No Devices**。如果你插入一台 iOS 设备，它将出现在该部分，你就可以在上面运行开发的应用进行测试。建议在实际设备上运行你的应用，因为模拟器无法准确反映实际 iOS 设备的性能特征，并且不具备实际设备所拥有的硬件功能。

**Build** 部分有两个菜单项，**Any iOS Device (arm64)** 和 **Any iOS Simulator Device (arm64, x86_64)**。这些在你需要在提交到 App Store 之前归档应用时使用。你将在 *第 27 章，Test and Submit Your App to the App Store* 中学习如何执行此操作。

现在让我们看看如何在实际 iOS 设备上构建和运行你的应用。本书中的大多数说明都不需要你拥有 iOS 设备，因此，如果你没有，可以跳过下一节，直接进入 *第 2 章，Simple Values and Types。*

## 在 iOS 设备上运行你的应用

虽然你可以使用模拟器完成本书中的大部分练习，但建议在实际 iOS 设备上构建和测试你的应用，因为模拟器无法模拟某些硬件组件和软件 API。

> 要全面了解模拟器与实际设备之间的所有差异，请参阅此链接：https://help.apple.com/simulator/mac/current/#!/devb0244142d。

除了设备之外，你还需要一个 Apple 账户（用于自动创建免费 Apple 开发者账户）或付费 Apple 开发者账户，以便在你的设备上构建和运行应用。你可以使用从 App Store 下载 Xcode 时使用的同一个 Apple 账户。

5. 要在你的 iOS 设备上启用开发者模式，请进入 设置 | 隐私与安全性，向下滚动到开发者模式项目并点击它：

![](img/0972ba626734a3986e095c788777818e_44_0.png)

图 1.18：显示开发者模式的隐私与安全性屏幕

6. 打开开发者模式开关：

![](img/0972ba626734a3986e095c788777818e_45_0.png)

图 1.19：开发者模式开关

7. 会出现一个警告，提示开发者模式会降低 iOS 设备的安全性。点击重启。

8. 你的 iOS 设备重启并解锁后，通过点击启用并输入 iOS 设备的密码来确认你想要启用开发者模式。

9. 设备和模拟器窗口将显示正在准备 iPhone 的消息。等待几分钟，然后验证设备和模拟器窗口不再显示开发者模式已禁用的文字：

![](img/0972ba626734a3986e095c788777818e_45_1.png)

图 1.20：Xcode 设备和模拟器窗口显示正在准备 iPhone 消息

你的 iOS 设备现在已准备好从 Xcode 安装和运行应用。

10. 在 Xcode 中，从目标菜单中选择你的 iOS 设备。

11. 通过点击运行按钮（或使用 Command + R）运行项目。你将在 Xcode 的签名与功能面板中收到以下错误：Signing for “JRNL” requires a development team：

图 1.21：Xcode 签名与功能面板

这是因为在 iOS 设备上运行应用需要数字证书，你需要在 Xcode 中添加免费或付费的 Apple 开发者账户，以便生成数字证书。

使用 Apple 账户创建免费开发者账户将允许你在 iOS 设备上测试你的应用，但有效期仅为 7 天。此外，你需要付费 Apple 开发者账户才能在 App Store 上分发应用。你将在第 27 章 Test and Submit Your App to the App Store 中了解更多相关信息。

证书可确保在你的设备上运行的唯一应用是你授权的应用。这有助于防止恶意软件。你也可以在此链接了解更多信息：https://help.apple.com/xcode/mac/current/#/dev60b6fbbc7。

12. 点击 Add Account...：

![](img/0972ba626734a3986e095c788777818e_47_0.png)

图 1.22：Xcode 签名与功能窗格中选中的 Add Account... 按钮

13. Xcode 设置窗口出现，并选中 Apple 账户窗格。点击 Add Apple Account...：

![](img/0972ba626734a3986e095c788777818e_47_1.png)

图 1.23：Xcode 设置窗口

14. 在提供的框中输入你的 Apple 账户电子邮件或电话号码，然后点击 Next。之后，在提供的框中输入你的密码，然后再次点击 Next：

![](img/0972ba626734a3986e095c788777818e_48_0.png)

图 1.24：Apple 账户登录对话框

你也可以通过在 Xcode 菜单中选择设置来访问 Xcode 设置。

15. 几分钟后，验证 Apple 账户窗格是否显示你的账户名称和电子邮件地址：

![](img/0972ba626734a3986e095c788777818e_48_1.png)

图 1.25：Xcode 设置中的 Apple 账户窗格

16. 完成后，点击左上角的红色关闭按钮关闭设置窗口。

17. 在 Xcode 的编辑器区域中，点击 Signing & Capabilities。确保已勾选 Automatically manage signing，并且在 Team 弹出菜单中选择了 Personal Team：



The first package that we need to obtain contains the SAP RFC SDK DLLs. These RFC DLLs get used during design time generation of SAP schemas and during the runtime operations when BizTalk is sending and receiving messages with SAP. Microsoft using these DLLs is a good thing and strengthens our case for having Microsoft handle the integration with an SAP system since we are using SAP code when establishing connections.

我们需要获取的第一个软件包包含 SAP RFC SDK DLL。这些 RFC DLL 在设计时生成 SAP 架构期间以及 BizTalk 与 SAP 发送和接收消息时的运行时操作期间使用。Microsoft 使用这些 DLL 是一件好事，并且加强了我们让 Microsoft 处理与 SAP 系统集成的理由，因为我们在建立连接时使用的是 SAP 代码。

The second package is called `R3DLLINST.zip` and contains the Microsoft runtime DLLs. Even though these are Microsoft DLLs, they are still available through the SAP Marketplace.

第二个软件包名为 `R3DLLINST.zip`，包含 Microsoft 运行时 DLL。尽管这些是 Microsoft DLL，但它们仍然可以通过 SAP Marketplace 获取。

Within the BizTalk Adapter Pack installation folder, an install guide called `InstallationGuide.htm` exists in the `C:\Program Files\Microsoft BizTalk Adapter Pack (x64)\Documents` folder for 64-bit systems. Similarly, a document will exist on a 32-bit system in the `C:\Program Files\Microsoft BizTalk Adapter Pack\Documents` folder. A table exists that describes the prerequisite DLLs that are required for specific versions of SAP. Microsoft has included a link within each section that will take us directly to the location within the SAP Marketplace to download the package and get more specific information. The SAP client version that we will be using for this chapter is SAP client 7.0 so the URLs that we will follow are:

在 BizTalk Adapter Pack 安装文件夹中，名为 `InstallationGuide.htm` 的安装指南位于 64 位系统的 `C:\Program Files\Microsoft BizTalk Adapter Pack (x64)\Documents` 文件夹中。同样，在 32 位系统上，文档将存在于 `C:\Program Files\Microsoft BizTalk Adapter Pack\Documents` 文件夹中。存在一个表格，描述了特定 SAP 版本所需的先决条件 DLL。Microsoft 在每个部分中都包含了一个链接，可直接带我们到 SAP Marketplace 中的位置下载软件包并获取更具体的信息。本章将使用的 SAP 客户端版本是 SAP client 7.0，因此我们将遵循的 URL 为：

| Download Purpose | SAP Note ID | Download URL |
|---|---|---|
| RFC SDK | 27517 | http://go.microsoft.com/fwlink/?LinkID=94691 |
| Microsoft Run Time DLLs | 684106 | http://go.microsoft.com/fwlink/?LinkID=94693 |

When we navigate to the SAP note for the installing the SAP RFC SDK, we are prompted to download the assemblies from another URL: `http://service.sap.com/swdc`.

当我们导航到安装 SAP RFC SDK 的 SAP 备注时，会提示我们从另一个 URL 下载程序集：`http://service.sap.com/swdc`。

![](img/23d1402649b6528b085cd6b900a2c9c3_183_0.png)

Once we navigate down the website's hierarchy of Support Packages and Patches - Entry by Application Group - Additional Components - SAP RFC SDK UNICODE - SAP RFC SDK 7.00 UNICODE we end up with the download section that we are interested in. If we are running a 64-bit environment, then we need both 32-bit and 64-bit packages due to the differences between the 32-bit BizTalk design/admin tools and the 64-bit BizTalk runtime.

一旦我们沿着网站的层级结构导航到 Support Packages and Patches - Entry by Application Group - Additional Components - SAP RFC SDK UNICODE - SAP RFC SDK 7.00 UNICODE，我们就会到达感兴趣的下载部分。如果我们运行的是 64 位环境，则需要同时获取 32 位和 64 位软件包，这是由于 32 位 BizTalk 设计/管理工具与 64 位 BizTalk 运行时之间的差异。

![](img/23d1402649b6528b085cd6b900a2c9c3_183_1.png)

These software packages that exist in the SAP Marketplace have been packaged in a SAP specific package tool called SAPCAR. In order to open these packages, we need to download the SAPCAR tool.

这些存在于 SAP Marketplace 中的软件包已使用名为 SAPCAR 的 SAP 特定打包工具进行打包。为了打开这些软件包，我们需要下载 SAPCAR 工具。

We can download SAPCAR from the same location as where we found the RFC SDK.

我们可以从找到 RFC SDK 的同一位置下载 SAPCAR。

![](img/23d1402649b6528b085cd6b900a2c9c3_184_0.png)

Once again we will need to choose the platform that we will be using when extracting the SAR package.

再次提取 SAR 软件包时，我们需要选择将使用的平台。

Once we have SAPCAR downloaded, we need to open a command line prompt and change our directory to the location where we saved the SAPCAR.exe. If we execute a command similar to the one below, we will be able to extract the SAR archive.

一旦下载了 SAPCAR，我们需要打开命令提示符并将目录更改为保存 SAPCAR.exe 的位置。如果执行类似于下面的命令，我们将能够提取 SAR 归档。

```
F:\Dev\SAP>SAPCAR_0-10003696.exe -xfv "RFC_25-20001765.SAR"

SAPCAR: processing archive RFC_25-20001765.SAR (version 2.01)

SAPCAR: 61 file(s) extracted
```

The result of this command being executed is that a folder called rfcsdk is created that will contain several sub folders including a bin and lib folders.

执行此命令的结果是创建一个名为 rfcsdk 的文件夹，其中将包含多个子文件夹，包括 bin 和 lib 文件夹。

- If we are running a 64-bit BizTalk environment, then we need to copy the SAP DLLs from the 32-bit SAR package and 64 bit SAR package to multiple locations.

- 如果我们运行的是 64 位 BizTalk 环境，则需要从 32 位 SAR 软件包和 64 位 SAR 软件包复制 SAP DLL 到多个位置。

- If you are installing both 32 bit and 64 bit Adapters and DLLs, you need to pay close attention to Microsoft's install instructions. The installation instructions that are provided as part of the adapter install specify the following:
  - "On a 64-bit computer, the 32-bit version of the DLLs must be added to the C:\Windows\SysWow64 folder. The 64-bit version of the DLLs must be added to the C:\Windows\System32 folder." These instructions defy common logic so be sure not to overlook this step. You can also read about this step in the SAP Adapter's Frequently Asked Questions page: http://msdn.microsoft.com/en-us/library/cc185322(BTS.10).aspx

- 如果您同时安装 32 位和 64 位适配器和 DLL，则需要仔细注意 Microsoft 的安装说明。随适配器安装提供的安装说明指定如下：
  - "On a 64-bit computer, the 32-bit version of the DLLs must be added to the C:\Windows\SysWow64 folder. The 64-bit version of the DLLs must be added to the C:\Windows\System32 folder." 这些说明违背了常识，因此请务必不要忽略此步骤。您也可以在 SAP Adapter 的常见问题解答页面中阅读有关此步骤的信息：http://msdn.microsoft.com/en-us/library/cc185322(BTS.10).aspx

In the demo environment for this book, the following 64-bit DLLs and libraries have been copied into the %WINDOWS%\System32 directory from the bin directory that was created in the SAPCAR process.

在本书的演示环境中，以下 64 位 DLL 和库已从 SAPCAR 过程中创建的 bin 目录复制到 %WINDOWS%\System32 目录中。

![](img/23d1402649b6528b085cd6b900a2c9c3_185_0.png)

We now need to copy the following libraries from the 64-bit lib directory into the %WINDOWS%\System32 directory.

我们现在需要从 64 位 lib 目录复制以下库到 %WINDOWS%\System32 目录。

![](img/23d1402649b6528b085cd6b900a2c9c3_185_1.png)

The end result is that the following nine files should all end up in the %WINDOWS%\System32 directory as illustrated in the following screenshot:

最终结果是以下九个文件都应位于 %WINDOWS%\System32 目录中，如以下屏幕截图所示：

![](img/23d1402649b6528b085cd6b900a2c9c3_186_0.png)

Similarly, the following 32-bit DLLs need to be copied into the %WINDOWS%\SysWOW64 directory. The 32-bit DLLs should come from the 32 bit SAR archive that we previously downloaded.

同样，以下 32 位 DLL 需要复制到 %WINDOWS%\SysWOW64 目录。32 位 DLL 应来自我们之前下载的 32 位 SAR 归档。

![](img/23d1402649b6528b085cd6b900a2c9c3_186_1.png)

The next set of prerequisite DLLs that we need to deal with are the Microsoft runtime DLLs. Once again, these DLLs can be retrieved from the SAP Marketplace via the URL that is specified in the BizTalk Adapter Pack Installation Guide. Navigating to http://go.microsoft.com/fwlink/?LinkID=94693 will render the following page. After reviewing the content that SAP has provided, we want to click on the **Attachments** link.

我们需要处理的下一组先决条件 DLL 是 Microsoft 运行时 DLL。再次，这些 DLL 可以通过 BizTalk Adapter Pack 安装指南中指定的 URL 从 SAP Marketplace 获取。导航到 http://go.microsoft.com/fwlink/?LinkID=94693 将显示以下页面。在查看 SAP 提供的内容后，我们希望单击 **Attachments** 链接。

![](img/23d1402649b6528b085cd6b900a2c9c3_187_0.png)

The file that we are interested in is r3dllinst.zip. Click on the link to download this package.

我们感兴趣的文件是 r3dllinst.zip。单击链接下载此软件包。

![](img/23d1402649b6528b085cd6b900a2c9c3_187_1.png)

Once we have the r3dllinst.zip downloaded, we need to extract this package and will end up with the following files in the nt patch folder:

一旦下载了 r3dllinst.zip，我们需要提取此软件包，并在 nt patch 文件夹中得到以下文件：

![](img/23d1402649b6528b085cd6b900a2c9c3_187_2.png)

We now want to execute R3DLLINS.EXE and should get prompted with the following message. Click OK to proceed with the installation of these DLLs.

我们现在想要执行 R3DLLINS.EXE，并应收到以下提示消息。单击 OK 继续安装这些 DLL。

![](img/23d1402649b6528b085cd6b900a2c9c3_188_0.png)

This concludes the prerequisite DLLs that are required in order for the SAP adapter to function properly. We will validate the SAP Adapter functions properly in an upcoming section when we generate IDOC schemas. If we are able to generate Schemas via Visual Studio, then we know our 32-bit installation is complete. We will not know about our 64-bit assemblies until we try to process a message.

这完成了 SAP 适配器正常运行所需的先决条件 DLL。我们将在生成 IDOC 架构时在即将到来的部分验证 SAP 适配器是否正常运行。如果我们能够通过 Visual Studio 生成架构，则我们知道 32 位安装已完成。我们在尝试处理消息之前不会知道 64 位程序集的情况。

## WCF-SAP Adapter vs WCF Customer Adapter with SAP binding

## WCF-SAP 适配器与带 SAP 绑定的 WCF 自定义适配器

When we generate schemas for SAP artifacts, the binding file that is also generated will use the WCF-Custom Adapter with an SAP Binding Type. With this binding type selected, we can manipulate any SAP-related property.

当我们为 SAP 工件生成架构时，同时生成的绑定文件将使用带 SAP 绑定类型的 WCF-Custom 适配器。选择此绑定类型后，我们可以操作任何与 SAP 相关的属性。

![](img/23d1402649b6528b085cd6b900a2c9c3_189_0.png)

The challenge with using the WCF-Custom Adapter is that when we configure the URI for our SAP server, the interface is not very intuitive. The SAP connection string is confusing at best and one missing key stroke will result in an error.

使用 WCF-Custom 适配器的挑战在于，当我们配置 SAP 服务器的 URI 时，界面并不十分直观。SAP 连接字符串最多令人困惑，缺少一个键入就会导致错误。

![](img/23d1402649b6528b085cd6b900a2c9c3_189_1.png)

## WCF SAP Adapter – Sending and Receiving IDOCs

## WCF SAP 适配器 – 发送和接收 IDOC

Conversely, the WCF-SAP adapter is a little more intuitive when we configure our SAP URI. By clicking the Configure button, we have a well-organized table to populate, which reduces the complexity of providing a valid SAP connection string.

相反，在配置 SAP URI 时，WCF-SAP 适配器更直观一些。通过单击 Configure 按钮，我们有一个结构良好的表格需要填写，这降低了提供有效 SAP 连接字符串的复杂性。

![](img/23d1402649b6528b085cd6b900a2c9c3_190_0.png)

To create a WCF-SAP adapter, we need to perform the following steps:

创建 WCF-SAP 适配器，我们需要执行以下步骤：

- 1. Right mouse click on Adapters and then select New - Adapter.

- 1. 右键单击 Adapters，然后选择 New - Adapter。

![](img/23d1402649b6528b085cd6b900a2c9c3_190_1.png)

2. Click the Adapter drop-down list and select WCF-SAP. In the Name text box, type in WCF-SAP and click OK button.

2. 单击 Adapter 下拉列表并选择 WCF-SAP。在 Name 文本框中输入 WCF-SAP，然后单击 OK 按钮。

![](img/23d1402649b6528b085cd6b900a2c9c3_191_0.png)

It is important to understand that no matter whether you use the WCF-Custom Adapter or WCF-SAP Adapter, you are not losing functionality. Using the WCF-SAP adapter simplifies the administration tasks that are involved in maintaining our BizTalk environment. Since the WCF-Custom adapter can leverage many different bindings including Oracle, SQL, SAP, and bindings that allow us to connect to the cloud, it is beneficial to explicitly create these adapters to avoid confusion.

重要的是要理解，无论您使用 WCF-Custom 适配器还是 WCF-SAP 适配器，都不会丢失功能。使用 WCF-SAP 适配器简化了维护 BizTalk 环境所涉及的管理任务。由于 WCF-Custom 适配器可以利用多种不同的绑定，包括 Oracle、SQL、SAP 以及允许我们连接到云的绑定，因此明确创建这些适配器以避免混淆是有益的。

## Supported SAP configurations

## 支持的 SAP 配置

The following SAP configurations are supported. New to BizTalk 2010 is the support for SAP 7. For the purpose of this chapter, the SAP system that is being used is SAP ECC 6.0 but the following versions are supported:

以下 SAP 配置受支持。BizTalk 2010 的新功能是支持 SAP 7。为了本章的目的，所使用的 SAP 系统是 SAP ECC 6.0，但以下版本受支持：

- Supported server versions: SAP 7, SAP ECC 6.0 Unicode, SAP ECC 5.0 Non-Unicode, SAP ECC 5.0 Unicode, SAP R/3 4.7 Non-Unicode, SAP R/3 4.7 Unicode, SAP R/3 4.6c Non-Unicode, SAP ERP 6.0 with EHP 4.0
- Supported client versions: SAP RFC SDK 7.11 UNICODE, SAP RFC SDK 7.1 UNICODE, SAP RFC SDK 7.0 UNICODE, SAP RFC SDK 6.4 UNICODE

- 支持的服务器版本：SAP 7、SAP ECC 6.0 Unicode、SAP ECC 5.0 Non-Unicode、SAP ECC 5.0 Unicode、SAP R/3 4.7 Non-Unicode、SAP R/3 4.7 Unicode、SAP R/3 4.6c Non-Unicode、SAP ERP 6.0 with EHP 4.0
- 支持的客户端版本：SAP RFC SDK 7.11 UNICODE、SAP RFC SDK 7.1 UNICODE、SAP RFC SDK 7.0 UNICODE、SAP RFC SDK 6.4 UNICODE


-   18. 你在末尾添加 $ 结束锚点，以确保它仅评估文件类型，而不是将文件扩展名作为单词的一部分进行评估。
-   19. 你将此正则表达式存储在 $searchRegex 变量中。
-   20. 你继续利用 measure-command cmdlet 来测量 foreach 循环和正则表达式的评估。
-   21. 你首先使用 foreach 来评估 $files 中的每个 $file。
-   22. 然后你使用 -match 比较运算符声明一个 if 语句，以确定 $file 是否匹配 $searchRegex 中设置的正则表达式。
-   23. 如果正则表达式返回 True，则 $filefound 变量将增加 1。
-   24. 在 foreach 循环完成对每个文件的评估后，你随后将执行持续时间捕获到 $time2 变量中。
-   25. 你还使用 write-host cmdlet 并传入 "Total Number of Found Files $filefound" 参数。
-   26. 这会显示 Total Number of Found Files 1288960。
-   27. 最后，你通过调用变量来显示 $time1 和 $time2 中的持续时间。
-   28. 然后你通过调用 $time1 - $time2 并引用 TotalSeconds 属性来计算执行时间的差值。
-   29. 你将此值设置在 $timediff 变量中。
-   30. 最后，你使用 write-host cmdlet 并传入 "Total Difference in Speed: $timediff Total Seconds" 来将时间值打印到显示器上。
-   31. 你会看到，在此示例中，利用正则表达式比使用 foreach 循环处理数据快 13.285 秒。

## Summary

在本章中，你学习了正则表达式。你探索了不同的常见正则表达式，并了解到你可以使用它们与 -match 比较运算符一起使用。你还了解到有一些 cmdlet 支持使用正则表达式。然后你使用数组中的值创建了一个动态正则表达式，并识别了匹配这些表达式的字符串。最后，你评估了使用正则表达式与使用多个数组进行字符串搜索之间的性能差异。在下一章中，你将把本书中学习到的所有概念结合起来，创建一个 Windows 服务器扫描脚本。

## 12 Overall Script Workflow, Termination Files, and Merging Data Results

当你准备创建 Windows 服务器扫描脚本时，在执行脚本本身之前，你需要创建多个组件。例如，如果你打算使用加密字符串，你需要开发一个脚本来创建随机密码、salt 和 init。你还需要另一个脚本来执行字符串的加密。由于你可能需要创建多个脚本，本章概述了推荐的脚本工作流程，以确保你在环境中运行扫描脚本所需的所有组件都已齐全。

在探索工作流程后，你将继续开发一个脚本来创建随机字符串。这些随机字符串是加密脚本的直接输入，并存储在 `answer` 文件中。你还将学习如何开发一个加密脚本，在循环中加密多个字符串。

完成字符串加密后，你将继续学习 `answer` 文件。你将了解可以在 answer 文件中包含的不同组件，以在脚本中启用多种功能。

你将探索利用终止文件在本地和远程停止扫描脚本执行的方法。你还将学习如何删除 `termination` 文件以重新启用 Windows 服务器扫描脚本的运行。

本章中你将学习的最后一个组件是如何合并服务器扫描脚本的结果。如果你在多个系统上运行了脚本，你需要一种方法将 CSV 文件合并在一起。这将使你能够在一个位置查看所有收集的数据，并发现环境中系统之间的相似之处。

In this chapter, you will:

-   Obtain an overview of the Windows server scanning script
-   Create a script to generate randomized strings for use in the encryption script
-   Develop an encryption script that will enable encrypting of multiple strings
-   Learn the `answer` file's structure
-   Understand the purpose of creating and deleting `termination` files locally and remotely on a system
-   Merge multiple CSVs into a single CSV file

## Windows server scanning script overview

Windows 服务器扫描脚本是一个企业级工具，你可以用它来检查环境中的系统。此脚本可作为起点，用于创建自己的脚本。在实施 Windows 服务器扫描脚本时，你需要遵循一个通用工作流程，以确保脚本安全可靠。

The workflow steps include:

-   1. **Security key generation:** 你将创建一个字符串生成脚本，可随机创建你的加密密码、salt 和 init。你还将利用编码来增强密码、salt 和 init。此脚本的输出将由 `encryption` 函数、`decryption` 函数和 `answer` 文件使用。
-   2. **Encryption script:** 然后你将利用安全密钥生成中生成的值来创建加密脚本。这将使你能够加密字符串以在你的 `answer` 文件中使用。
-   3. **Answer file:** 你将学习需要在 `answer` 文件中存储的不同答案。你还将了解到你需要从本书本章的代码文件中下载 `Scan_Answers.xml` answer 文件。
-   4. **Termination script**: 为了确保你能够远程终止 Windows 服务器扫描脚本，你将创建一个终止脚本。此脚本将在系统上创建一个终止文件，如果存在该文件，则会使 Windows 服务器扫描脚本停止。
-   5. **Windows server scanning script**: 在下一章中，你将把本书中所有示例的概念结合起来，创建一个完整的扫描脚本。本节中的所有其他工作流程步骤都会创建脚本和数据，以补充 Windows 服务器扫描脚本。
-   6. **Merging files**: 工作流程中的最后一个脚本是合并脚本。如果在多个系统上运行 Windows 服务器扫描脚本，你可以将结果合并到一个易于阅读的文件中。

## Pre-script security

当你想要创建一个企业级脚本时，你需要从安全组件开始。在 Chapter 4, String Encryption and Decryption 中，你学习了利用编码和加密为脚本提供额外安全性的重要性。你可以遵循几种企业策略，以确保正确实施编码和加密。其中一种流行策略是创建所有脚本共享的加密和解密 salt、init 和密码。从脚本管理角度来看，这是一种更简单的方法，但你会让所有部门访问相同的加密数据。或者，你可以让各部门为脚本创建自己的编码 salt、init 和密码。这样，加密数据与你的部门生成的数据不同。这为在 answer 文件中实现的加密数据提供了额外的模糊层。虽然这种方法更安全，但协调共享脚本会更困难一些。你需要确保每个团队在脚本和 answer 文件中实现自己的编码 salt、init 和密码。

## Script 1 - randomized password and string generator

无论你为环境选择什么策略，你都需要通过创建随机密码来开始脚本编写过程。然后你对该密码进行编码，拆分为脚本侧解密器（SSD），或 $SSD，答案文件解密器（AFD），或 $AFD，以及运行时解密器（RTD），或 $RTD。最后，你指定一个 salt 字符串，并将其编码到 $salt 变量中，并指定一个 init 字符串，并将其编码到 $init 变量中。你现在拥有了所有先决条件，可以通过利用加密函数开始加密数据。

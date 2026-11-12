

- 8. 活动的 **保留格式** 属性允许用户在提取 PDF 文件中的文本时保留文本格式。例如，如果文档包含两栏文本，如 *图 11.3* 所示，该选项可让您按原样提取文本。将活动属性设置为 `True`：

It was November. Although it was not yet late, the sky was dark when I turned into Laundress Passage. I closed the door and put the shop key in its usual place behind Bailey's Advanced Principles of Geometry. Poor Bailey. No one has wanted his fat gray book for thirty years. Sometimes I wonder what he makes of his role as guardian of the bookshop keys. I don't suppose it's the destiny he had in mind for the masterwork that he spent two decades writing. heavily embellished capitals and curly flourishes, my first impression was that it had been written by a child. The letters seemed untrained. Their uneven strokes either faded into nothing or were heavily etched into the paper. There was no sense of flow in the letters that spelled out my name. Each had been undertaken separately -- M A R G A R E T L E A -- as a new and daunting enterprise. But I knew no children. That is when I thought, It is the hand of an invalid. It gave me a queer feeling. Yesterday or the day before, while I had been going about my business, quietly and in private, a letter. For me. That was something of an event. The crisp-cornered envelope, puffed up with its thickly folded contents, was addressed in a hand that must have given the postman a certain amount of trouble. Although the style of the writing was old-fashioned, with its

Figure 11.3 – 用于保留格式的双栏文本示例 PDF 文件

- 9. 将 **写入文本文件** 活动拖放到位。
- 10. 通过将先前创建的字符串变量提供给 **文本** 属性来配置 **写入文本文件** 活动。
- 11. 通过在 **写入到文件名** 属性中提供新文本文件的输出文件路径来配置 **写入文本文件** 活动。
- 12. 配置完成后，您的工作流应类似于以下截图：

![](img/84b06ad2832c53c5f677f8ce4c80f184_349_0.png)

Figure 11.4 – 使用读取 PDF 文本读取 PDF 文件

- 13. 运行工作流并检查文本文件中提取数据的输出。我们在此练习中使用的示例 PDF 文件及工作流结果如下截图所示：

![](img/84b06ad2832c53c5f677f8ce4c80f184_349_1.png)

Figure 11.5 – 读取 PDF 文本的输出

仔细查看输出和 PDF 文件中的文本。您会发现，**读取 PDF 文本** 活动未提取居中对齐语句下方两栏中的文本。原因是居中对齐文本下方的文本是图像。因此，我们需要使用 **使用 OCR 读取 PDF** 活动来提取图像中的文本。要更改工作流以捕获缺失的文本，请执行以下步骤：

- 1. 删除 **读取 PDF 文本** 活动。
- 2. 搜索 **使用 OCR 读取 PDF** 活动，并将其放置在 **读取 PDF 文本** 活动之前所在的顶部位置。
- 3. 检查活动属性时，您应会看到与 **读取 PDF 文本** 活动中看到的类似属性。像之前为 **读取 PDF 文本** 活动所做的那样，使用文件路径更改 **FileName** 属性。
- 4. 默认情况下，**范围** 属性已设置。目前，我们不会更改它，只需使用相同的值。
- 5. 提供您先前创建的字符串变量来更改输出 **文本** 属性。配置应类似于 *图 11.6*：

![](img/84b06ad2832c53c5f677f8ce4c80f184_350_0.png)

Figure 11.6 – 配置使用 OCR 读取 PDF 活动

- 6. 您还可以看到，有一些特定于 OCR 的其他属性。您需要将 OCR 引擎拖放到使用 OCR 读取 PDF 活动中，此活动才能正常工作。在活动面板中搜索 OCR 以找到所有可用的 OCR 引擎。您在活动面板中的搜索应类似于图 11.7：

![](img/84b06ad2832c53c5f677f8ce4c80f184_351_0.png)

Figure 11.7 – 在活动面板中搜索可用的 OCR 引擎

- 7. 我们可用多种 OCR 引擎。使用 Microsoft OCR 活动。将 Microsoft OCR 活动拖放到使用 OCR 读取 PDF 活动中以完成配置。

- 8. 删除输出文本文件并运行工作流，以根据 OCR 配置生成新输出。

- 9. 打开输出文本文件并比较生成的结果。您会发现，数据提取未保留格式。所有文本都以原始形式显示，如下截图所示。但是，它能够提取页面图像部分中的所有文本：

![](img/84b06ad2832c53c5f677f8ce4c80f184_352_0.png)

Figure 11.8 – 使用使用 OCR 读取 PDF 活动的示例数据提取

从本示例中吸取的教训是，使用 OCR 无法为我们提供格式。此外，使用 OCR 引擎提取的文本的准确性可能取决于 PDF 文件中图像的质量。您可以尝试将 ImageDPI 属性更改为不同的值，以确定哪种配置能为您提供最高级别的准确性。

如您现在所了解的，两个基本活动使我们能够从 PDF 文件中提取文本。您可以在 XPF 文件中自行练习 XPF 特定的活动。此外，探索其余活动，了解如何获取页数和提取页面范围，以熟悉这些活动。既然您已对 PDF 交互有基本了解，我们将进入下一节，该节说明如何使用屏幕抓取向导从 PDF 文件中提取文本。

## 使用屏幕抓取提取 PDF 文本

UiPath 中的屏幕抓取提供了从特定应用程序或 UI 元素提取文本的功能。与我们在 *第 10 章，探索用户界面和用户/机器人交互* 中使用的同一屏幕抓取向导一样，可以从 PDF 文件中提取数据。既然您已对屏幕抓取有总体了解，让我们尝试构建一个新工作流，使用屏幕抓取从同一个 PDF 文件中提取文本：

- 1. 打开需要进行屏幕抓取的 PDF 文档。
- 2. 打开本章前一部分中您正在处理的同一 UiPath 解决方案。
- 3. 创建一个新的 **序列** 工作流文件。
- 4. 单击顶部功能区中的 **设计** 选项卡。
- 5. 找到 **屏幕抓取** 选项并单击它以打开 **屏幕抓取向导**。单击后将自动激活 UI 元素检测，您移动鼠标指针时即可高亮显示元素。

- 6. 用鼠标高亮显示整个页面区域并左键单击。向导将检测该区域，提取给定区域的数据，并在屏幕抓取向导屏幕中向您显示：

It was November. Although it was not yet late, the sky was dark when I turned into Laundress Passage. I closed the door and put the shop key in its usual place behind Bailey’s Advanced Principles of Geometry. Poor Bailey. No one has wanted his fat gray book for thirty years. Sometimes I wonder what he makes of his role as guardian of the bookshop keys. I don’t suppose it’s the destiny he had in mind for the masterwork that he spent two decades writing.

A letter. For me. That was something of an event. The crisp-cornered envelope, puffed up with its thickly folded contents, was addressed in a hand that must have given the postman a certain amount of trouble. Although the style of the writing was old-fashioned, with its

![](img/84b06ad2832c53c5f677f8ce4c80f184_354_0.png)

Figure 11.9 – 使用屏幕抓取向导的示例数据提取

如图 11.9 所示，您可以看到使用了 FullText 方法来抓取数据。使用 FullText 方法也在一定程度上保留了格式。尝试更改抓取方法并刷新结果，以查看您收到的不同输出。


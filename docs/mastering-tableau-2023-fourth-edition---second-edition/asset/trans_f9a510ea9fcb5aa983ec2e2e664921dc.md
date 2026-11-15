

`Fastener` 也是来自辅助数据源的一个维度。在*图 4.35* 中，它在一个单元格中显示 `\*`，从而表明 `Fastener` 没有按维度应有的方式工作；也就是说，它没有对数据进行切片，正如*第 1 章*、*回顾基础知识*中所讨论的。显示星号的原因是有多种 `Fastener` 类型与 `Wool` 相关联。`Button` 和 `Velcro` 之所以显示，是因为 `Acrylic` 和 `Polyester` 在底层数据中各自只有一种 `Fastener` 类型。

如果您使用混合，请确保您的主要原因是为了组合度量，并且您不需要在详细级别上使用维度。在创建仪表板之前了解这一点尤其有用，以便做好相应的准备。您的数据需要额外的准备（请查看*第 3 章*、*使用 Tableau Prep Builder*），因为无论是联接还是混合都无法为您带来预期的数据结构。或者您可以利用搭建（scaffolding）技术，这是一种使用辅助数据源的技术——我们将在下一节讨论这一点。

## 介绍搭建技术

搭建是一种通过混合引入第二个数据源，以重塑和/或扩展初始数据源的技术。搭建启用了一些功能，可扩展 Tableau 以满足原本极难甚至完全无法实现的可视化和分析需求。搭建技术在 Tableau 中的先驱 Joe Mako 讲述了一个故事，他在其中使用该技术仅用四个工作表就重建了一个仪表板。原来的仪表板未使用搭建技术，需要 80 个工作表，并需费力地逐像素对齐。

搭建技术所实现的众多可能性中，包括扩展 Tableau 的预测功能。Tableau 原生的预测功能有时因缺乏复杂性而受到批评。搭建技术可用于解决这一批评。

以下是步骤：

1. 在与本章关联的工作簿中，选择 `Scaffolding` 工作表并连接到 `World Indicators` 数据源。
2. 使用 Excel 或文本编辑器，创建一个 `Records` 数据集。下面的两行表格代表了 `Records` 数据集的全部内容：
   ![](Images/B18435_04_31.png)
   *图 4.31：Excel 文件*
3. 将 Tableau 连接到该数据集。
4. 为方便起见，可考虑使用 `Ctrl` + `C` 直接从 Excel 复制数据集，并使用 `Ctrl` + `V` 直接粘贴到 Tableau 中。
5. 在 Tableau 中创建一个 `Start Date` 参数，设置如下面屏幕截图所示。请注意屏幕截图中高亮显示的部分，您可以通过它们设置所需的显示格式：
   ![](Images/B18435_04_32.png)
   *图 4.32：显示格式*
6. 创建另一个名为 `End Date` 的参数，设置相同。
7. 在 `数据` 窗格中，右键单击刚创建的 `Start Date` 和 `End Date` 参数，然后选择 `显示参数`：
   ![](Images/B18435_04_33.png)
   *图 4.33：显示参数*
8. 根据需要设置开始和结束日期，例如 `2000` – `2027`：
   ![](Images/B18435_04_34.png)
   *图 4.34：开始和结束日期*
9. 选择 `Records` 数据源，并创建一个名为 `Date` 的计算字段，代码如下：
   ```
   IIF([Records]=1,[Start Date],[End Date])
   ```
10. 将 `Date` 字段放置在 `行` 货架上。
11. 右键单击 `行` 货架上的 `Date` 字段，然后选择 `显示缺失值`。请注意，现在显示了开始和结束日期设置之间的所有日期：
    ![](Images/B18435_04_35.png)
    *图 4.35：显示缺失值*
12. 创建一个名为 `Select Country A` 的参数，设置如下面屏幕截图所示。请注意，国家列表是通过 `从字段添加值` 按钮添加的：
    ![](Images/B18435_04_36.png)
    *图 4.36：从字段添加*
13. 创建另一个名为 `Select Country B` 的参数，设置相同。
14. 创建一个名为 `Select Country A Forecast` 的参数，设置如下面屏幕截图所示：
    ![](Images/B18435_04_37.png)
    *图 4.37：编辑参数*
15. 创建另一个名为 `Select Country B Forecast` 的参数，设置相同。
16. 在 `数据` 窗格中，右键单击刚创建的四个参数（`Select Country A`、`Select Country B`、`Select Country A Forecast` 和 `Select Country B Forecast`），然后选择 `显示参数`：
    ![](Images/B18435_04_38.png)
    *图 4.38：显示参数*
17. 确保 `World Indicators` 数据源中的 `Date` 字段显示了橙色的链接图标。这表明它被用作链接字段：
    ![](Images/B18435_04_39.png)
    *图 4.39：链接*
18. 在 `World Indicators` 数据集中，创建以下计算字段：
    | **计算字段名称** | **计算字段代码** |
    | --- | --- |
    | `Country A Population` | 
    ```
    IIF([Country] = [Select Country A],[Population Total],NULL)
    ```
    |
    | `Country B Population` | 
    ```
    IIF([Country] = [Select Country B],[Population Total],NULL)
    ```
    |
19. 在 `Records` 数据集中，创建以下计算字段：
    | **计算字段名称** | **计算字段代码** |
    | --- | --- |
    | `Actual/Forecast` | 
    ```
    IIF(ISNULL(AVG([World Indicators].[Population Total])),"Forecast","Actual")
    ```
    |
    | `Country A Population` | 
    ```
    IF [Actual/Forecast] = "Actual" THEN SUM([World Indicators].[Country A Population]) ELSE PREVIOUS_VALUE(0)
    *[Select Country A Forecast] + PREVIOUS_VALUE(0)
    END
    ```
    |
    | `Country A YOY Change` | 
    ```
    ([Country A Population] - LOOKUP([Country A Population], -1)) /
    ABS(LOOKUP([Country A Population], -1))
    ```
    |
    | `Country B Population` | 
    ```
    IF [Actual/Forecast] = "Actual" THEN SUM([World Indicators].[Country B Population]) ELSE PREVIOUS_VALUE(0)
    *[Select Country B Forecast] + PREVIOUS_VALUE(0)
    END
    ```
    |
    | `Country B YOY Change` | 
    ```
    ([Country B Population] - LOOKUP([Country B Population], -1)) /
    ABS(LOOKUP([Country B Population], -1))
    ```
    |
    | `Country A-B Diff` | 
    ```
    [Country A Population] - [Country B Population]
    ```
    |
    | `Country A-B % Diff` | 
    ```
    [Country A-B Diff]/[Country A Population]
    ```
    |
20. 在 `数据` 窗格中，右键单击 `Country A YOY Change`、`Country B YOY Change` 和 `Country A-B % Diff`，然后选择 `默认属性` | `数字格式…` 以将默认数字格式更改为 `百分比`：
    ![](Images/B18435_04_40.png)
    *图 4.40：默认属性*
21. 选中 `Records` 数据源，将 `Actual/Forecast`、`度量值` 和 `度量名称` 字段分别放置在 `颜色`、`文本` 和 `列` 货架上：
    ![](Images/B18435_04_41.png)
    *图 4.41：预测设置*
22. 调整 `度量值` 货架，使显示的字段与下面的屏幕截图完全一致。此外，确保这些字段中的每一个的 `计算使用` 都设置为 `表格(向下)`：
    ![](Images/B18435_04_42.png)
    *图 4.42：计算使用*

那么，我们迄今为止实现了什么？我们创建了一个重复的数据结构，使我们能够在两个单独的列中比较两个国家，即使原始数据集中的数据在一列中。此设置允许我们提出更高级的问题。


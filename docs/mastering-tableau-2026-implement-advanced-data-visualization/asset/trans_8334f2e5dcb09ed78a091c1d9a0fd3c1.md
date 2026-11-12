

在创建 **诊断文本** 字段时使用了 REPLACE 函数：

```
REPLACE([Diagnosis],[DX] + "","")
```

此计算字段使用 DX 中隔离的 ICD-9 代码，从 **诊断** 字段中删除相同的代码，从而提供一个相当干净的描述。请注意术语 *相当干净*。已删除的行最初与较长的描述相关联，因此包含回车符。由此产生的额外行正是我们在此练习中删除的行。因此，在此解决方案中，使用 REPLACE 计算对较长的描述进行截断。

此练习的最终输出可以是将 Tableau 中的数据导出为额外的数据源。随后，这些数据可用于 Tableau 和其他工具以满足未来的报告需求。例如，**DX** 字段在数据混合中可能很有用。

Tableau 是否提供了更好的方法来解决前一种方案中与截断数据相关的问题？有！让我们将注意力转向下一个练习，我们将在其中考虑正则表达式函数。

## 提取数据

虽然如上一个练习所示 *清理数据*，SPLIT 函数可用于清理数据，但正则表达式函数功能更强大，代表了 Tableau 传统专注于可视化和分析的范围扩展到包含数据清理功能。让我们看一个示例，该示例要求我们在 Tableau 中处理一些相当混乱的数据。我们的目标是提取电话号码。

以下是步骤：

- 1. 如果您尚未下载，请从 https://github.com/PacktPublishing/Mastering-Tableau-2026/tree/main/Chapter92 下载 **第 2 章** 工作簿并在 Tableau 中打开它。
- 2. 选择 **提取数据** 选项卡。
- 3. 在 **数据** 窗格中，选择 **字符串数据** 数据源，并将 **字符串数据** 字段拖动到 **行** 卡上。观察与提取电话号码相关的挑战：

![](img/66cf6b89349fda3581a4f196cb3c98a4_92_0.png)

- 4. 单击 **查看数据** 按钮并复制几行来访问底层数据：

![](img/66cf6b89349fda3581a4f196cb3c98a4_93_0.png)

图 2.15：访问底层数据

- 5. 导航到 http://regexpal.com/ 并将数据粘贴到标记为 **测试字符串** 的窗格中；即第二个窗格：

![](img/66cf6b89349fda3581a4f196cb3c98a4_93_1.png)

图 2.16：RegEx Pal

- 6. 在第一个窗格（标记为 **正则表达式** 的窗格）中，键入以下内容：

```
([0-9]{3}) - [0-9]{3} - [0-9]{4}
```

- 7. 返回 Tableau 并使用以下代码块创建名为 **电话号码** 的计算字段。请注意计算字段中嵌套的正则表达式：

```
REGEXP_EXTRACT([String of Data (String of Data)], '(\\(([0-9]{3})\\)-[0-9]{3}-[0-9]{4}) ')
```

- 8. 将 **电话号码** 放置在 **行** 卡上，并观察结果：

| 字符串数据 (字符串数据) | 电话号码 | |
|---|---|---|
| Alexis Sweat 246-6985 Nullam Ave (832)-706-8508 nunc@etegitpsumdonec.ca | (832)-706-8508 | Abc |
| Anastasia Lowe 1643 Pretium Rd. (655)-655-8703 nunc.commodo.auctor@eratasednunc.net | (655)-655-8703 | Abc |
| Ariana Townsend 6814 Neque Av. (291)-873-2613 eu.arcu@disparturient.co.uk | (291)-873-2613 | Abc |
| Arsenio Horn (Ap #492) 5765 Faucibus Road (493)-822-1732 amet@quam.net | (493)-822-1732 | Abc |
| Athena Franks P.O. Box 595, 6191 Lectus, Avenue (330)-875-5639 adipiscing@arcuacorci.net | (330)-875-5639 | Abc |
| Aubrey Castillo 4207 Lorem Street (317)-843-9415 maecenas@egestinciduntdui.com | (317)-843-9415 | Abc |
| Autumn Nieves 324-8593 Orci. St. (114)-928-1965 nam.nulla@quisquepurus.net | (114)-928-1965 | Abc |
| Ayve Mcneill (Ap #701) 365 Quis Av. (880)-142-0812 pede@lorem.edu | (880)-142-0812 | Abc |
| Barclay Singleton (Ap #619) 7401 Nec, Street (726)-203-3559 vel.nisl.quisque@nibhenim.net | (726)-203-3559 | Abc |
| Barrett Mcclure P.O. Box 849, 9586 At Street (460)-504-2219 risus.varius@pharetraanamac.com | (460)-504-2219 | Abc |
| Berk Cash (Ap #461) 817 Sapien. Rd. (148)-568-3717 nunc@eratemper.org | (148)-568-3717 | Abc |
| Boris Colon 2386 Pede. Avenue (692)-897-2476 sed@elitpede.net | (692)-897-2476 | Abc |
| Brady Armstrong 651-255 Dui Rd. (766)-679-1355 eget.metus@egetmetus.co.uk | (766)-679-1355 | Abc |
| Brenden Murray 816-1192 Fringilla Rd. (738)-548-7286 neque.non.quam@purus.org | (738)-548-7286 | Abc |
| Breanna York 2456 Nullam Road (713)-794-9922 risus.donec@ametfaucibus.net | (713)-794-9922 | Abc |
| Catherine Hess (Ap #134) 8538 Fringilla, St. (541)-955-2468 a.dui.cras@lectusantedictum.ca | (541)-955-2468 | Abc |
| Chancellor Sellers 522-3985 Vehicula Rd. (175)-854-2222 nec.cursus@orciutsagittis.net | (175)-854-2222 | Abc |
| Charissa Ochoa 960-8336 Dignissim Av. (410)-709-3045 augue@sit.org | (410)-709-3045 | Abc |
| Chastity Spencer 873-2646 Praesent St. (806)-616-0063 eget.nisi@ac.ca | (806)-616-0063 | Abc |
| Ciara Rios 529-8517 Tellus St. (374)-422-2206 aliquet.magna@egestasnuncsed.org | (374)-422-2206 | Abc |

图 2.17：提取数据最终视图

现在，让我们更详细地考虑前一个练习中的一些具体内容。

考虑以下代码块：

```
REGEXP_EXTRACT([String of Data], '()')
```

此处故意排除了表达式模式，因为将在稍后详细介绍。' ( ' ) 代码充当表达式模式的占位符。本示例中使用的 REGEXP_EXTRACT 函数在 Tableau 帮助文档中的描述如下：

Returns a substring of the given string that matches the capturing group within the regular expression pattern.

使用此函数时，请牢记以下几点重要注意事项：

- 请注意，截至撰写本文时，Tableau 文档未说明如何确保函数的模式输入部分正确分隔。对于本示例，请务必在模式输入部分周围包含 ' ( ' )，以避免输出为 null。
- 在本身嵌套在 VizQL 查询中的计算字段内嵌套可能会影响性能（如果嵌套/聚合层级过多）。
- 有许多正则表达式网站允许您输入自己的代码，并根据您提供的示例数据即时反馈来帮助您。http://regexpal.com/ 只是其中一个网站，请按需搜索以找到满足您需求的网站！

现在，请考虑以下表达式：

```
\(([0-9]{3}))[0-9]{3}-[0-9]{4}
```

在此上下文中，\ 表示下一个字符不应被视为特殊字符，而应被视为普通字符。在我们的示例中，我们实际上是在查找左括号。[0-9] 实例仅声明我们正在查找一位或多位数字。或者，请考虑使用 \d 以实现相同的结果。{3} 实例表示我们正在查找三个连续数字。

与模式开头的左括号一样，\ 字符将右括号指定为普通字符。– 字符是专门查找连字符的普通字符。表达式模式的其余部分应根据前面的信息可以解读。

回顾本练习后，您可能好奇如何仅返回电子邮件地址。根据 http://www.regular-expressions.info/email.html，符合 RFC 5322 标准的电子邮件地址的正则表达式如下：

```
(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9]))\.){3}(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9])|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])
```



```php
$first[$key] = $row['bin'];
$second[$key] = $row['count'];
}

array_multisort($second, SORT_DESC, $pastebin_counts);
echo json_encode($pastebin_counts);
?>
```

我们可以通过查看脚本的输出来查看此脚本在测试表上运行时生成的 JSON 输出。我的随机行产生了以下计数：

```json
[{"bin":"jsfiddle","count":44},{"bin":"jsbin","count":4},{"bin":"pastebin","count":3},{"bin":"dpaste","count":0},{"bin":"gists","count":0},{"bin":"pastie","count":0}]
```

> > 记住，由于您拥有不同的随机选择的 URL 集合，因此您的数据可能不同。

当我们进入本章的“第 5 步 – 可视化数据”部分时，我们将使用此 JSON 代码构建条形图。但首先，让我们回答之前提出的另外两个问题。

## 哪些粘贴站点在问题中很受欢迎，哪些在答案中很受欢迎？

我们的第二个问题是 pastebin URL 在问题帖子中更常见还是在答案帖子中更常见。为了开始构建答案，我们将运行一系列 SQL 查询。第一个查询只是询问 clean_posts_urls 表中每种类型的帖子有多少，既有问题也有答案：

```sql
SELECT tp.postTypeId, COUNT(cpu.id)
FROM test_posts tp
INNER JOIN clean_posts_urls cpu ON tp.Id = cpu.postid
GROUP BY 1;
```

结果显示，在我随机选择的测试集中，我有 237 个问题和 440 个答案：

| 帖子类型 ID | 计数(cpu.id) |
|------------|---------------|
| 1 | 237 |
| 2 | 440 |

phpMyAdmin 显示了问题 URL 和答案 URL 的计数。

现在，我们想知道这个问题的答案：在这 677 个 URL 中，按问题和答案划分，有多少个专门引用了六个 pastebin URL 中的一个？我们可以运行以下 SQL 代码来找出答案：

```sql
SELECT  tp.postTypeId, count(cpu.id)
FROM test_posts tp
INNER JOIN clean_posts_urls cpu ON tp.Id = cpu.postId
WHERE cpu.url LIKE '%jsfiddle%'
OR cpu.url LIKE '%jsbin%'
OR cpu.url LIKE '%pastebin%'
OR cpu.url LIKE '%dpaste%'
OR cpu.url LIKE '%gist%'
OR cpu.url LIKE '%pastie%'
GROUP BY 1;
```

结果如下表所示。总共有 18 个问题引用了其中一个粘贴站点，而 24 个答案引用了其中一个粘贴站点。

![](img/9aa003afabe05fdb07da46ad71325de6_228_0.png)

phpMyAdmin 显示了引用 pastebin 的问题和答案 URL 的计数。

关于这些查询需要注意的一点是，它们统计的是单个 URL。因此，如果某个 postId 引用了五个 URL，则会被计算五次。如果我有兴趣知道有多少帖子至少使用了一次粘贴站点 URL，我需要按如下方式修改两个查询的第一行。此查询统计 URLs 表中的不同帖子：

```sql
SELECT tp.postTypeId, COUNT(DISTINCT cpu.postId)
FROM test_posts tp
INNER JOIN clean_posts_urls cpu ON tp.Id = cpu.postId
GROUP BY 1;
```

以下屏幕截图显示了有多少问题和答案包含 URL：

![](img/9aa003afabe05fdb07da46ad71325de6_228_1.png)

phpMyAdmin 显示了有多少问题和答案包含任何 URL。

此查询统计 URLs 表中提及粘贴站点的特定帖子：

```sql
SELECT  tp.postTypeId, count(DISTINCT cpu.postId)
FROM test_posts tp
INNER JOIN clean_posts_urls cpu ON tp.Id = cpu.postId
WHERE cpu.url LIKE '%jsfiddle%'
OR cpu.url LIKE '%jsbin%'
OR cpu.url LIKE '%pastebin%'
OR cpu.url LIKE '%dpaste%'
OR cpu.url LIKE '%gist%'
OR cpu.url LIKE '%pastie%'
GROUP BY 1;
```

此粘贴站点查询的结果如下，正如预期的那样，数字更小。在我们的测试集中，11 个问题至少使用了一个 pastebin URL，16 个答案也是如此。合计，37 个帖子至少引用了一次 pastebin URL。

| 帖子类型 ID | count(distinct cpu.postid) |
|---|---|
| 1 | 11 |
| 2 | 16 |

PhpMyAdmin 显示了有多少问题和答案包含任何粘贴站点 URL。

即使这些结果似乎表明人们在答案中比在问题中更常引用粘贴站点 URL，我们仍需要在问题和答案的总体数量方面进行比较。我们应该将结果值报告为该帖子类型（问题或答案）总数的百分比。考虑到总数，我们现在可以说类似这样的话：“仅考虑至少使用过一种 URL 的问题和答案，81 个问题中有 11 个至少使用了一个粘贴站点 URL（13.6%），222 个答案中有 16 个至少使用了一个粘贴站点 URL（7.2%）。” 考虑到这一点，似乎问题在引用粘贴站点方面实际上超过了答案，几乎是二比一。

在任何数据分析项目的这个阶段，你一定会有大量问题，例如：

- 问题和答案中粘贴站点 URL 的使用情况如何随时间变化？
- 带有粘贴站点 URL 的问题在投票和收藏方面表现如何？
- 发布带有粘贴站点 URL 的问题的用户的特征是什么？

但由于这是一本关于数据清洗的书，而且我们甚至还没有可视化这些数据，我会克制自己暂时不回答这些问题。我们仍有一个原始的三个问题需要回答，然后我们将继续可视化一些结果。

## 帖子是否同时包含粘贴站点的 URL 和源代码？

回答我们的第三个问题需要我们比较 Stack Overflow 问题中的代码量与 Stack Overflow 答案中的代码量，特别关注包含某种源代码的帖子，由 <code> 标签分隔。在“第三步 – 清理数据”部分，我们从测试表中的帖子中提取了所有代码，并创建了一个新表来保存这些代码片段。现在，一个简单的查询来确定有多少包含代码的帖子如下：

```sql
SELECT count(DISTINCT postid)
FROM clean_posts_code;
```

在我样本集中，这产生了 664 个包含代码的帖子，共 1,000 个测试帖子。换一种说法：1,000 个帖子中有 664 个至少包含一个 <code> 标签。

为了确定这些包含代码的帖子中有多少也包含任何 URL，我们可以运行以下 SQL 查询：

```sql
SELECT count(DISTINCT cpc.postid)
FROM clean_posts_code cpc
INNER JOIN clean_posts_urls cpu
ON cpu.postId = cpc.postId;
```

我的样本集对此产生了 175 行。我们可以这样解释：原始 1,000 个帖子测试集中有 17.5% 的帖子同时包含代码和 URL。

现在，为了确定有多少包含代码的帖子也包含粘贴站点 URL，我们将进一步缩小 SQL 查询范围：

```sql
SELECT count(DISTINCT cpc.postid)
FROM clean_posts_code cpc
INNER JOIN clean_posts_urls cpu
ON cpu.postId = cpc.postId
WHERE cpu.url LIKE '%jsfiddle%'
OR cpu.url LIKE '%jsbin%'
OR cpu.url LIKE '%pastebin%'
OR cpu.url LIKE '%dpaste%'
OR cpu.url LIKE '%gist%'
OR cpu.url LIKE '%pastie%';
```

从这些结果可以看出，只有 25 个帖子同时包含源代码和粘贴站点 URL。从第二个问题我们知道，37 个不同的帖子（问题和答案）至少使用了一次某种粘贴站点 URL。因此，25/37 约为 68%。在更大的数据集上运行这些查询以查看这些值的结果将很有趣。

与此同时，我们将对我们的问题中的至少一个进行一些简单的可视化，以便在数据科学六步流程的一轮完整循环上收尾。

## 第五步 – 可视化数据

可视化步骤有点像我们在晚宴中的甜点课程。每个人都喜欢精美的图形，看起来也很漂亮。然而，由于本书的重点是清理而不是分析和可视化，我们这里的图形会非常简单。在下面的代码中，我们将使用 JavaScript D3 可视化库以图形方式显示第一个问题的结果。这种可视化比我们在第 4 章《说通用语 – 数据转换》中所做的 D3 可视化要简单得多。在那一章中，你会记得我们构建了一个相当复杂的网络图，但在本文中，简单的条形图就足够了，因为我们要显示的只是几个标签和计数。

HTML 和 JavaScript/D3 代码如下。此代码扩展了 Mike Bostock 的“让我们构建条形图”教程，可在 http://bl.ocks.org/mbostock/3885304 获取。我扩展此代码的一种方式是使其读取我们在 q1.php 脚本中之前生成的 JSON 文件。我们的 JSON 文件打印得非常漂亮，并且已经按从高到低排序，因此从中构建一个小条形图会很容易：

```html
<!DOCTYPE html>
<meta charset="utf-8">
<!--
this code is modeled on mbostock's
"Let's Make a Bar Chart" D3 tutorial
available at http://bl.ocks.org/mbostock/3885304
My modifications:
* formatting for space
* colors
* y axis labels
* changed variable names to match our data
* loads data via JSON rather than .tsv file
-->
```

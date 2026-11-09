
```
dt = re.search('(Date: )(.+?)\r', line)
    if irt:
        i= irt.group(2)
    if mid:
        m= mid.group(2)
    if dt:
        d= dt.group(2)
    f.close()
    if i and d:
        replies[i] = d
    if m and d:
        originals[m] = d

for (messageid, origdate) in originals.items():
    try:
        if replies[messageid]:
            replydate = replies[messageid]
        try:
            parseddate = email.utils.parsedate(origdate)
            parsedreply = email.utils.parsedate(replydate)
        except:
            pass
        try:
            # this still creates some malformed (error) times
            timeddate = time.mktime(parseddate)
            timedreply = time.mktime(parsedreply)
        except:
            pass
        try:
            dtdate = datetime.datetime.fromtimestamp(timeddate)
            dtreply = datetime.datetime.fromtimestamp(timedreply)
        except:
            pass
        try:
            difference = dtreply - dtdate
            totalseconds = difference.total_seconds()
            timeinhours = (difference.days*86400+difference.seconds)/3600
            # this is a hack to take care of negative times
            # I should probably handle this with timezones but alas
            if timeinhours > 1:
                #print timeinhours
```
```
timelist.append(timeinhours)
    except:
        pass
    except:
        pass

print numpy.mean(timelist)
print numpy.std(timelist)
print numpy.median(timelist)
```

在这段代码中，初始的 for 循环遍历每条消息并提取我们感兴趣的三个数据片段。（此程序不会将这些数据存储到单独的文件或磁盘中，但如果你愿意，可以添加此功能。）这部分代码还创建了两个重要的列表：

- `originals[]` 是原始消息的列表。我们假设这些主要是向列表成员提出的问题。
- `replies[]` 是回复消息的列表。我们假设这些主要是对另一条消息中提出的问题的回答。

第二个 for 循环处理原始消息列表中的每条消息，执行以下操作：如果原始消息有回复，则尝试计算该回复发送所需的时间。我们随后保留一个回复时间列表。

## 提取代码

对于本章，我们主要关注代码的清理和提取部分，因此让我们仔细查看这些行。这里，我们处理电子邮件文件的每一行，查找三个电子邮件头：In-Reply-To、Message-ID 和 Date。我们使用正则表达式搜索和分组，就像本章前面方法 1 中所做的那样，来界定这些头并轻松提取其后的值：

```
for line in f:
    irt = re.search('(In\-Reply\-To: <)(.+?)@', line)
    mid = re.search('(Message\-ID: <)(.+?)@', line)
    dt = re.search('(Date: )(.+?)\r', line)
    if irt:
        i = irt.group(2)
    if mid:
        m = mid.group(2)
    if dt:
        d = dt.group(2)
```

我们为什么决定在这里使用正则表达式而不是基于树的解析器？主要有两个原因：

- 1. 因为我们下载的电子邮件不是 HTML，无法轻易描述为具有父子关系的节点树。因此，像 BeautifulSoup 这样的基于解析树的解决方案并不是最佳选择。
- 2. 因为电子邮件头是结构化且非常可预测的（尤其是我们在这里查找的三个头），正则表达式解决方案是可以接受的。

## 程序输出

此程序的输出是打印三个数字，估计该 Google 群组中消息回复的平均时间、标准差和中位数（以小时为单位）。当我运行此代码时，结果如下：

```
178.911877395
876.102630872
18.0
```

这意味着发布到 BigQuery Google 群组的消息的中位响应时间约为 18 小时。现在让我们考虑如何从另一个来源：网络论坛中提取类似的数据。你认为网络论坛中对问题的回复会比 Google 群组更快、更慢还是差不多？

## 第二部分 – 从网络论坛清理数据

我们将在本项目中研究的网络论坛来自一家名为 DocuSign 的公司。他们还将开发者支持转移到了 Stack Overflow，但他们仍在线存档了旧的基于网络的开发者论坛。我在他们的网站上查找，直到发现如何下载这些旧论坛中的一些消息。这里展示的过程比 Google Groups 示例更复杂，但你将学到很多关于如何自动收集数据的内容。

### 第一步 – 收集指向 HTML 文件的 RSS

DocuSign 开发者论坛上有数千条消息。我们希望获得所有这些消息或讨论线程的 URL 列表，以便编写代码自动下载它们并高效地提取回复时间。

为此，首先我们需要一份所有讨论的 URL 列表。我发现 DocuSign 旧 Dev-Zone 开发者站点的存档位于 https://community.docusign.com/t5/DevZone-Archives-Read-Only/ct-p/dev_zone。

浏览器中该站点看起来如下：

![](img/9aa003afabe05fdb07da46ad71325de6_149_0.png)

我们绝对不想逐个点击这些论坛，然后逐条进入每条消息并手动保存。那样会花费永远，而且极其无聊。有更好的方法吗？

DocuSign 网站的帮助页面指出，可以下载一个 Really Simple Syndication（RSS）文件，显示每个论坛中最新的线程和消息。我们可以使用 RSS 文件自动收集该站点上许多讨论的 URL。我们感兴趣的 RSS 文件是仅与开发者支持论坛相关的那些（不是公告或销售论坛）。这些 RSS 文件可从以下 URL 获取：

- `https://community.docusign.com/docusign/rss/board?board.id=upcoming_releases`
- `https://community.docusign.com/docusign/rss/board?board.id=DocuSign_Developer_Connection`
- `https://community.docusign.com/docusign/rss/board?board.id=Electronic_Signature_API`
- https://community.docusign.com/docusign/rss/board?board.id=Java
- https://community.docusign.com/docusign/rss/board?board.id=php_api
- https://community.docusign.com/docusign/rss/board?board.id=dev_other
- https://community.docusign.com/docusign/rss/board?board.id=Ask_A_Development_Question_Board`

在你的网络浏览器中访问列表中的每个 URL（如果时间紧张，只访问一个）。文件是 RSS，看起来像带有标签的半结构化文本，类似于 HTML。将 RSS 保存为本地系统上的文件，并为每个文件添加 .rss 文件扩展名。完成此过程后，你最多应有七个 RSS 文件，每个对应前面显示的每个 URL。

每个 RSS 文件内部都包含描述论坛上所有讨论线程的元数据，包括我们在此阶段真正想要的一项数据：每个特定讨论线程的 URL。在文本编辑器中打开其中一个 RSS 文件，你就能找到我们感兴趣的 URL 示例。它看起来像这样，并且在文件中，你会看到每个讨论线程都有一个这样的条目：

```
<guid>http://community.docusign.com/t5/Misc-Dev-Archive-READ-ONLY/Re-Custom-CheckBox-Tabs-not-marked-when-setting-value-to-quot-X/m-p/28884#M1674</guid>
```

现在，我们可以编写一个程序循环遍历每个 RSS 文件，查找这些 URL，访问它们，然后提取我们感兴趣的回复时间。下一节将把此过程分解为一系列更小的步骤，然后展示一个完成整个任务的程序。

### 第二步 – 从 RSS 提取 URL；收集并解析 HTML

在此步骤中，我们将编写一个程序执行以下操作：

- 1. 打开我们在步骤 1 中保存的每个 RSS 文件。
- 2. 每当看到 <guid> 和 </guid> 标签对时，提取其中的 URL 并将其添加到列表中。
- 3. 对列表中的每个 URL，下载该位置的 HTML 文件。
- 4. 读取该 HTML 文件并从每条消息中提取原始发布时间和回复时间。
- 5. 计算发送回复所需的时间的平均值、中位数和标准差，就像我们在第一部分中所做的那样。


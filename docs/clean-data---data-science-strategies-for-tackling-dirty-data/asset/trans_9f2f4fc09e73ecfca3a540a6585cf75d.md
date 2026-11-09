
为本书的目的，我们主要关注的是如何处理存在于一种编码中的数据，并通过将其转换为另一种编码来清理数据。可能相关的一些示例场景包括：

- 你有一个使用简单编码创建的 MySQL 数据库（如 MySQL 默认的 256 个字符的 Latin-1 字符集之一），并且它将 UTF-8 数据存储为 Latin-1，但现在你希望将整个表转换为 UTF-8。
- 你有一个 Python 2.7 程序，它使用为 ASCII 设计的函数，但现在必须处理 UTF-8 文件或字符串。

在本节中，我们将根据这些场景逐步介绍几个基本示例。遇到字符编码问题的情况还有很多，但这将是一个起点。

## 示例一 – 在 MySQL 数据中查找多字节字符

假设我们有一个数据列，我们好奇该列中有多少值实际上具有多字节编码。看起来像单个字符但编码需要多个字节的字符可以通过比较其字节长度（使用 length() 函数）和字符长度（使用 char_length() 函数）来发现。

> 以下示例使用的是 MySQL 文档中提供的 MySQL World 数据库的 MyISAM 版本，网址为 http://dev.mysql.com/doc/index-other.html

默认情况下，MySQL World 测试数据库的 MyISAM 版本使用 latin1_swedish_ci 字符编码。因此，如果我们对包含特殊字符的国家名称运行查询，对于 Côte d'Ivoire，我们可能会看到如下内容：

```
SELECT Name, length(Name)
FROM Country
WHERE Code='CIV';
```

这显示该国家的长度为 15，且国家名称 Côte d'Ivoire 被编码为 CÂ™te dÃlvoire。其他列中也有几个条目被奇怪地编码了。为修复此问题，我们可以使用以下 SQL 命令将 name 列的默认排序规则更改为 utf8：

```
ALTER TABLE Country
  CHANGE Name `Name` CHAR(52)
  CHARACTER SET utf8
  COLLATE utf8_general_ci
  NOT NULL DEFAULT '';
```

现在我们可以清空表并再次插入 239 条 Country 行：

```
TRUNCATE Country;
```

现在我们有了使用 UTF-8 编码的国家名称。我们可以通过运行以下 SQL 来测试是否有哪些国家的名称现在使用了多字节字符表示：

```
SELECT *
  FROM Country
  WHERE length(Name) != char_length(Name);
```

它显示 Côte d'Ivoire 和法国岛屿 Réunion 都有多字节字符表示。

如果你无法访问 world 数据集，或者事实上根本没有任何数据集，这里是另一个示例。你可以作为 MySQL select 命令运行多字节字符比较：

```
SELECT length('私は、データを愛し'), char_length('私は、データを愛し');
```

在此示例中，日文字符的长度为 27，但字符长度为 9。

当我们想要测试数据的字符集时，会使用此技术——也许你的行数太多，无法逐一查看，你只想要一个 SQL 语句，可以一次性显示所有多字节条目，以便你制定清理计划。此命令显示了当前具有多字节格式的数据。

## 示例二 – 查找存储在 MySQL 中的 Unicode 字符的 UTF-8 和 Latin-1 等价项

以下代码将使用 convert() 函数和 RLIKE 运算符来打印使用 Latin-1 保存到 MySQL 中的 Unicode 字符串的 UTF-8 等价项。如果你在 MySQL 的 Latin-1 编码文本列中存储了多字节数据，这很有用，这是一个不幸的常见情况，因为 Latin-1 仍然是默认编码（截至 MySQL 5）。

此代码使用公开可用且广泛使用的 Movielens 电影及其评论数据库。整个 movielens 数据集在许多网站上广泛可用，包括原始项目站点：http://grouplens.org/datasets/movielens/。另一个适合 SQL 的链接在这里：https://github.com/ankane/movielens.sql。为了更轻松地完成这些示例，本书作者的 GitHub 站点上提供了仅针对相关行的一个小子集的 CREATE 和 INSERT 语句：https://github.com/megansquire/datacleaning/blob/master/ch2movies.sql。这样，如果你愿意，只需使用该代码创建这一个表并完成以下示例即可。

```
SELECT convert(
    convert(title USING BINARY) USING latin1
) AS 'latin1 version',
convert(
    convert(title USING BINARY) USING utf8
) AS 'utf8 version'
FROM movies
WHERE convert(title USING BINARY)
RLIKE concat(
    '[',
    unhex('80'),
    '-]',
    unhex('FF'),
    ']'
);
```

以下截图显示了在 Movielens 数据库的 movies 表中对 Latin-1 编码的 title 列在 phpMyAdmin 中运行此命令后，前三个电影的结果：

| latin-1 版本 | utf-8 版本 |
|---|---|
| City of Lost Children, The (CitÃ© des enfants perd... | City of Lost Children, The (Cité des enfants perdu... |
| MisÃ©rables, Les (1995) | Misérables, Les (1995) |
| Happiness Is in the Field (Bonheur est dans le prÃ... | Happiness Is in the Field (Bonheur est dans le pré... |

![](img/9aa003afabe05fdb07da46ad71325de6_76_0.png)

> 转换现有数据库到 UTF-8 有什么建议吗？
> 由于 UTF-8 在网络上的普遍性及其在准确传达世界各地自然语言书写信息方面的重要性，我们强烈建议你以 UTF-8 编码方案创建新数据库。以 UTF-8 编码开始会比不这样做容易得多。
> 然而，如果你已经创建了非 UTF-8 编码的表，但尚未填充数据，你将不得不将表更改为 UTF-8 编码，并将每个列的字符集更改为 UTF-8 编码。然后，你就可以插入 UTF-8 数据了。
> 最困难的情况是当你已有大量非 UTF-8 编码的数据，并且希望在数据库中就地转换它时。在这种情况下，你需要做一些规划。你必须根据是否可以通过在少数表和/或少数列上运行命令来解决，还是有一个很长的列和表列表需要调整，来采取不同的方法。在规划此转换时，你应查阅特定于你的数据库系统的文档。例如，在执行 MySQL 转换时，有使用 mysqldump 实用程序的解决方案，也有使用 SELECT、convert() 和 INSERT 组合的解决方案。你必须确定哪一种适合你的数据库系统。

## 示例三 – 在文件级别处理 UTF-8 编码

有时，你需要在文件级别调整代码以处理 UTF-8 数据。想象一个设计用于收集和打印网络内容的简单程序。如果大多数网络内容现在都是 UTF-8 编码的，那么我们的程序内部需要准备好处理它。不幸的是，许多编程语言仍需要一点点引导才能干净地处理 UTF-8 编码数据。考虑以下 Python 2.7 程序示例，该程序设计用于使用其 API 连接到 Twitter 并将 10 条推文写入文件：

```python
import twitter
import sys

#################
def oauth_login():
    CONSUMER_KEY = ''
    CONSUMER_SECRET = ''
    OAUTH_TOKEN = ''
    OAUTH_TOKEN_SECRET = ''
    auth = twitter.oauth.OAuth(OAUTH_TOKEN, OAUTH_TOKEN_SECRET,
                               CONSUMER_KEY, CONSUMER_SECRET)
    twitter_api = twitter.Twitter(auth=auth)
    return twitter_api
#################

twitter_api = oauth_login()
codeword = 'DataCleaning'
twitter_stream = twitter.TwitterStream(auth=twitter_api.auth)
stream = twitter_stream.statuses.filter(track=codeword)

f = open('outfile.txt','wb')
counter = 0
max_tweets = 10
for tweet in stream:
    print counter, "-", tweet['text'][0:10]
    f.write(tweet['text'])
    f.write('\n')
    counter += 1
    if counter >= max_tweets:
        f.close()
        sys.exit()
```

![](img/9aa003afabe05fdb07da46ad71325de6_77_0.png)

如果你担心设置 Twitter 身份验证以获取此脚本中使用的密钥和令牌，请不要担心。你可以按照 https://dev.twitter.com/apps/new 处的简单设置流程进行操作，或者我们在第 10 章《Twitter 项目》中有一个更长、更深入的 Twitter 挖掘示例。在该章节中，我们将完整介绍 Twitter 开发者账户的设置，并更详细地介绍推文收集流程。


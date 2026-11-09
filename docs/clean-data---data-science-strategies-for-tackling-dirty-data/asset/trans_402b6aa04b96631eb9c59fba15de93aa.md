
然而，如果我们在喜爱的搜索引擎上稍微搜索一下，就会发现可以轻松找到一些相当令人印象深刻且实用的通用 URL 匹配模式，这些模式能够处理更复杂的链接模式。这之所以有用，是因为如果我们编写的 URL 提取能够处理更复杂的情况，那么即使未来数据发生变化，它仍然会有效。

一个记录非常详尽的 URL 模式匹配例程在 http://daringfireball.net/2010/07/improved_regex_for_matching_urls 网站上给出。下面的代码展示了如何修改我们的 PHP 代码以使用此模式在 Sentiment140 数据集中进行 URL 提取：

```php
<?php
// connect to db
$dbc = mysqli_connect('localhost', 'username', 'password',
    'sentiment140')
    or die('Error connecting to database!' . mysqli_error());
$dbc->set_charset("utf8");

// pull out the tweets
$select_query = "SELECT id, tweet_text FROM sentiment140";
$select_result = mysqli_query($dbc, $select_query);

// die if the query failed
if (!$select_result)
    die ("SELECT failed! [$select_query]" . mysqli_error());

// pull out the URLS, if any
$urls = array();
$pattern = '#\b(([\\w-]+://?|www[.] )[^\s()<>]+(?:\:[\\w\d]+\)|[^[:punct:]\\s|/]))#';

while($row = mysqli_fetch_array($select_result))
{
    echo "<br/>working on tweet id: " . $row["id"];
    if (preg_match_all(
        $pattern,
        $row["tweet_text"],
        $urls
    ))
    {
        foreach ($urls[0] as $name)
        {
            echo "<br/>---url: ".$name;
            $insert_query = "INSERT into sentiment140_urls (id,
            tweet_id, url)
VALUES (NULL," . $row["id"] . ",'$name')";
            echo "<br />$insert_query";
            $insert_result = mysqli_query($dbc, $insert_query);
            // die if the query failed
            if (!$insert_result)
                die ("INSERT failed! [$insert_query]" .
                mysqli_error());
        }
    }
}
?>
```

此程序几乎与我们之前编写的提及提取程序相同，只有两个例外。首先，我们将正则表达式模式存储在一个名为 $pattern 的变量中，因为它很长且复杂。其次，我们对数据库 INSERT 命令做了少量修改，就像我们为 hashtag 提取所做的那样。

正则表达式模式的逐行完整解释可在其原始网站上找到，但简要说明是，所示模式将匹配任何 URL 协议，例如 http:// 或 file://，并且它还尝试匹配有效的域名模式以及数层深的目录/文件模式。源网站还提供了自己的测试数据集，如果你想查看它将匹配的各种模式以及一些肯定不会匹配的已知模式。

## 第八步 – 为查找表清理

在“第七步 – 分离用户提及、hashtag 和 URL”部分，我们创建了新表来保存提取的 hashtag、用户提及和 URL，然后通过 id 列提供了一种将每一行链接回原始表的方法。我们遵循数据库规范化的规则，创建了表示推文与用户提及之间、推文与 hashtag 之间或推文与 URL 之间一对多关系的新表。在本步骤中，我们将继续优化此表以提高性能和效率。

我们现在关注的列是 query_phrase 列。查看列数据，可以看到它包含反复重复出现的相同短语。这些显然是最初用于定位和选择现在存在于此数据集中的推文的搜索短语。在 sentiment140 表中的 498 条推文中，有多少查询短语是反复重复的？我们可以使用以下 SQL 来检测这一点：

```
SELECT count(DISTINCT query_phrase)
FROM sentiment140;
```

查询结果显示只有 80 个不同的查询短语，但这些短语在 498 行中反复使用。

在 498 行的表中，这可能看起来不是问题，但如果我们有一个极大的表，例如包含数亿行，我们应该对该列担心两件事。首先，反复重复这些字符串会占用数据库中不必要的空间，其次，搜索不同的字符串值速度非常慢。

为了解决这个问题，我们将创建一个 **查找表** 来存放查询值。每个查询字符串在这个新表中只存在一次，我们还将为每个字符串创建一个 ID 号。然后，我们将更改原始表，使其使用这些新的数值而不是当前使用的字符串值。我们实现此目的的步骤如下：

- 1. 创建一个新的查找表：
  ```sql
  CREATE TABLE sentiment140_queries (
      query_id int(11) NOT NULL AUTO_INCREMENT,
      query_phrase varchar(25) NOT NULL,
      PRIMARY KEY (query_id)
  ) ENGINE=MyISAM DEFAULT CHARSET=utf8 AUTO_INCREMENT=1;
  ```
- 2. 使用不同的查询短语填充查找表，并自动为每个短语分配一个 query_id 编号：
  ```sql
  INSERT INTO sentiment140_queries (query_phrase)
  SELECT DISTINCT query_phrase FROM sentiment140;
  ```
- 3. 在原始表中创建一个新列来保存查询短语编号：
  ```sql
  ALTER TABLE sentiment140
  ADD query_id INT NOT NULL AFTER query_phrase;
  ```
- 4. 在下一步出错以防万一备份 sentiment140 表。任何时候我们在表上执行 UPDATE，都最好先进行备份。要创建 sentiment140 表的副本，我们可以使用 phpMyAdmin 等工具轻松复制表（使用 Operations 选项卡）。或者，我们可以重新创建一个表的副本，然后将原始表的行导入其中，如以下 SQL 所示：
  ```sql
  CREATE TABLE sentiment140_backup(
      polarity int(1) DEFAULT NULL,
      id int(5) NOT NULL,
      date_of_tweet varchar(30) CHARACTER SET utf8 DEFAULT NULL,
      date_clean datetime DEFAULT NULL COMMENT 'holds clean,
      formatted date_of_tweet',
      query_id int(11) NOT NULL,
      user varchar(25) CHARACTER SET utf8 DEFAULT NULL,
      tweet_text varchar(144) CHARACTER SET utf8 DEFAULT NULL ,
      PRIMARY KEY (id)) ENGINE=MyISAM DEFAULT CHARSET=utf8;

  SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
  INSERT INTO sentiment140_backup SELECT * FROM sentiment140;
  ```
- 5. 用正确的编号填充新列。为此，我们将两个表按其文本列连接起来，然后从查找表中查找正确的编号值，并将其插入 sentiment140 表中。在以下查询中，每个表都已分配别名 s 和 sq：
  ```sql
  UPDATE sentiment140 s
  INNER JOIN sentiment140_queries sq
  ON s.query_phrase = sq.query_phrase
  SET s.query_id = sq.query_id;
  ```
- 6. 删除 sentiment140 表中的旧 query_phrase 列：
  ```sql
  ALTER TABLE sentiment140
  DROP query_phrase;
  ```

此时，我们有一种有效的方式来创建短语列表，如下所示。
这些按字母顺序显示：

```
SELECT query_phrase
FROM sentiment140_queries
ORDER BY 1;
```

我们也可以通过在两个表之间执行连接来查找具有给定短语（baseball）的推文：

```
SELECT s.id, s.tweet_text, sq.query_phrase
FROM sentiment140 s
INNER JOIN sentiment140_queries sq
ON s.query_id = sq.query_id
WHERE sq.query_phrase = 'baseball';
```

此时，我们有一个清理过的 sentiment140 表和四个新表来保存各种提取和清理后的值，包括 hashtag、用户提及、URL 和查询短语。我们的 tweet_text 和 date_clean 列是干净的，并且我们有一个查询短语的查找表。

## 第九步 – 记录你所做的工作

经过九步清理并使用多种语言和工具，毫无疑问我们会在某个时候犯错并不得不重复某个步骤。如果我们必须向他人描述我们做了什么，我们几乎肯定会难以记住确切的步骤以及我们做每件事的所有原因。

为了在过程中避免错误，保留清理步骤日志至关重要。至少，日志应按执行顺序包含以下内容：

- 每条 SQL 语句
- 每个 Excel 函数或文本编辑器例程，包括必要时的屏幕截图
- 每个脚本
- 关于为何执行每项操作的说明和注释

另一个绝佳的想法是在每个阶段创建表的备份，例如，我们在对 sentiment140 表执行 UPDATE 之前创建了备份，并且我们讨论过在创建新的 date_clean 列后进行备份。备份很容易完成，如果你决定不需要它，随时可以删除备份的表。

## 摘要

在本章中，我们使用了一个示例数据集，即名为 Sentiment140 的推文集合，来学习如何在关系数据库管理系统 中清理和操作数据。我们在 Excel 中执行了一些基本的清理操作，然后回顾了如何将数据从 CSV 文件中取出并导入数据库。此时，其余的清理操作都在 RDBMS 内部完成。我们学习了如何将字符串操作为正确的日期，然后着手从推文文本中提取三种类型的数据，最终将这些提取的值移动到新的、干净的表中。接下来，我们学习了如何创建当前存储效率低下的值的查找表，从而允许我们用高效的数值查找值更新原始表。最后，由于我们执行了许多步骤，并且总有可能出现错误或对我们所做的事情的沟通不畅，我们回顾了一些记录清理过程的策略。

在下一章中，我们将转换视角，从清理给定的数据转向为他人使用而准备清理后的数据。我们将学习一些最佳实践，以创建需要他人最少清理的数据集。

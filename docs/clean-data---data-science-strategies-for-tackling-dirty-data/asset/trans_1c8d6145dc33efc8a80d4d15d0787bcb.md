
此时，我们已经拥有一个完全可用的 MySQL 数据库，其中包含八个表，每个表都已填入数据。然而，这些表非常庞大。仅这八个表就超过 1.9 亿行。在开始清理数据并准备进行分析时，我们会注意到，如果在像 posts、comments、votes 或 post_history 这样的大表上犯错，重建表将花费很长时间。在下一步中，我们将学习如何创建测试表，以便在我们的程序或查询出现问题时将损失控制在最小范围内。

## Building test tables

本节中，我们将构建原始表的八个较小版本，每个版本都使用来自原始表的数据随机填充。

第一步是重新运行 CREATE 语句，但这次在每个表名前加上 test_ 前缀，如下所示的一个表：

```sql
DROP TABLE IF EXISTS test_post_links;
CREATE TABLE test_post_links (
    Id INT NOT NULL PRIMARY KEY,
    CreationDate INT,
    PostId INT,
    RelatedPostId INT,
    LinkTypeId INT
);
```

除了在表名前加上 test_ 之外，这八个测试表与我们之前创建的其他表完全相同。

接下来，我们需要用数据填充新的测试表。我们可以简单地从每个表中选择前 1,000 行并加载到测试表中。然而，这样做的缺点是行是根据插入 Stack Overflow 数据库的时间排序的，因此如果我们只请求前 1,000 行，我们的子集中将不会有来自不同日期和时间的好样本。我们希望所选行具有相当随机的分布。我们如何随机选择一组行？在这本书中我们之前还没有处理过这个问题，所以这又是我们必须准备尝试新事物以确保数据科学聚会顺利进行的一个例子。

选择随机行的可能性有几种，其中一些比其他更高效。在本项目中，效率对我们很重要，因为我们处理的表相当大。使我们的随机行选择比预期更棘手的一点是，虽然我们的表确实有 Id 列作为数字主键，但这些 Id 数字不是顺序的。存在许多空缺，例如，在 post_links 表中，Id 列的前几个值是 19、37、42 和 48。

数据中的空缺是有问题的，因为简单的随机化器操作如下：

- 1. 编写一个 PHP 脚本，查询表中最低和最高的 Id 值，如下所示：
```sql
SELECT min(Id) FROM post_links;
SELECT max(Id) FROM post_links;
```
- 2. 然后，在脚本中，生成 min 和 max 值之间的某个随机数，并请求具有该随机值的行：
```sql
SELECT * FROM post_links WHERE Id = [random value];
```
- 3. 重复步骤 2，直到获取所需数量的行。

不幸的是，在 Stack Overflow 数据库表（例如我们的 post_links 表）中这样做会导致许多查询失败，因为 Id 列中有太多空缺，例如，如果前一个示例中的步骤 2 生成了数字 38 怎么办？我们的 post_links 表中没有 Id 为 38 的记录。这意味着我们需要检测此错误并使用新的随机值重试。

![](img/9aa003afabe05fdb07da46ad71325de6_218_0.png)

> 在这一点上，懂一点 SQL 但不多的人通常会建议我们只需让 MySQL 对包含 Id 的列进行 ORDER BY rand()，然后执行 LIMIT 命令来筛选出我们想要的记录数。这个想法的问题在于，即使我们排序的列是索引列，ORDER BY rand() 也必须读取每一行以便为其分配一个新的随机数。因此，在像 Stack Overflow 数据库中这样的大表上，这完全无法扩展。我们将不得不等待 ORDER BY rand() 查询完成太长时间。ORDER BY rand() 对于小表来说是可以接受的解决方案，但对于我们这里处理的大小则不行。

以下 PHP 脚本展示了我们的最终随机行选择过程如何工作，以构建每个包含恰好 1,000 行的八个测试表。每个表都将由尽可能随机且尽可能省力地选择的行值填充，并且不会过度设计这个简单问题：

```php
<?php //randomizer.php
// how many rows should be in each of the test tables?
$table_target_size = 1000;

// connect to db, set up query, run the query
$dbc = mysqli_connect('localhost','username','password','stackoverflow')
    or die('Error connecting to database!' . mysqli_error());
$dbc->set_charset("utf8");

$tables = array("badges",
    "comments",
    "posts",
    "post_history",
    "post_links",
    "tags",
    "users",
    "votes");

foreach ($tables as $table)
{
    echo "\n=== Now working on $table ===\n";
    $select_table_info = "SELECT count(Id) as c, min(Id) as mn,
        max(Id) as mx FROM $table";
    $table_info = mysqli_query($dbc, $select_table_info);
    $table_stuff = mysqli_fetch_object($table_info);
    $table_count = $table_stuff->c;
    $table_min = $table_stuff->mn;
    $table_max = $table_stuff->mx;

    // set up loop to grab a random row and insert into new table
    $i=0;
    while($i < $table_target_size)
    {
        $r = rand($table_min, $table_max);
        echo "\nIteration $i: $r";
        $insert_rowx = "INSERT IGNORE INTO test_$table (SELECT *
            FROM $table WHERE Id = $r)";
        $current_row = mysqli_query($dbc, $insert_rowx);

        $select_current_count = "SELECT count(*) as rc FROM
            test_$table";
        $current_count= mysqli_query($dbc, $select_current_count);
        $row_count = mysqli_fetch_object($current_count)->rc;
        $i = $row_count;
    }
}
?>
```

运行该代码后，我们可以看到如果需要，我们有一个不错的八个测试表可供使用。使用这些较小的表进行测试可确保我们的清理练习更顺利进行，并且错误可以得到控制。如果我们发现随机表中需要更多行，我们只需提高 `$table_target_size` 命令并再次运行即可。

一旦你知道以简单且有用的方式创建它们是多么容易，建立测试表就是一个很好的习惯。

## Step three – cleaning the data

记住我们的目标是开始分析某些 URL 在问题、答案和评论中被引用的频率，从 Stack Overflow `posts` 和 `comments` 表的文本开始是有意义的。然而，由于这些表非常庞大，我们将改用刚刚创建的 `test_posts` 和 `test_comments` 表。然后，一旦我们确信查询完美运行，我们就可以在更大的表上重新运行它们。

此清理任务与我们在 *第 7 章，RDBMS 清理技术* 中存储从推文中提取的 URL 的方式非常相似。然而，本项目有一套自己的特定规则：

- 由于 posts 和 comments 从一开始就是不同的实体，我们应该为来自 posts（包括问题和答案）的 URL 和来自 comments 的 URL 分别创建表。
- 每个问题、答案或评论内部可能包含多个 URL。我们应该存储所有 URL，并且还应跟踪该 URL 来自哪个 post 或 comment 的唯一标识符。
- 每个问题和答案中也可能包含格式化的源代码。`<code>` 标签用于分隔 Stack Overflow 帖子中的源代码。将代码与帖子分开将有助于我们回答关于粘贴站点 URL 与源代码共存的问题。这样的链接通常会伴随多少代码，如果有的话？

![](img/9aa003afabe05fdb07da46ad71325de6_220_0.png)

> 从技术上讲，posts *可以*在没有 `<code>` 标签的情况下创建，但通常有人会迅速编辑错误的帖子以包含这些有用的标签，并因此获得 Stack Overflow 积分。为简洁起见，在本项目中，我们将假设代码包含在 `<code>` 标签中。

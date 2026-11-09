
- 根据 Stack Overflow 数据库转储文档（可在 http://meta.stackexchange.com/questions/2677/ 获取），实际上共有八种类型的帖子，其中问题和答案只是其中两种类型。因此，我们需要将查询限制为帖子类型 ID 为 `postTypeId=1` 的问题和 `postTypeId=2` 的答案。
- 为确保我们仅从针对问题或答案的评论中提取 URL，而非其他类型的帖子，我们需要将结果联回 posts 表，并将结果限制为 `postTypeId=1` 或 `postTypeId=2`。

## 创建新表

我们需要存储这些 URL 的数据库表的 SQL 创建语句如下：

```
CREATE TABLE clean_comments_urls (
    id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    commentId INT NOT NULL,
    url VARCHAR(255) NOT NULL
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

```
CREATE TABLE IF NOT EXISTS clean_posts_urls (
    id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    postId INT NOT NULL,
    url VARCHAR(255) NOT NULL
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

我们还需要创建一个表来保存从帖子中剥离出的代码：

```
CREATE TABLE clean_posts_code (
    id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    postId INT NOT NULL,
    code TEXT NOT NULL
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

此时，我们有三个新表用于存储清理后的 URL 和清理后的源代码。在下一节中，我们将提取 URL 和代码并填充这些新表。

## 提取 URL 并填充新表

我们可以修改我们在 *第 7 章，RDBMS 清理技术* 中编写的脚本，以在新的 Stack Overflow 环境中提取 URL，方法如下：

```php
<?php // urlExtractor.php
// connect to db
$dbc = mysqli_connect('localhost', 'username', 'password',
    'stackoverflow')
    or die('Error connecting to database!' . mysqli_error());
$dbc->set_charset("utf8");

// pull out the text for posts with
// postTypeId=1 (questions)
// or postTypeId=2 (answers)
$post_query = "SELECT Id, Body
    FROM test_posts
    WHERE postTypeId=1 OR postTypeId=2";

$comment_query = "SELECT tc.Id, tc.Text
    FROM test_comments tc
    INNER JOIN posts p ON tc.postId = p.Id
    WHERE p.postTypeId=1 OR p.postTypeId=2";

$post_result = mysqli_query($dbc, $post_query);
// die if the query failed
if (!$post_result)
    die ("post SELECT failed! [$post_query]" . mysqli_error());

// pull out the URLS, if any
$urls = array();
$pattern = '#\b(([\w]+://|www\.[.])[^\s()<>]+(?:\([\w\d]+\))
|([^\[:punct:]\s]|/))#';

while($row = mysqli_fetch_array($post_result))
{
    echo "\nworking on post: " . $row["id"];
    if (preg_match_all(
        $pattern,
        $row["Body"],
        $urls
    ))
    {
        foreach ($urls[0] as $url)
        {
            $url = mysqli_escape_string($dbc, $url);
            echo "\n---url: ".$url;
            $post_insert = "INSERT INTO clean_posts_urls (id,
            postid, url)
                VALUES (NULL," . $row["Id"] . ",'$url')";
            echo "\n$post_insert";
            $post_insert_result = mysqli_query($dbc,
                $post_insert);
        }
    }
}

$comment_result = mysqli_query($dbc, $comment_query);
// die if the query failed
if (!$comment_result)
    die ("comment SELECT failed! [$comment_query]" .
    mysqli_error());

while($row = mysqli_fetch_array($comment_result))
{
    echo "\nworking on comment: " . $row["id"];
    if (preg_match_all(
        $pattern,
        $row["Text"],
        $urls
    ))
    {
        foreach ($urls[0] as $url)
        {
            echo "\n---url: ".$url;
            $comment_insert = "INSERT INTO clean_comments_urls
            (id, commentid, url)
                VALUES (NULL," . $row["Id"] . ",'$url')";
            echo "\n$comment_insert";
            $comment_insert_result = mysqli_query($dbc,
            $comment_insert);
        }
    }
}
?>
```

我们现在已经完全填充了 clean_post_urls 和 clean_comment_urls 表。对于我随机填充的测试表，运行此脚本仅产生大约 100 条评论 URL 和 700 条帖子 URL。尽管如此，在对完整数据集运行之前，这足以测试我们的想法。

## 提取代码并填充新表

要提取 <code> 标签中嵌入的文本并填充我们新的 clean_posts_code 表，我们可以运行以下脚本。它与 URL 提取器类似，只是它不需要搜索评论，因为评论中没有使用 <code> 标签分隔的代码。

在我版本的随机选择的测试表中，初始 SELECT 产生约 800 行，共 1,000 行 test_post 表。然而，每个帖子可能包含多个代码片段，因此最终表最终包含超过 2,000 行。以下 PHP 代码提取 <code> 标签中嵌入的文本：

```php
<?php // codeExtractor.php
// connect to db
$db = mysqli_connect('localhost', 'username', 'password',
    'stackoverflow')
    or die('Error connecting to database!' . mysqli_error());
$db->set_charset("utf8");

// pull out the text for posts with
// postTypeId=1 (questions)
// or postTypeId=2 (answers)
$code_query = "SELECT Id, Body
    FROM test_posts
    WHERE postTypeId=1 OR postTypeId=2
    AND Body LIKE '%<code>%'";

$code_result = mysqli_query($db, $code_query);
// die if the query failed
if (!$code_result)
    die ("SELECT failed! [$code_query]" . mysqli_error());

// pull out the code snippets from each post
$codesnippets = array();
$pattern = '/<code>(.*?)<\/code>/';

while($row = mysqli_fetch_array($code_result))
{
    echo "\nworking on post: " . $row["Id"];
    if (preg_match_all(
        $pattern,
        $row["Body"],
        $codesnippets
    ))
    {
        $i=0;
        foreach ($codesnippets[0] as $code)
        {
            $code = mysqli_escape_string($dbc, $code);
            $code_insert = "INSERT INTO clean_posts_code (id,
                postid, code)
                VALUES (NULL," . $row["Id"] . ",'$code')";
            $code_insert_result = mysqli_query($dbc,
                $code_insert);
            if (!$code_insert_result)
                die ("INSERT failed! [$code_insert]" .
                    mysqli_error());
            $i++;
        }
        if($i>0)
        {
            echo "\n    Found $i snippets";
        }
    }
}
?>
```

我们现在拥有每个帖子中打印的所有代码的列表，并已将其存储在 `clean_post_code` 表中。

## 第四步 – 分析数据

本节中，我们编写一些代码来回答本章开头提出的三个问题。我们有兴趣查找：

- 帖子和评论中 URL 提及的不同粘贴站点的计数
- 问题与答案中粘贴站点 URL 的计数
- 包含粘贴站点 URL 的帖子中 `<code>` 的出现频率统计

## 哪些粘贴站点最受欢迎？

要回答第一个问题，我们将使用 clean_posts_urls 和 clean_comments_urls 表生成粘贴站点 URL 和计数的 JSON 表示。这种简单的分析将帮助我们了解哪些 pastebin 网站在此 Stack Overflow 数据转储中很受欢迎。以下 PHP 查询数据库中我们在 $pastebins 数组中预列出的粘贴站点，然后对帖子和评论中匹配的 URL 进行计数。它使用测试表，因此数字比使用真实表时要小得多：

```php
<?php // q1.php
// connect to db
$dbc = mysqli_connect('localhost', 'username', 'password',
    'stackoverflow')
    or die('Error connecting to database!' . mysqli_error());
$dbc->set_charset("utf8");

// these are the web urls we want to look for and count
$pastebins = array("pastebin",
    "jsfiddle",
    "gists",
    "jsbin",
    "dpaste",
    "pastie");
$pastebin_counts = array();

foreach ($pastebins as $pastebin)
{
    $url_query = "SELECT count(id) AS cp,
        (SELECT count(id)
        FROM clean_comments_urls
        WHERE url LIKE '%$pastebin%' ) AS cc
        FROM clean_posts_urls
        WHERE url LIKE '%$pastebin%'";
    $query = mysqli_query($dbc, $url_query);
    if (!$query)
        die ("SELECT failed! [{$url_query}]" . mysqli_error());
    $result = mysqli_fetch_object($query);
    $countp = $result->cp;
    $countc = $result->cc;
    $sum = $countp + $countc;

    array_push($pastebin_counts, array('bin' => $pastebin,
        'count' => $sum));
}
// sort the final list before json encoding it
// put them in order by count, high to low
foreach ($pastebin_counts as $key => $row)
{
```

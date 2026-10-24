

响应是通过 requests 库中的 `json()` 函数收集的。HTTP 请求头，`headers`，也提供给 `requests.get()`。请求头实体或 headers 已通过 DevTools 中的 **Copy | Copy as cUrl (bash)** 收集。

如 *Figure 7.4* 所示，`apiUrl` 以 JSON 格式返回 HTTP 响应。为了处理 JSON 内容，requests 库有一个名为 `json()` 的方法（它完成了 JSON 内容的加载和转储，因此无需导入 json Python 库）用于处理 JSON 内容。

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_185_0.png)

Figure 7.4: JSON response from the API

JSON 响应看起来简洁明了，易于处理。已决定仅从所有 JSON 列表中收集 `date` 和 `name`。

以下代码块显示 `date` 和 `name` 条目正被收集到 lists 收集器中：

```
```python
lists=[]                 # empty list
for holiday in response:
    lists.append([holiday['date'],holiday['name']])

lists                     # final list
writeto_csv(lists,'holidays.csv',['Date','Name'])       # Export to CSV
```
```

最后，`lists` 使用预定义的 `writeto_csv()` 函数导出到名为 `holidays.csv` 的文件，其中列标题为 Date 和 Name。

## Example 2 – Open Library book details

Open Library (https://openlibrary.org/developers/api) 提供了一个实验性搜索 API。它为其书籍和类似内容的集合提供了各种 API 相关选项。对于本示例，将使用 https://openlibrary.org/dev/docs/api/search 来查找主题为 python 且作者为 Wes 的书籍数据（结果可通过 https://openlibrary.org/search.json?q=python&author=Wes 获取）。

如以下代码所定义，apiUrl 在 openlibrary.org 中搜索定义的 subject 和 author 变量。apiUrl 的响应为 JSON 格式，但结构嵌套。对 apiUrl 的 HTTP 请求连同几个选定的 HTTP 头一起处理。`json()` 方法和 `requests.get()` 显示内容为 JSON 格式：

```
subject="python"
author="Wes"
apiUrl="https://openlibrary.org/search.json?q="+subject+"&author="+author

headers={
'Upgrade-Insecure-Requests': '1',
'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36'
}
response = requests.get(apiUrl, headers=headers).json()
```

有关书籍的信息提供在各种索引、标题或键下，位于 response['docs'] 内部。字段如 'book_title'、'published_date'、'publisher'、'author' 和 'subjects' 的值（如果可用）被收集到名为 lists 的列表中：

```
lists=[]  # Collector
for book in response['docs']:
    if 'subject_key' in book: # check if subject_key exists
        lists.append([book['title'],
        book['publish_date'][0],
        book['publisher'][0],
        book['author_name'][0],
        "|".join(book['subject_key'])])
    else:
        lists.append([book['title'],
        book['publish_date'][0], book['publisher'][0],
        book['author_name'][0], ''])
```

收集完所有值后，数据随后写入 CSV 文件 python_wes.csv：

```
writeto_csv(
    lists,
    'python_wes.csv',
    ['book_title', 'published_date', 'publisher', 'author', 'subjects']
)
```

收集的数据显示在 Figure 7.5 中：

| book_title | published_date | publisher | author | subjects |
|---|---|---|---|---|
| Python For Data Analysis | Oct 04, 2022 | O'Reilly Media, Incor... | Wes McKinney | data_mining|programming... |
| Python Programming f... | 2019 | Independently Publis... | Jason Wes | |
| Python for Data Analysis | 2019 | Independently Publis... | Clark Wes | |
| Python for Data Analysis | 2012 | O'Reilly Media, Incor... | Wes McKinney | |
| Python for Data Science | 2019 | Independently Publis... | Jason Wes | |
| Python Programming L... | 2019 | Independently Publis... | Clark Wes | |
| Python Para Análise d... | Sep 07, 2000 | Novatec | Wes McKinney | |
| Genuine _ use Python ... | Jan 01, 2014 | Machinery Industry P... | MAI JIN NI (Wes McK... | |
| Python Programming f... | 2019 | Independently Publis... | Tony Wes | |
| Python for Data Analys... | Oct 20, 2017 | O'Reilly Media, Inc. | Wes McKinney | computers|data_mining|da... |
| Python for Data Analysis | 2019 | Independently Publis... | Tony Wes | |

Figure 7.5: Book details from the search API

尽管 publish_date、publisher 和 author_name 有多个值，但只收集找到的第一个数据。本示例中使用的 API 包含许多嵌套且相似的数据块。开发者有责任决定捕获哪些元素。

## Example 3 – US cities and time zones

在本示例中，我们使用免费的 https://api.travelpayouts.com/data/en/cities.json API。该 URL 包含全球城市及其坐标、国家和城市代码以及时区。API 的数据为 JSON 格式，转换为 Python dict 对象列表。有超过 9,000 条记录。

从全球记录中，我们只对美国的详细信息感兴趣。此外，经过简要分析，你会注意到美国城市存在不同的时区，因此我们将数据收集到两个数据集中。zone_america 数据集包含时区包含单词 America 的记录，而 zone_not_america 数据集包含时区不包含单词 America 的城市记录，如以下代码块所示：

```
apiUrl="https://api.travelpayouts.com/data/en/cities.json
response = requests.get(apiUrl).json() # Load API
zone_america=[] # time zone like "America"
zone_not_america=[] # time zone without word "America"
for info in response:
    if info['country_code']=="US" and "America" in
    info['time_zone']:
        zone_america.append([info['country_code'],
        info['code'],info['name'],info['time_zone']])

    if info['country_code']=="US" and "America" not in
    info['time_zone']:
        zone_not_america.append([info['country_code'],
        info['code'],info['name'],info['time_zone']])
```

根据前述代码中实现的条件，从迭代中收集数据。数据写入 zone_america.csv 和 zone_not_america.csv 文件：

```
# Write collected data to CSV
writeto_csv(zone_america,'america_timezone.csv',['country_code','city_code','city_name','time_zone'])
writeto_csv(zone_not_america,'not_america_timezone.csv',['country_code','city_code','city_name','time_zone'])
```

收集的数据显示在 Figure 7.6 中：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_188_0.png)

Figure 7.6: US – time zones with city names and codes

在本示例中，我们过滤 API 结果仅返回美国的城市，并按时区文本是否包含单词 America 进行过滤。将所需数据收集到 CSV 文件后，进行数据分析任务和数据可视化会容易得多。

## Summary

Web APIs 为查找和抓取数据提供了许多好处。由于 API 返回的数据具有预定义格式，你无需使用 XPath 或 CSS 选择器执行额外的查询相关任务，从而节省时间。Web API 结构化、易于访问、可过滤，并且可随时在机器、平台和协议之间交换数据。

在下一章中，我们将学习使用 Selenium 进行自动化，并用它来抓取数据。

## Further reading

- APIs:
  - https://restfulapi.net/
  - https://developer.mozilla.org/en-US/docs/Web/API
  - https://auth0.com/blog/developing-restful-apis-with-python-and-flask/
- JSON:
  - https://www.json.org/json-en.html
  - https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON
- HTTP methods: https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods
- SOAP: https://www.soapui.org/learn/api/soap-vs-rest-api/
- REST:
  - https://www.ibm.com/topics/rest-apis
  - https://pythonbasics.org/flask-rest-api/


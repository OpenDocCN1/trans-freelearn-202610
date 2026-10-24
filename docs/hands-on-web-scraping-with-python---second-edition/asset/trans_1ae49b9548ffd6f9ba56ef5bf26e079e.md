

服务器会识别 cookie 的值，并根据存储在 cookie 中的信息与浏览器进行通信。cookie 中的数据帮助网站访问和传递诸如会话 ID、过期日期以及 cookie 信息存储时间等值。

会话（session）或 Web 会话通常被标识为存储在服务器端（临时）的信息，它在用户与网站交互的整个过程中持续存在。

如图 2.24 所示，会话通常与用户在网站上的凭据相关联：

```
session_obj = requests.Session()
```

```
response_obj = session_obj.get('https://httpbin.org/cookies', cookies={'name': 'anishchapagain','date':'30-dec-2022'})
response_obj.text
```

'{\n  "cookies": {\n    "date": "30-dec-2022", \n    "name": "anishchapagain"\n  }\n}\n'

```
response_obj.headers
```

{'Date': 'Fri, 30 Dec 2022 09:24:12 GMT', 'Content-Type': 'application/json', 'Content-Length': '88', 'Connection': 'keep-alive', 'Server': 'gunicorn/19.9.0', 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Credentials': 'true'}

```
response_test = session_obj.get('https://httpbin.org/cookies')
response_test.text
```

'{\n  "cookies": {}\n}\n'

图 2.24：使用 `requests.Session()` 对象发起 HTTP 请求

## 重要说明

在 HTTP 请求过程中，务必对从 DevTools 或代码输出中获取的 header、cookie 和 session 值进行监控，并经常更新或替换为代理、header/cookie 键等值。在运行爬虫脚本时，使用这些更新后的值和键将有助于绕过服务器异常并检测机器人程序。有关会话和代理的更多信息，请访问 https://requests.readthedocs.io/en/latest/user/advanced/#advanced 。

现在你已经对 HTTP 请求属性（如 cookie、header、session 和方法）有了基本了解。在下一节中，我们将重点讨论 URL 的内容，或者更准确地说，HTTP 响应。

## 响应内容

我们可以使用 `content` 属性以字节形式接收页面或响应，或所请求 URL 的页面源码，而 `text` 属性则返回字符串对象，如图 2.25 所示：

```
response.content[0:500] #content 500 Chars

b'\\n<!DOCTYPE html>\\n<html lang="en">\\n  <head>\\n    <meta charset="utf-8" />\\n    <meta name="viewport" content="width=device-width, initial-scale=1.0" />\\n    <meta name="generator" content="Docutils 0.17.1: http://docutils.sourceforge.net/" />\\n    <title>Requests: HTTP for Humans\\x84&#8212; Requests 2.28.1 documentation</title>\\n    <link rel="stylesheet" type="text/css" href="_static/pygments.css" />\\n    <link rel="stylesheet" type="text/css" href="_static/alabaster.css" />\\n    <link rel="stylesheet" type="text/css" href="_static/ty'

response.text[0:500]

'\\n<!DOCTYPE html>\\n<html lang="en">\\n  <head>\\n    <meta charset="utf-8" />\\n    <meta name="viewport" content="width=device-width, initial-scale=1.0" />\\n    <meta name="generator" content="Docutils 0.17.1: http://docutils.sourceforge.net/" />\\n    <title>Requests: HTTP for Humans\\x84&#8212; Requests 2.28.1 documentation</title>\\n    <link rel="stylesheet" type="text/css" href="_static/pygments.css" />\\n    <link rel="stylesheet" type="text/css" href="_static/alabaster.css" />\\n    <link rel="stylesheet" type="text/css" href="_static/ty'
```

此外，requests 还返回一个原始套接字响应，如图 *2.26* 所示。我们可以通过带 `stream` 参数的 `get()` 方法获取响应：

```
new_response = requests.get(link,stream=True)

new_response.raw.read(200) #first 200

b'\\x1f\\x8b\\x08\\x00\\x00\\x00\\x00\\x00\\x00\\x03\\x00\\x00\\x00\\xff\\xff\\xbc\\x...'
```

原始响应是尚未被自动转换或解码的字符字节。

在收到响应或页面内容后，大多数情况下将其保存到本地文件是相当有用的。这样做是为了研究内容，以揭示代码格式、确定 DOM 树结构或查找元素。如图 *2.27* 所示，我们将这个本地文件命名为 `test.html`：

```
f = open('test.html','wb') #open the file in 'w' mode
f.write(response.content) #write response.content to the file
f.close() #close file handler
```

现在我们有了响应，并且它们保存在一个本地 HTML 文件中。该文件可用于分析 DOM 结构、查找与查询相关的模式以及许多其他用途。我们会遇到不同类型的 HTTP 响应（JSON、PDF、HTML 和 XML），下一节将帮助我们处理 JSON 响应。

## 读取 JSON

requests 凭借其内置解码器非常高效地处理 JSON。你可以在图 2.28 中看到 `json()` 的用法：

```
json_response = requests.get("https://feeds.citibikenyc.com/stations/stations.json").json()
for i in range(5):
    print(json_response['stationBeanList'][i]['stationName'])
```

W 52 St & 11 Ave
Franklin St & W Broadway
St James Pl & Pearl St
Atlantic Ave & Fort Greene Pl
W 17 St & 8 Ave

图 2.28：加载和读取 JSON 内容

Web 抓取和爬行任务可能会使用前面提到的任何过程（HTTP header、获取 cookie 信息、状态码，或将响应内容保存到本地文件）来进行 HTTP 通信以及收集或验证内容。大多数情况下，Python 库提供的方法和属性使事情变得方便而简单。原因如下：

- 某些库中用少量逻辑步骤（如带条件的循环）编写成的简短代码片段，这些片段在需要的地方发挥特定用途，并封装为函数，比过程式编码更方便使用。
- 大多数新库通过嵌入和封装一个或多个函数来克服限制和逻辑约束，因为旧库中已有的新特性由最新库来管理。
- 新库中某些函数可能还有一些冗长且难以管理的额外功能。在这种情况下，开发者会选择使用旧库。

在下一节中，我们将使用 requests 来实现 HTTP GET 和 POST 方法。

## 实现 HTTP 方法

通常，网站与用户之间基于 Web 的交互或通信通过以下方式实现：

- 1. 用户访问一个网页或浏览他们可获得的内容。
- 2. 然后，用户通过 HTML 表单向网站提交信息，例如搜索、登录、注册等，最终收到他们请求的内容。

在本节中，我们将使用 Python requests 库来实现适合上述场景的 HTTP 方法。

GET 是默认的 HTTP 方法。类似地，我们还有 POST，它稍微更严格一些，将在下一节中进行说明。

## POST

POST 请求被认为是向源或服务器发出的安全请求。被请求资源的状态也可能被更改。发布或发送到所请求 URL 的数据在浏览器中不可见（与 GET 查询字符串不同）；相反，它会被传输到所请求的主体中。使用 POST 方法的请求无法被缓存或添加书签，并且在字符长度方面没有限制。

在示例代码中，如图 2.30 所示，使用了 `https://httpbin.org/post` URL（一个简单的 HTTP 请求和响应服务）来演示 POST 请求：

```
```python
pageUrl = 'http://httpbin.org/forms/post'
postUrl = 'http://httpbin.org/post'
params = {'customer': 'Mr Anish', 'custtel': '', 'custemail': 'anish@anishchapagain.com', 'size': 'medium', 'topping': ['cheese', 'mushroom'], 'delivery': '14:45', 'comments': 'No comments'}
headers = {'accept': 'text/html, */*', 'referer': pageUrl}

response = requests.post(postUrl, data=params, headers=headers).json()
print(response)
```
```

图 2.30：HTTP POST 请求

pageUrl 接受要发布的数据（由 params 定义），并发送到 postUrl。自定义 header 被分配给 headers。`post()` 函数接受几个参数：URL（例如 postUrl）、data（要发布的数据）以及 headers，并返回 JSON 格式的响应。

图 2.30 中的 POST 请求代码自动化了 params 和 headers（部分地），并配合所需的 URL，就像我们在 http://httpbin.org/forms/post 上提交实际的 HTML 表单，并从浏览器将其发布到 http://httpbin.org/post 一样。

你也可以从浏览器手动执行相同的任务——打开 DevTools 并验证 Network 面板中使用的参数或 Payload，如图 2.31 所示：


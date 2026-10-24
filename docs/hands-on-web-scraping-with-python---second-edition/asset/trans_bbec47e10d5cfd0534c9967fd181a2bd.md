
在收集所需数据后，`driver.back()` 会再次加载列表页面（它提供即时历史记录，类似于浏览器窗口中可用的后退按钮），并迭代下一个可用的 `<li>` 列表元素。Selenium WebDriver 通过 `back()` 和 `forward()` 方法支持浏览器历史相关控件：

```python
# 返回历史记录（从图书详情页到列表页）
driver.back()
```

由于“儿童”类别中有多个页面，代码会以 `pagination(True)` 值迭代页面，并搜索带有“next”文本的链接。本例中使用 Python 异常处理来验证带有“next”文本的元素是否存在：

- 如果元素存在，则页面计数递增，并加载另一个列表页面
- 如果元素不存在，则捕获 `NoSuchElementException` 异常，通过将分页布尔值更新为 `False` 来禁用页面循环：

```python
try:
    # 在列表页面中检查带有文本 'next' 的分页
    driver.find_element(By.LINK_TEXT,'next').click()
    page = page+1
except NoSuchElementException:
    pagination = False
    print(f"Further Pagination is not possible, currently at {page}")
```

> **重要提示**
> 有关异常处理的更多信息，请访问以下链接：https://docs.python.org/3/tutorial/errors.html 和 https://selenium-python.readthedocs.io/api.html。

在页面和列表的循环完成后，收集在 `dataSet` 中的最终数据被赋予计划好的列名，这些列名列在列表 `['Upc', 'Title', 'Price', 'Rating', 'Stock', 'Stock_Qty', 'Url', 'Image']` 中，并用于创建 CSV 和 JSON 文件。

## 示例 2 – 表单与搜索

在示例 2 中，我们将使用 Selenium 登录、注销，并遍历可用页面，从 http://quotes.toscrape.com/ 收集基于各种引用的数据。与示例 1 类似，我们将从 http://toscrape.com 定位并处理包含“Login”文本的链接。

以下代码搜索带有“Login”文本的锚元素，并收集 `href` 属性。`click()` 操作应用于该“Login”元素。这会加载 http://quotes.toscrape.com/login 页面。WebDriver 有一些属性，例如 `current_url`。该属性返回浏览器中加载页面的 URL：

```python
mainUrl=https://toscrape.com/
driver.get(mainUrl)  # 加载 toscrape.com
loginPage = driver.find_element(By.LINK_TEXT,
    "Login").get_attribute('href')
print(loginPage)
http://quotes.toscrape.com/login

driver.find_element(By.LINK_TEXT, "Login").click()

print(f"Before Login : {driver.current_url}")
http://quotes.toscrape.com/login
```

带有“Login”文本的元素位于所加载页面页脚附近，我们需要向下滚动才能看到该元素。如果浏览器窗口有垂直滚动条（取决于窗口尺寸，如果已配置），`driver.find_element(By.LINK_TEXT, "Login").click()` 代码将垂直向下滚动页面，直到带有“Login”文本的元素可见，然后点击它。这减少了在 4.1.0 版本之前必须编写的窗口滚动相关代码。

http://quotes.toscrape.com/login 页面包含 HTML `<form>` 元素，其中包含几个输入框和一个提交按钮。该表单接受任何凭据，例如 `username=test` 和 `password=test`。由于必须用凭据填充 `<input>` 类型，因此先识别它们，使用 `clear()` 清除，最后使用 `send_keys()` 输入文本值。这些操作自动化了滚动或移动到元素、点击元素以及键入字符的过程：

```python
username = driver.find_element(By.ID, "username")
username.clear()  # 清除可能存在的任何字符
username.send_keys("test") # 输入值
password = driver.find_element(By.ID, "password")
password.clear()
password.send_keys("test")
```

在向相应的输入元素提供用户名和密码值后，必须进行表单提交。提交相关方法在 4.0 版本之前可用，但我们可以通过定位提交表单值的按钮并点击它（使用 `click()` 方法）来实现相同的目的：

```python
driver.find_element(By.CLASS_NAME,'btn').click()
# 点击提交按钮
quotesUrl = driver.current_url
print(f"After Login : {quotesUrl}") http://quotes.toscrape.com/
logoutUrl = driver.find_element(By.LINK_TEXT,
    "Logout").get_attribute('href')
print(f"Logout : {logoutUrl}") http://quotes.toscrape.com/logout
```

提交登录表单后，您会注意到有一个带有“Logout”文本的链接，指向 http://quotes.toscrape.com/logout URL。这验证了登录相关步骤已成功完成。

**重要提示**
读者和开发人员会发现处理 HTML 表单相当容易。如果使用 **DevTools** 跟踪，HTTP POST 方法会随有效负载数据（用户名、密码和 csrf_token）一起提交。

页面上有多个引用 `<div>` 块，带有分页元素（`driver.find_element(By.CSS_SELECTOR,'li.next a').click()`）。`quotes` 标识使用 `find_elements()` 定位的各个引用块。分页使用布尔值管理，并从每个引用块中提取所需或计划的数据。最后，将数据添加到主 `dataSet` 收集器中：

```python
quotes = driver.find_elements(By.CSS_SELECTOR,
    "div.row .quote")
for quote in quotes: # 迭代可用的引用
    content =
        quote.find_element(By.CSS_SELECTOR,'.text').text
    author =
        quote.find_element(By.CLASS_NAME,"author").text
    authorLink =
        quote.find_element(By.PARTIAL_LINK_TEXT,"about")
        .get_attribute('href')
    authorGoodread =
        quote.find_element(By.PARTIAL_LINK_TEXT,"Goodread")
        .get_attribute('href')
    tags =
        quote.find_element(By.TAG_NAME,'meta')
        .get_attribute('content')
```

```python
.....:
# 将值添加到 dataSet
dataSet.append([author, content, tags, tag_count,
    authorLink, authorGoodread])
```

在分页循环和每个页面的引用完成后，最后执行注销操作。注销后，浏览器加载 http://quotes.toscrape.com 页面：

```python
driver.get(logoutUrl)
print(f"Current URL: {driver.current_url}")
http://quotes.toscrape.com/
```

有了数据在手或主 `dataSet` 收集器中，这些数据可以写入 CSV 文件（使用“author”、“quote”、“tags”、“tag_count”、“author_url”、“goodread_url”作为表头）或 JSON 文件。最后，使用 `driver.quit()` 关闭由 WebDriver 加载的浏览器。

本节中的网页抓取示例使用了 selenium 库中的各种标识符、定位器、属性和方法。WebDriver 对于自动化任务（无论是重复性还是非重复性）非常有效。Selenium 常用于 Web 应用相关测试，但我们在这里探讨了它用于抓取的目的。要进一步探索 Selenium，请查看本章中分享的内容和链接。

## 总结

Selenium 框架具有许多功能，并广泛用于应用测试和浏览器自动化。通过探索其功能，我们学习了如何使用 WebDriver 进行与抓取相关的任务。Python 编程确实有独立的库来处理 HTML 或 Web 内容、浏览器属性、网络、解析等。Selenium 可以独立处理这些功能，这是 Selenium 相对于其他各种 Python 库的主要优势。该框架也在不断更新，用测试、自动化和开发人员友好的功能丰富了平台。

在下一章中，我们将学习正则表达式以及如何使用它们来提取或抓取数据。

## 进一步阅读

- Selenium：
  - https://selenium-python.readthedocs.io/
  - https://developer.mozilla.org/en-US/docs/Web/HTML
  - https://www.lambdatest.com/blog/selenium-webdriver-tutorial-with-examples/


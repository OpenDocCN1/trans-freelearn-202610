

## 创建网页抓取器

我们可以使用以下代码从 Barnes and Noble 找到售价：

```python
def get_sellingprice_barnes(isbn):
    url_forbarnes = "http://www.barnesandnoble.com/s/"
    url_for_isbn_barnes = url_forbarnes+isbn
    page = urllib2.urlopen(url_for_isbn_barnes)
    soup_barnes = BeautifulSoup(page, "lxml")
    page.close()
    selling_price_tag = soup_barnes.find('div', class_="price hlight")
    if selling_price_tag:
        print("Barnes Price" + selling_price_tag.string)
```

创建网页抓取器的完整代码可在代码包中找到。

> 💡 之前的抓取器可从库中免费下载

## 小结

在本章中，我们使用 Beautiful Soup 创建了一个示例抓取器。我们使用 Beautiful Soup 的搜索和导航方法，从 packtpub.com、amazon.com 和 barnesandnoble.com 获取信息。

## 索引

### 符号

- `.children` 属性 58
- `.contents` 属性 57
- `.descendants` 属性 58
- `.next_sibling` 属性 62
- `.parent` 属性 60
- `.parents` 属性 61
- `.previous_sibling` 属性 62
- `.string` 属性 59
- `.strings` 属性 60

### A

- **append() 方法**
    - 使用 71, 75

### B

- **Beautiful Soup**
    - 编码 88
    - 格式化打印 93
    - 验证安装 13
    - 安装 7
    - 在 Linux 中安装 7
    - 在 Windows 中安装 10
    - 导航 53
    - 输出 93
    - 输出编码 90
    - 输出格式化器 95
    - 搜索 27
    - 搜索方法 27
    - TreeBuilders 19
    - 非格式化打印 94
    - 无需安装即可使用 12, 13
- **Beautiful Soup 在 Linux 中的安装**
    - 使用 easy_install 9
    - 使用包管理器 8, 9
    - 执行安装 7, 8
    - 使用 pip 9
- **Beautiful Soup 在 Windows 中的安装**
    - 执行安装 10
    - 验证 Python 路径 10, 11
    - 使用 setup.py 12
- **BeautifulSoup 对象**
    - 创建 15
    - 创建用于 XML 解析 18
    - 从类文件对象创建 16, 17
    - 从字符串创建 16
    - features 参数 19

### C

- **clear() 方法**
    - 使用 79

### D

- **decode() 函数** 95
- **decompose() 方法**
    - 使用 77
- **后代** 56
- **直接子节点** 56

### E

- **easy_install** 9
- **encode() 方法** 91, 95
- **extract() 方法**
    - 使用 78

### F

- **features 参数** 19-22
- **类文件对象**
    - 关于 16
    - 从类文件对象创建 BeautifulSoup 对象 16, 17
- **使用 find_all() 方法搜索**
    - 关于 37
    - 使用参数 38, 39
    - 搜索三级消费者 37
- **find_all_previous() 方法** 45
- **find() 方法** 30
- **使用 find() 方法搜索**
    - 基于正则表达式的搜索 32
    - 使用函数搜索 35, 36
    - 组合应用搜索方法 36
    - 基于标签属性值的搜索 33
    - 搜索标签 31
    - 搜索文本 32
- **find_next_sibling() 方法** 43
- **find_next_siblings() 方法** 43
- **find_parents() 方法** 37
- **find_previous_sibling() 方法** 44
- **find_previous_siblings() 方法** 44
- **find_siblings() 方法** 37
- **格式化打印** 93
- **函数格式化器** 99
- **用于内容修改的函数**
    - insert_after() 80
    - insert_before() 80
    - replace_with() 方法 81
    - unwrap() 方法 82
    - wrap() 方法 82

### G

- **get_bookurls() 函数** 106
- **get_text()**
    - 关于 100
    - 使用 100, 101

### H

- **html5lib 解析器**
    - 使用 21
- **HTML 文档**
    - 指定编码 89, 90
- **html 格式化器** 98
- **html.parser**
    - 使用 21

### I

- **insert_after() 方法** 80
- **insert() 方法**
    - 使用 73
- **安装**
    - Beautiful Soup 7

### L

- **limit 参数（find_all() 方法）** 38

### M

- **最小格式化器** 98

### N

- **NavigableString 对象** 24, 53
- **向下导航**
    - 关于 55
    - 使用子标签名 55
    - 使用预定义属性 56
    - 特殊属性 59
- **横向导航至兄弟节点**
    - .next_sibling 属性 62
    - .previous_sibling 属性 62
- **向上导航**
    - 关于 60
    - .parent 属性 60
    - .parents 属性 61
- **Beautiful Soup 导航**
    - 关于 53, 54
    - 向下导航 55
    - 横向导航至兄弟节点 61
    - 导航至解析后的下一个和上一个对象 63
    - 向上导航 60
- **新标签**
    - 使用 append() 方法添加 72
    - 使用 new_tag() 方法添加 72
    - 使用 new_tag() 方法创建 71
    - 向 li 标签添加新的 div 标签 73
    - 使用 new_tag() 和 append() 添加新的生产者 71
- **new_tag() 方法**
    - 使用 71
- **下一个兄弟节点**
    - 查找 44
- **None 格式化器** 99

### O

- **输出编码** 90, 91
- **输出格式化器**
    - 关于 95-97
    - 函数格式化器 99
    - html 格式化器 98
    - 最小格式化器 98
    - None 格式化器 99

### P

- **参数（find_all() 方法）**
    - limit 参数 38
- **pip** 9
- **用于向下导航的预定义属性**
    - 关于 56
    - .children 属性 58
    - .contents 属性 57
    - 后代 56
    - .descendants 属性 58
    - 直接子节点 56
- **prettify() 方法** 91-94
- **上一个兄弟节点**
    - 查找 45
- **Python 包索引（PyPI）** 9

### R

- **replace_with() 方法** 81

### S

- **在 Beautiful Soup 中搜索**
    - 关于 27
    - 使用 find_all() 方法 37
    - 使用 find() 方法 28, 29
    - 搜索下一个兄弟节点 44
    - 搜索父标签 40
    - 搜索上一个兄弟节点 45
    - 搜索兄弟节点 42, 44
    - 搜索标签 40
- **搜索方法**
    - 关于 27
    - 用于从网页抓取信息 46-50
- **使用 find() 搜索**
    - 查找第一个生产者 29, 30
    - 执行搜索 28, 29
- **setup.py 脚本**
    - 使用 12
- **兄弟节点**
    - 搜索 42-44
- **soup.original_encoding** 89
- **修改字符串内容**
    - 关于 73
    - 使用 .string 属性 74
    - 使用 .append() 方法添加字符串 75
    - 使用 insert() 方法添加字符串 76
    - 使用 new_string() 方法添加字符串 76
- **str() 方法** 95

### T

- **使用 find() 方法基于标签属性值搜索**
    - 关于 33
    - 基于 CSS 类的搜索 35
    - 基于自定义属性的搜索 34
    - 查找第一个初级消费者 33
- **使用 Beautiful Soup 修改标签**
    - 添加属性值 69
    - 修改属性值 68
    - 更新属性值 68
    - 修改 name 属性 66-68
    - 添加新标签 71
    - 删除标签属性 70
- **Tag 对象**
    - 关于 22
    - 从 BeautifulSoup 访问 22
    - 属性 23
    - name 23
- **标签**
    - 使用 Beautiful Soup 删除内容 79
    - 从 HTML 文档中删除 77
    - 搜索父标签 40, 41
    - 使用 decompose() 删除生产者 77
    - 使用 extract() 删除生产者 78
    - 搜索 40

### U

- **非格式化打印** 94
- **UnicodeDammit 库** 89
- **unicode() 方法** 94
- **unwrap() 方法** 83
- **UTF-8** 87
- **UTF-8 编码** 89

### W

- **网页抓取器**
    - 查找书籍详细信息 107-109
    - 获取书籍详细信息 103
    - 创建 103, 106
    - 查找包含书籍列表的页面 104-106
    - 从 Barnes and Noble 获取售价 111
    - 在 Amazon 上搜索售价 109, 110
- **wrap() 方法** 82

![](img/c437561ffd11fadb571896e0a4ca4d18_127_0.png)

## 感谢购买

## Beautiful Soup 入门

## 关于 Packt Publishing

Packt，发音为“packed”，于 2004 年 4 月出版了其第一本书《Mastering phpMyAdmin for Effective MySQL Management》，此后一直专注于出版针对特定技术和解决方案的高聚焦度图书。

我们的图书和出版物分享了您的 IT 同行在适应和定制当今系统、应用程序和框架方面的经验。我们基于解决方案的图书为您提供定制所使用软件和技术以完成工作的知识与能力。Packt 的图书比您过去见过的 IT 图书更具体、更少泛泛而谈。我们独特的商业模式使我们能够为您带来更有针对性的信息，让您获得更多需要知道的内容，减少不需要的内容。

Packt 是一家现代而独特的出版公司，专注于为开发者、管理员和新手等群体出版高质量的前沿图书。欲了解更多信息，请访问我们的网站：www.packtpub.com。

## 关于 Packt Open Source

2010 年，Packt 推出了两个新品牌：Packt Open Source 和 Packt Enterprise，以继续专注于专业化领域。本书属于 Packt Open Source 品牌，该品牌专注于出版围绕开源许可证构建的软件相关图书，并为从高级开发者到初出茅庐的网页设计师等各类人群提供信息。该开源品牌还运营 Packt 的开源版税计划（Open Source Royalty Scheme），根据该计划，Packt 会向每本图书所涉及软件对应的开源项目支付版税。

## 为 Packt 撰稿

我们欢迎所有对写作感兴趣的人前来咨询。图书提案请发送至 author@packtpub.com。如果您的图书想法尚处于早期阶段，希望在撰写正式提案之前先进行讨论，请联系我们；我们的一位策划编辑将与您联系。

我们不仅仅寻找已出版图书的作者；如果您拥有扎实的技术技能但缺乏写作经验，我们经验丰富的编辑可以帮助您发展写作事业，或者让您的专业知识获得额外回报。


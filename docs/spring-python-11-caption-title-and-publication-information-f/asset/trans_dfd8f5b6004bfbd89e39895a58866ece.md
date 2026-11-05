
```
return EditPage(article=article,
                wikitext="",
                controller=self)

def create_edit_tuple(self, text, summary):
    return (text,
            summary,
            time.strftime("%H:%M:%S %d %b %Y", time.localtime()))

def updatePage(self, article, wikitext, summary):
    if self.exists(article):
        wiki_db[article][1].append(
            self.create_edit_tuple(wiki_db[article][0],summary))
        wiki_db[article][0] = wikitext
    else:
        wiki_db[article] = [None, None]
        wiki_db[article][1] = [
            self.create_edit_tuple(wikitext,summary)]
        wiki_db[article][0] = wikitext
```

存在一个用于检索页面的方法。它会检查文章是否存在。如果不存在，则返回一个专门的页面以支持创建新页面；否则，返回一个包含检索到的 wiki 文本的普通页面。

存在另一个用于返回编辑页面的方法，用于编辑现有的文章条目。

最后，还有一个用于用新的 wiki 文本来更新当前页面的函数。如果注意一下，在 wiki 文本更新时，会以元组的形式创建一个条目记录，并存储在文章条目的末尾列表中。

让我们定义一些简单的模型对象以在应用程序中传递。首先，我们需要一个页面的基本表示，包含一些基本的 wiki 格式规则。

```
import re

intrawikiR = re.compile("\[\[(?P<link>.*?)( \| (?P<desc>.*?))?\]\]")
externalLinkR = re.compile("\[(?P<link>.*?)\s(?P<description>.*)\]")

class Page(object):
    def __init__(self, article, wikitext, controller):
        self.article = article
        self.wikitext = wikitext
        self.controller = controller

    def link_substitution(self, match):
        g = match.groupdict()
```
```
if self.controller.exists(g["link"]):
    str = '<a href="' + g["link"] + '">' 
else:
    str = '<a href="edit/' + g["link"] + '">' 

if g["desc"]:
    str += g["desc"]
else:
    str += g["link"]

str += "</a>"
return str

def header(self):
    """Standard header used for all pages"""
    return """
    <html>
    <head>
    <title>Spring Python book demo</title>
    </head>

    <body>
        <h1>""" + self.article + """</h1>
    """

def footer(self):
    """Standard footer used for all pages."""
    footer = """
        <ul>
            <li><a href="/edit/""" + self.article + """">Edit</a></li>
            </ul>
            <a href="http://springpythonbook.com">Spring Python book</a>
        </body>
    """
    return footer

def wiki_to_html(self):
    htmlText = self.wikitext
    try:
        htmlText = intrawikiR.sub('<a href="\g<link>">\g<desc></a>', htmlText)
    except:
```
```
htmlText = intrawikiR.sub('<a href="\\g<link>">\\g<link></a>', htmlText)
    htmlText = externalLinkR.sub('<a href="\\g<link>">\\g<description></a>', htmlText)
    return htmlText

def html(self):
    results = self.header()
    results += """
    <!-- BEGIN main content -->
    """
    results += self.wiki_to_html()
    results += """
    <!-- END main content -->
    """
    results += self.footer()
    return results
```

Page 持有文章的标题、与其关联的 wiki 文本以及控制器的句柄。此类中的大部分代码用于支持 html()，该函数用于以 HTML 格式渲染此页面。

![](img/bcf886d8b665372ee8afbb620d4273bb_138_0.png)

> 大多数 Web 框架鼓励使用模板以避免在应用程序中嵌入 HTML。这有助于将页面上显示的信息与显示格式解耦。由于本章的重点是安全而非干净的模型-视图-控制器（MVC）策略，因此 HTML 直接嵌入到应用程序中。

我们的 wiki 页面基本上生成三个部分：一个 header、一些转换为 HTML 的 wiki 文本，然后是 footer()。对于我们的示例，wiki 文本规则很简单：

- 它支持维基百科的自由链接风格，其中 `[[article to link to]]` 映射到本地 Web 路径 `/article_to_link_to`。你也可以插入一个管道，后跟要显示的备用文本块 `[[article to link|my alt text]]`。
- 外部链接 `[http://springpythonbook.com Spring Python book site]` 映射到完整 URL，第一个空格后的文本将在页面上显示

虽然实现更多 wiki 格式规则很容易，但我们希望坚持演示 Web 应用程序的安全性。

接下来我们需要编辑页面的表示。

```
class EditPage(Page):
    def __init__(self, article, wikitext, controller):
        Page.__init__(self, article, wikitext, controller)

    def header(self):
        """Standard header used for all pages"""
        return """
            <html>
            <head>
            <title>Spring Python book demo</title>
            </head>

            <body>
                <h1>Editing """ + self.article + """</h1>
        """

    def wiki_to_html(self):
        htmlText = """
            <form method="post"
                    action="/submit?article=""" + \
                    self.article + """" enctype="multipart/form-data">
                <textarea name="wpTextbox"
                    rows='25' cols='80'>""" + \
                    self.wikitext + """</textarea>
                <br/>
                Summary: <input type='text' value=""
                    name="wpSummary" maxlength='200' size='60'/>
                <br/>
                <input type='submit' value="Save page"
                    title="Save your changes"/>
                <em><a href="/""" + self.article + """" title="""" + \
                    self.article + """">Cancel</a></em>
            </form>
        """
        return htmlText
```

EditPage 本质上是一个具有特殊渲染的 Page。header() 块在文章名称前插入 Editing。HTML 的主要块显示一个包含 wiki 文本、摘要输入框和保存按钮的表单。

让我们创建最终的对象来建模一个不存在的页面。

```
class NoPage(Page):
    def __init__(self, article, controller):
        Page.__init__(self, article,
            "This page does not yet exist.", controller)
```

NoPage 是一个带有硬编码 wiki 文本的 Page。它继承了与 Page 相同的格式规则，并且还具有编辑按钮，因此用户可以用真实页面替换不存在的页面。

让我们创建一个纯 Python 应用程序上下文来连接这些组件。

```
import controller
import view
from springpython.config import PythonConfig
from springpython.config import Object

class SpringWikiAppContext(PythonConfig):
    def __init__(self):
        super(SpringWikiAppContext, self).__init__()

    @Object
    def view(self):
        return view.Springwiki(self.controller())

    @Object
    def controller(self):
        return controller.SpringWikiController()
```

在所有部分都编写完成后，让我们编写一个 CherryPy 服务器应用程序来托管我们的 Web 应用程序。

```
import cherrypy
import os
import noxml
from springpython.context import ApplicationContext

if __name__ == '__main__':
    cherrypy.config.update({'server.socket_port': 8003})

    applicationContext = ApplicationContext(noxml.SpringWikiAppContext())
    cherrypy.tree.mount(
        applicationContext.get_object("view"),
        '/',
        config=None)

    cherrypy.engine.start()
    cherrypy.engine.block()
```

假设我们的控制器代码在 `controller.py` 中，视图代码在 `view.py` 中，应用程序上下文在 `noxml.py` 中，我们应该能够启动我们的 CherryPy 应用程序。

![](img/bcf886d8b665372ee8afbb620d4273bb_141_0.png)

CherryPy 运行自己的 Web 服务器，在本例中我们将其配置为在端口 `8003` 上运行。然后它创建我们应用程序上下文的一个实例。接下来，它获取 `view` 对象，并将其与 Web 路径 `/` 挂载。最后，它启动 CherryPy 引擎并阻塞以处理任何 Web 请求。打开浏览器并指向 http://localhost:8003，你将看到结果。

![](img/bcf886d8b665372ee8afbb620d4273bb_141_1.png)


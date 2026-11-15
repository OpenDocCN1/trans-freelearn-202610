
在*第 4 章*、*绘制可缩放图形*中，您将了解 SVG，它比位图更适合用于图表、图标、动画以及随数据变化的动态图形。在本书的调查项目中，SVG 对于渲染图表和可视化结果将特别有用。

使用生成的图像来支持页面，而不是分散注意力。有用的图像应符合页面的内容、布局和目的。

## 尝试这些快速编辑以证明它是实时的

当预览打开时：

* 将 `<h1>` 和 `<title>` 文本更改为其他内容
* 添加新列表项：`<li>I can preview changes instantly</li>`
* 通过将时间值从 10 改为 15 来编辑表格

您已创建一个基本网页并使用了许多不同的 HTML 元素。您可以创建一个包含只读内容的完整网站，但它将是静态的。使网站具有交互性的最常见方法之一是提供表单供访问者填写并提交。这些可以是用于验证访问者的登录表单，或用于创建或编辑数据的表单。

让我们看看如何创建包含表单和各种输入元素的网页。

# 使用表单和输入元素

到目前为止，您的页面一直是*只读*的。表单是用户在页面中输入信息并将其发送到某处的方式，例如联系表单、登录屏幕、新闻通讯注册、结账等。

其核心是 `<form>` 元素，它包裹您的输入控件（文本框、下拉菜单、按钮）并定义浏览器应如何提交数据。

在本节中，您将：

1. 创建一个名为 `contact.html` 的新页面，包含联系表单
2. 从您的 `index.html` **About Me** 页面链接到它

## 创建 contact.html

在 VS Code 中，在与 `index.html` 相同的文件夹中创建一个新文件：`contact.html`。

粘贴以下内容：

此 HTML 文件可从以下链接获取：[https://github.com/markjprice/web-dev-ai/blob/main/Code/Chapter02/about-me-webpage/contact.html](https://github.com/markjprice/web-dev-ai/blob/main/Code/Chapter02/about-me-webpage/contact.html)

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Contact Me</title>
  </head>
  <body>
    <h1>Contact Me</h1>
    <p>
      Use this form to send me a message. For now, it doesn't actually send email.
      It demonstrates how forms collect data and submit it to a server.
    </p>
    <form action="/contact" method="post">
      <h2>Your details</h2>
      <p>
        <label for="fullName">Full name</label><br>
        <input type="text" id="fullName" name="fullName" required>
      </p>
      <p>
        <label for="email">Email address</label><br>
        <input type="email" id="email" name="email" required>
      </p>
      <p>
        <label for="phone">Phone (optional)</label><br>
        <input type="tel" id="phone" name="phone">
      </p>
      <h2>Your message</h2>
      <p>
        <label for="topic">Topic</label><br>
        <select id="topic" name="topic">
          <option value="question">Question</option>
          <option value="feedback">Feedback</option>
          <option value="business">Business inquiry</option>
        </select>
      </p>
      <p>
        <label for="message">Message</label><br>
        <textarea id="message" name="message" rows="6" cols="40" required></textarea>
      </p>
      <h2>Preferences</h2>
      <p>
        Preferred contact method:
        <br>
        <input type="radio" id="contactEmail" name="preferredContact" value="email" checked>
        <label for="contactEmail">Email</label>
        <br>
        <input type="radio" id="contactPhone" name="preferredContact" value="phone">
        <label for="contactPhone">Phone</label>
      </p>
      <p>
        <input type="checkbox" id="newsletter" name="newsletter" value="yes">
        <label for="newsletter">Send me occasional updates</label>
      </p>
      <p>
        <button type="submit">Send message</button>
        <button type="reset">Clear form</button>
      </p>
    </form>
    <p>
      <a href="index.html">Back to About Me</a>
    </p>
  </body>
</html>
```

有几点需要注意：

* `<form>` 具有 `action="/contact"` 和 `method="post"`
  * `action` 是浏览器发送表单数据的 URL
  * `method="post"` 在请求体中发送数据；`get`（默认）将数据附加到 `?` 后的 URL
* 每个应提交的控件都有一个 `name="..."`。如果您忘记 name，该值将不会包含在提交的数据中。
* 每个文本字段都使用 `<label for="...">` 进行标记，该标记与输入框的 `id` 匹配。这提高了可用性和可访问性。

## 从 index.html 链接到联系表单

打开您现有的 `index.html` 并在合适的地方添加链接，例如在 `Useful link` 部分下方或靠近底部。

例如：

```
<h2>Contact</h2>
<p>
  Want to get in touch?
  <a href="contact.html">Go to my contact form</a>.
</p>
```

因为 `contact.html` 与 `index.html` 在同一文件夹中，所以简单的**相对链接**（如仅 HTML 文件名）即可工作。或者，您可以使用完整 URL 指定**绝对链接**：`https://example.com/contact.html`。

绝对链接是资源（如网页）的完整地址，无论当前页面位置如何均可工作。相对链接是基于当前页面位置的较短地址。如果我在阿拉斯加，想给苏格兰的人寄信，我需要使用他们的完整（即绝对）地址。如果我想给我街上的人寄信，我可以交给邻居，让他们转交给“23 号的 Bob”。这是相对于同一街道上另一个地址的地址。要寄给同城市的人，我们可能还需要街道名。要寄给同州的人，我们还需要城市名。网页等资源有一个绝对地址，但根据您距离其位置的远近，有许多相对地址。

## 在实时预览中尝试

现在我们可以在实时预览中看到结果：

1. 打开 `index.html` 并启动 `Live Preview`，如*图 2.6*所示：
![](Images/B34553_02_06.png)
图 2.6：联系表单链接的实时预览

1. 点击**转到我的联系表单**链接
2. 填写表单并点击**发送消息**

您现在可能会看到：

* 如果您留空必填字段，浏览器将阻止提交并提示您。（这是 HTML 的内置验证。）
* 如果确实提交了，您可能会收到 `Cannot POST /contact` 样式的消息、 `404` 或某些 `not found` 响应，如*图 2.7*所示：
![](Images/B34553_02_07.png)
图 2.7：没有服务器处理表单时出现的文件未找到消息

出现此错误是因为您的浏览器尝试将表单数据发送到 `/contact` 服务器，但目前尚无服务器运行来接收它。这在此时是预期的，因为您仅构建了 HTML 页面，而非处理表单的服务器。

您刚刚触及了前端（HTML）和后端（服务器代码）Web 开发的界限。您将在*第 9 章*、*Django – 后端 Web 开发*中学习如何处理表单提交。

## 常见表单元素快速实用表

学习时可随身携带的速查表如*表 2.1*所示：

| **元素** || **示例** || **用途** |
| --- | --- | --- |

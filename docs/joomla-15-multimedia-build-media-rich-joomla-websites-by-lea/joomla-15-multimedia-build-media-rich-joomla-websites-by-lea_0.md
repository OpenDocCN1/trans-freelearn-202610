# Joomla! 1.5 多媒体 media-rich Joomla! 网站构建指南 通过学习嵌入和显示多媒体内容 - 含索引





## About the Author

**Allan Walker** grew up in New Zealand, and now lives in the United Kingdom, where he runs an online business consultancy and development company called "Amplify" (www.projectamplify.com). In recent years, Allan has been involved with a number of large-scale Joomla! projects, helping to establish Joomla! within the UK government and corporate sectors.

Allan has been a mentor for the 2009 Joomla! Google Summer of Code program, and is a member of the Joomla! Marketing Team.

> > When first starting to write this title, I assumed a couple of full-on weekends might break the back of the book. Nine months later, I have a better understanding of what efforts go into a project like this.

I would like to thank anyone who has contributed in some way to the development of Joomla!. The product provides a diverse solution for projects, and through the ever-growing extensions directory, most users can now configure a Joomla!-based project, rather than have to build it. It is the design and simplicity of Joomla! which leads to its success, thus allowing non-technical users to build complex projects easily.

For many of us who have based services around Joomla!, it has helped put food on the table. For this I am grateful. It is the generosity of people's time, skills, and the community support that keeps the project rolling, and Joomla!'s features improving with every release. If you are interested in helping the product and community, please see information about contributing at www.joomla.org.

For the lost evenings and weekends, my love and thanks go to my wife Georgina. Thank you for the continued support and patience.

## About the Reviewers

**Jose Argudo** is a web developer from Valencia, Spain. After finishing his studies he started working for a web design company. Then, six years later, he decided to start working as a freelancer.

Now that some years have passed, he thinks it's the best decision he has ever taken, a decision that let him work with the tools he likes, such as Joomla!, CodeIgniter, CakePHP, jQuery, and other known open source technologies.

His desire to learn and share his knowledge has led him to be a regular reviewer of books from Packt, such as Joomla! With Flash (http://www.packtpub.com/joomla-with-flash/book), Joomla! 1.5 SEO (http://www.packtpub.com/joomla-1-5-search-engine-optimization-seo/book), Magento 1.3 Theme Design (http://www.packtpub.com/magento-1-3-theme-design/book), or Symfony 1.3 web application development (http://www.packtpub.com/symfony-1-3-web-application-development/book).

Recently he has even published his own book, CodeIgniter 1.7, which you can also find at Packt's site (http://www.packtpub.com/improve-coding-productivity-with-codeigniter-1-7/book). If you work with PHP... take a look at it!

If you want to know more about him, you can check his site www.joseargudo.com.

> > To my girlfriend and to my brother, I wish we will always be together.

**Joseph L. LeBlanc** is a freelance Joomla! developer specializing in Joomla!, PHP, and JavaScript. Joseph can be found teaching Joomla! classes and speaking at conferences. He has authored a book on Joomla! extension programming and a Lynda.com video series on using Joomla!.

# Chapter 4: Adding and Managing Image Content ........................................ 73

+   **Images and why we use them in websites** ........................................ 73
+   **Image formats and which ones to use** ........................................ 75
+   　Digital images ........................................ 76
+   　Lossy and lossless data compression ........................................ 77
+   　Image formats ........................................ 78
+   　　GIF ........................................ 79
+   　　JPEG ........................................ 80
+   　　PNG ........................................ 80
+   　　SVG ........................................ 81
+   **Including images in your content articles and modules** ........................................ 82
+   　Adding an image using the Joomla! Article image button ........................................ 82
+   　Adding an image using the Editor Image button ........................................ 86
+   　Image placement using custom HTML code ........................................ 88
+   **Creating image galleries and slideshows** ........................................ 93
+   　Image management extensions, and how to install them ........................................ 94
+   　　Image gallery extensions ........................................ 96
+   　　Image slideshows ........................................ 103
+   **Joomla! Template images** ........................................ 106
+   　How to adjust your template images ........................................ 109
+   **Browser support and image accessibility** ........................................ 112
+   **Summary** ........................................ 113

# Chapter 5: Using Audio in Your Joomla! Website ........................................ 115

+   **What is audio?** ........................................ 115
+   **Audio and the Internet** ........................................ 116
+   **Audio formats** ........................................ 117
+   　Audio compression ........................................ 118
+   　　Audio codecs ........................................ 118
+   　Audio file format ........................................ 118
+   　Bit rates ........................................ 120
+   　How to compress and encode audio ........................................ 121
+   **Embedding and displaying audio in Joomla! Articles** ........................................ 122
+   　The source ........................................ 123
+   　The encoding ........................................ 123
+   　The upload ........................................ 123
+   　　Via FTP ........................................ 123
+   　　Via the Media Manager ........................................ 124
+   　Editors ........................................ 125
+   　Creating the article ........................................ 127




The audio HTML code 127
  Direct download 127
  Streaming 128
  Embedding code 128
Including audio using a Joomla! Plugin 131
Creating an audio podcast for Joomla! 134
  What is a podcast? 134
  What is RSS? 135
Joomla! and RSS 135
  FeedBurner 136
  Creating a new podcast feed 136
Podcasting using extensions 144
  Third-party audio extensions 145
  Podcast Suite 145
  MP3 Browser 146
  Simple MP3 Player 148
  Jukebox 148
  1 PixelOut Audio Player 150
  Zina 150
Browser support and audio accessibility 151
  Good practice techniques 151
  Text transcripts 151
  Alternative content 152
  Captioning 152
  Link to downloads 152
  Audio controls 152
Summary 153
# Chapter 6: Using Video in Your Joomla! Website 155
  Video on the Internet 155
  Bandwidth limitations 156
  Integration of video with other web content 156
  Authoring and playing video for the Web 156
Video formats 157
  Video compression 158
  Video codec 159
  Bit rates 160
  Video containers 160
    AVI 161
    FLV 161
    MOV 161
    MPEG 162
    WMV 162
    RM 162
Embedding and displaying video in your Joomla! web pages 163
  The source 163
  The encoding 163
  The upload 165
    Via FTP 165
    Via the Media Manager 166
  Editors 167
  Creating the article 169
  The video HTML code 169
    Direct download 169
    Streaming 170
    Embedding code 170
Creating a video podcast 175
Third-party video extensions 176
  AllVideos 176
  hwdVideoShare 178
  Simple Flash Video Player 179
  RokBox 180
  Shadowbox 181
Browser support and video accessibility 182
  Good practice techniques 183
    Text transcripts 183
    Alternative content 183
    Captioning 183
    Video controls 184
Summary 185
# Chapter 7: Collaborating with External Sources 187
Flickr, YouTube, MySpace, Twitter, Facebook, Google... the list goes on 187
Using HTML code to include resources 189
Using third-party extensions for external resources 192
  Joomla! and social networking integration 193
    Twitter Follow Me (M) 193
    AutoTweet NG (C, P) 194
    JTweet (M) 196
    Tweetboard (P) 198
    WebScribble jConnector (C,M,P) 199
    Facebook Fanbox Free (M) 200
    jwFacebook Comments (P) 202
  Joomla! and video channel integration 203
    easiertube (P) 203
    QTube (M, P) 205
    JMultimedia Suite (C, M, P) 206
Joomla! and audio and radio channel integration 207
  LCPlayer - Radio / TV (M) 208
Joomla! and photo integration 209
  UniversalPlayground - Flickr slideshow plug-in (P) 209
  Flickr + Highslide (M) 210
Joomla! and weather integration 212
  Google Weather - Plugin (P, M) 212
  Z Weather (C,M,P) 213
  eWeather (C,M) 214
Joomla! and news channel integration 216
  Google News (M) 216
Other resources and how to integrate them with Joomla! 217
  Plugin GoogleMaps (P) 218
  Ajax Whois (C,M) 219
  SlideShare (P) 220
  Slick RSS (M) 220
  RokBox Plugin (P) 222
  Simple RSS Feed Reader (M) 223
Summary 225
# Chapter 8: Joomla! Templates and Multimedia 227
### What is a Joomla! Template? 228
### Template components 229
### Scripting and multimedia 232
  Mellow template—Yootheme 233
  Infuse—RocketTheme 235
  JA Halite—Joomla Art 238
  Motion—Yootheme 240
### The mobile web 243
### Making your site mobile friendly 245
  Consider these adjustments 246
    Areas of importance 246
    Disabling images, CSS, and JavaScript 247
    Take an on-demand approach with your multimedia 247
    Load in custom stylesheets or templates for mobile devices 247
  Available extensions 248
    PDA-plugin for Joomla! 1.5 (P,T) 248
    WAFL: Mobile Content Adaption (P,C,T,M) 250
    Mobilebot for Joomla 1.5 (P) 251
  Testing for the mobile web 252
    Disable your stylesheet 252
    Resize your browser 253
    Borrow a phone to test with 254
    Target your visitors, and what you want them to see 255
    Use an emulator 255
### Accessibility 256
  Accessible content 256
  Accessible template code 256
  Fonts, colors, and contrasts 257
### Summary 258
# Chapter 9: Joomla! Multimedia Project 259
### Overview 259
  Preliminary advice 260
  Getting started 261
  Local development 262
    Installing the MAMP package 264
    How to use MAMP 267
  Remote server development 270
### The site structure 271
  Things to consider when configuring from the start 272
    Global Configuration settings 272
    Content parameters 277
    Front page 277
    Sections and categories 278
  Choosing a template / theme for your site 280
### Multimedia features 283
  Text content 283
  Module positions 284
  Adjusting the menu 285
  Front page features 286
    The front page component 286
    Adding a module to display our latest news 288
  Adding the date and time 290
  Adding a search feature 291
  Adding images 293
    Adding an image gallery 293
  Adding video features 303
    RokBox, here we go again! 303
    Video using hwdVideoShare 305
    Videos within articles using FlowPlayer 307
    Using the QTube extension to display YouTube videos 309
  Adding audio features 311
    MP3 Browser 312
    Simple MP3 Player 313
    RokBox for audio 314
    Adding podcasts 315
  Adding social profiling features 319
    Easy Twitter Status 319
    Twitter Follow Me 322




## 前言

Joomla! 是一种内容管理系统，旨在在网站环境中组织和发布内容。

多媒体为我们提供了令人惊叹的交互式用户体验和美妙的设计选择，但要有效地利用它，需要纪律和知识，否则我们会疏远我们的受众。

如果您希望在 Joomla! 页面上展示的不只是文本，本书就是为您设计的，必读。它带您超越 Joomla! 的基础知识，并帮助充分利用这种强大的 CMS 结构，为网站用户提供丰富的媒体内容。本书为 Joomla! 管理员提供了创建引人入胜、媒体丰富的 Joomla! 网站所需的详细资料和全部必备知识。利用 Joomla! 的核心功能和 Joomla! 扩展的强大功能，本逐步指南将向您展示如何将流行的媒体元素纳入您的网站并与外部网络资源协作。

您将学习所需的一切知识，以便通过手动嵌入内容、使用更自动化的方法和专用扩展的强大功能，在内容中呈现文本、图像、视频和音频。您将学习如何创建常规播客并利用 RSS 来帮助宣传和发布您的网站内容。

了解 Joomla! 的流行多媒体扩展；了解它们如何通过额外功能使您的 CMS受益，以及它们如何安装和配置。利用现在在网络上可用的大量外部资源，并学习您的 Joomla! 网站如何与 YouTube、Flickr、Twitter 和 Google 等网站进行交互并呈现内容。

## 本书涵盖内容

第 1 章，*开始使用 Joomla! 多媒体* 向您介绍多媒体，分解主要元素，并概述后续章节中将出现的一些内容。您的 Joomla! 内容管理系统包含许多内置的多媒体功能，这些功能通过第三方扩展增强，可以将您基本 Joomla! CMS 转变为媒体丰富的交互式用户体验。

第 2 章，*管理您的 Joomla! 媒体* 告诉我们，使用多媒体在 Joomla! 网站中使用时最重要的任务之一是能够上传和管理您网络服务器上的文件。在本章中，我们将查看使用内置 Joomla! 媒体管理器，以及管理网站媒体和文件的替代方法。

第 3 章，*Joomla! 网站中的文本、字符和字体* 显示文本（及其样式）是网站页面中使用最广泛、至今仍在使用的媒体类型之一。尽管它是如此重要的媒体元素，网络开发人员往往对此关注甚少。在本章中，我们将重点介绍字体、字符和文本在 Joomla! 网站中的使用，以及我们如何通过使用另一种字体类型来改变网站设计，从而影响您网站用户的整体体验。

第 4 章，*添加和管理图像内容* 将提供帮助，以在 Joomla! 网站中有效利用图像内容，无论您需要在 Joomla! 网站中创建多媒体图像画廊，还是只需将图像添加到文章和模块中。

第 5 章，*在 Joomla! 网站中使用音频* 突出了音频在 Joomla! 网站中的使用。您将学习如何将强大的音频功能（如音频播放器和播客功能）纳入 Joomla!。

第 6 章，*在 Joomla! 网站中使用视频* 突出了视频媒体在 Joomla! 网站中的使用。我们将查看将视频内容嵌入文章的过程，创建和维护视频播客，以及第三方 Joomla! 扩展提供的强大视频解决方案。

第 7 章，*与外部来源协作* 谈论了在 Twitter、Facebook 和 Google 等流行外部资源之间共享信息是如何越来越受欢迎的 Joomla! 用户所需的功能。旨在扩展 Joomla! 框架以提供新功能，本章将介绍 Joomla! 最受欢迎的协作扩展及其包含的功能，以及使用老式 HTML 方法将外部内容嵌入到您的网站中。

## 惯例

在本书中，您会发现多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例，以及它们的含义。

代码词在文本中显示如下："我们可以通过使用 `include` 指令来包含其他上下文。"

代码块设置如下：

```
```css
body {
    font-family: Arial,Courier,sans-serif;
    line-height: 1.3em;
    margin: 0px;
    font-size: 12px;
    color: #333;
}
```
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
```css
h1.logo a
{
    **width: 208px;**
    display: block;
    background: url(../images/logo.png) no-repeat;
    **height: 80px;**
    position: relative;
    z-index: 100;
}
```
```

**新术语和重要词语**以粗体显示。您在屏幕上看到的、菜单或对话框中的词语，例如，出现在文本中如下："您的 Joomla! 网站 **媒体设置** 可以通过转到 **站点 | 全局配置** 找到。"

![](img/0151410972a5c5ea0aab26416e44b1da_18_0.png)

![](img/0151410972a5c5ea0aab26416e44b1da_18_1.png)

## 读者反馈

我们始终欢迎读者的反馈。请让我们知道您对本书的看法——您喜欢什么或可能不喜欢什么。读者反馈对我们开发您真正受益的书名非常重要。

要发送一般反馈，只需发送电子邮件至 `feedback@packtpub.com`，并在消息主题中提及书名。您也可以在 `jmultimedia@projectamplify.com` 与作者分享您的反馈。

如果您需要一本书并希望我们出版，请在 `www.packtpub.com` 上的 **建议书名** 表单中发送给我们说明，或发送电子邮件至 `suggest@packtpub.com`。

如果您在某个主题上有专业知识，并且有兴趣撰写或为一本书做出贡献，请参阅 `www.packtpub.com/authors` 上的作者指南。

## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们有一系列帮助您充分利用购买的项目。

![](img/0151410972a5c5ea0aab26416e44b1da_19_0.png)

> **下载本书的示例代码**
> 
> 访问 `http://www.packtpub.com/files/code/7702_Code.zip` 直接下载示例代码。
> 
> 可下载文件包含如何使用它们的使用说明。

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果您在我们的书中找到错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。通过这样做，您可以节省其他读者的挫败感，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 `http://www.packtpub.com/support`，选择您的书籍，单击 **让我们知道** 链接，并输入您的勘误详情。一旦您的勘误得到验证，您的提交将被接受，勘误将被上传到我们的网站，或添加到该标题现有勘误列表中，在该标题的勘误部分下。任何现有勘误都可以通过选择标题从 `http://www.packtpub.com/support` 查看。




## 盗版

互联网上的版权内容盗版是所有媒体领域持续存在的问题。在 Packt，我们非常重视版权和许可证的保护。如果您在互联网上发现我们作品的任何非法副本（无论何种形式），请立即提供位置地址或网站名称，以便我们寻求法律补救。

请将涉嫌盗版内容的链接发送至 copyright@packtpub.com 与我们联系。

我们感谢您在保护我们作者以及向我们提供有价值内容方面提供的帮助。

## 疑问

如果您在使用本书的任何方面遇到问题，可以通过 questions@packtpub.com 联系我们，我们将竭诚为您解决。

## 快速入门 Joomla! 多媒体

目前，网页中多媒体内容的应用正以惊人的速度发展。每周都会在 http://extensions.joomla.org 扩展目录中发布新的多媒体扩展。几乎同时，新的社交/媒体网络门户也层出不穷，它们提供了将媒体内容交互和流媒体传输到您网站页面的功能。其中一些流行的门户正在定义新的多媒体功能，并为网站用户交互树立新的标准。

您的 Joomla! 内容管理系统拥有许多出色的内置多媒体功能；这些功能通过第三方扩展和流媒体源的丰富，可以帮助将您的默认 Joomla! 网站演变为面向访客的强大且丰富的媒体交互体验。

本章是多媒体的介绍，以及 Joomla! 如何与这些媒体元素进行交互。

### 什么是多媒体？

**多媒体**的定义是一种模棱两可的描述，它被宽松地用来定义多种相互集成并共同使用的媒体类型。多媒体遍布我们的日常生活，并应用于包括营销、广告、艺术、医学、工程在内的许多领域！

多媒体的例子可以是从简单的两种媒体类型混合（如包含文字和图片的书籍），到先进的交互视频游戏（包括图像、视频、文本和音频）。

如描述所示，“多媒体”包含不同类型的媒体内容，而这些不同的媒体元素通常可以归类。

### 多媒体的类别

从学术角度来看，多媒体通常被定义为两个主要类别：线性和非线性。超媒体是第三种定义，虽然它通常被视为独立的分类，但它属于非线性多媒体类别。

### 线性

线性多媒体内容通常像演示文稿一样推进，不需要观看者进行交互或导航。线性多媒体的例子包括电影院里的商业大片或电视节目。

### 非线性

非线性多媒体内容是一种允许用户控制并推进自身体验的媒体类别。非线性多媒体的例子可以是交互式视频游戏或基于计算机的学习应用程序。

### 超媒体

超媒体是非线性类别的成员。它是“超文本”一词的扩展，意味着用户可以点击并导航到另一个区域的链接文本。

超媒体可以包含相互链接的文本、图像、视频和音频内容。由于其导航属性，超媒体是网页的基本元素。超媒体的最佳例子是万维网（WWW）本身，它包含数百万个提供相互链接媒体内容的网页。

### 网站多媒体

网站多媒体描述了通过 Web 服务器或网站页面以电子方式传递给最终用户的多种媒体和内容形式。网站多媒体通常归入非线性多媒体分类，因为通常情况下，它为最终用户提供了导航和控制的机会。网站使用的媒体类型可以包括文本、图像、音频和视频，但也可以包含许多符合“多媒体”描述的其他交互功能。

在这本书中，我们将探讨这些多媒体元素，以及如何在我们的 Joomla! 网站中有效地显示它们。

### 各种类型的网站多媒体

互联网最初是从简单的文本内容开始的。随着 HTML 和其他编程语言的实施，以及 Web 浏览器和连接速度的演变，媒体丰富的应用程序和网站如今遍布万维网。

## 快速入门 Joomla! 多媒体

多媒体直接影响了互联网的增长，正如互联网也影响了多媒体一样。在线信息的扩展正在成为最重要的多媒体载体。Web 用户现在能够按需控制数据的传输，而用户交互性已成为流行网站使用的关键过程。

![](img/0151410972a5c5ea0aab26416e44b1da_24_0.png)

如本章前面所述，多媒体是多种形式内容的组合。这些内容可以包括文本、图像、音频、视频以及脚本和许多其他网站功能，这些功能有助于网站用户的导航和交互。

### 什么是 Joomla!？

Joomla! 是一个屡获殊荣的开源内容管理系统（CMS），使您能够构建强大的数据库驱动的网站应用程序。

Joomla! 允许您轻松构建、组织和管理网站内容。Joomla! 本身是一个快速演变的产品，支持其增长的团队和社区也是如此：

![](img/0151410972a5c5ea0aab26416e44b1da_25_0.png)

Joomla! 创建可公开查看的页面，并包含一个由 Web 驱动的管理区域，可通过登录屏幕访问：

![](img/0151410972a5c5ea0aab26416e44b1da_25_1.png)

Joomla! 在全球各地被用于驱动各种形状和大小的内部网络和互联网网站。

Joomla! 的使用示例包括：
- 在线杂志、新闻门户和出版物
- 政府应用程序
- 学校和教堂网站
- 公司网站和内部网络
- 个人网页和博客
- 音频和视频门户

Joomla! 已成为许多人喜爱的 CMS。其快速且持续增长的原因包括：
- Joomla! 是一个免费向所有人开放的开源应用程序。有关此主题的更多信息，请访问 http://www.opensourcematters.org。
- 该产品具有结构完善的路线图，用于持续开发和支持。
- Joomla! 拥有一个充满活力的成长社区，创建了一个优秀的帮助和支持网络。
- 有数千个第三方扩展可用，它们提供了易于安装的附加功能。这使得强大的新功能可以轻松添加到您的 Joomla! 网站中，只需几次鼠标点击。
- 模板结构允许您完全自定义 Joomla! 网站的外观和感觉。
- 后端管理包含强大的功能，灵活且易于使用。

### 在 Joomla! 站点中包含多媒体

就像手工编码的网站一样，您的 Joomla! 网站可以轻松包含多媒体站点内容。事实上，Joomla! CMS 的构建旨在轻松创建、管理和展示多种内容类型，允许您在站点页面上发布这些内容。

将多媒体材料纳入软件驱动的 Web 框架（如 Joomla! CMS）的方法可能与手工编码的网站不同。使用的媒体元素需要按照 Joomla! CMS 框架的方式包含和部署。

Joomla! CMS Web 平台专为内容管理而设计，并尽可能减少额外杂乱。开箱即用，它在管理内容方面做得非常出色，但应用程序框架为全球成千上万的开发者提供了为基本 CMS 贡献额外功能的能力。这些被称为 Joomla! 扩展。

Joomla! 扩展是第三方产品，旨在增强核心 Joomla! 功能。它们形态各异，可以为默认的 Joomla! 添加强大功能。有数千个 Joomla! 扩展可在位于 http://extensions.joomla.org 的 Joomla! 扩展目录（JED）以及互联网上可用的其他 Joomla! 资源中找到。

![](img/0151410972a5c5ea0aab26416e44b1da_27_0.png)

多媒体扩展构成了 JED 的一个重要部分，我们将在接下来的章节中介绍一些最强大和最有用的扩展。





*Joomla! 多媒体入门*

## Joomla! CMS 中的多媒体使用

Joomla! CMS 的强大功能之一在于其组织和内容管理能力。这些内容可以是文本、图像、文档、音频、视频，以及你能想到的任何其他形式，用于提供给网站用户。

Joomla! 自带“媒体管理器”工具。它允许你将不同类型的媒体上传、整理和删除到 images 目录，或者如果你愿意，也可以上传到子目录。媒体管理器提供缩略图视图以及文件属性的详细视图，例如名称、文件大小等。

内置媒体管理器的目的是让网站管理员能够直接在网站页面中管理媒体内容，而无需通过文件传输协议（FTP）访问服务器。作为开发人员，使用 FTP 是一种基本的传输方法，但在将基于 Joomla! 的网站移交给最终客户时，限制这种访问是常见需求。

一旦文件放置在 Web 服务器上，一切就有趣了。我们的媒体已经就位，一切就绪，接下来呢？在我们深入之前，现在是时候着重介绍 Joomla! 模板了。

Joomla! **模板**是 Joomla! CMS 中的一组文件，用于控制内容的呈现。模板允许你在网站页面上获得完全不同的“外观和感觉”。当与图像、层叠样式表（CSS）和 CMS 数据库内容组合时，模板有助于网站的整体设计和布局。

在生成网站页面时，主要元素的位置通常保持不变；这些元素可能是菜单、横幅等。这些页面可能还需要相同的徽标、颜色和样式，这就是我们的模板和层叠样式表发挥作用的所在。模板还有一个重要作用，就是允许我们将特定的 Joomla! 内容发布到模板的特定区域。

CMS 和模板方法不仅使我们能够将多种媒体内容投放到网站模板的不同位置，而且还能灵活地在需要时展示我们想要的内容。我们将用一整章来介绍 Joomla! 模板以及它们如何增强我们网站的多媒体功能。

目前，重点强调的是，开箱即用的 Joomla! 似乎只提供基本的网站内容管理。但凭借对产品的了解以及额外的扩展和配置，它可以提供一个充满多媒体功能的惊艳 Web 解决方案。

## 文本

文本由字母、数字、字符、单词和符号组成。它们组合在一起形成词语，传达思想、信念或事实。文本是使用最广泛的交流形式，被视为视觉多媒体的基本元素。

文本在网站上无处不在，甚至用于许多用户看不见的网站部分。其中一个例子就是网站页面的 HTML 源代码：

```
```php
<?php if ($this->params->get('show_page_title', 1) && ...
    <div class="componentheading">...
        <?php echo $this->escape($this->params->get(...
    </div>
<?php endif; ?>
<?php if (condition || $this->params->get('show_title'...)
<table class="contentpaneopen">
<tr>
    <?php if ($this->params->get('show_title')) : ?>
    <td class="contentheading">
        <?php if ($this->params->get('link_titles') && ...
        <a href="<?php echo $this->article->readmore_l...
            <?php echo $this->escape($this->article->t...
        <?php else : ?>
            <?php echo $this->escape($this->article->t...
        <?php endif; ?>
```
```

文本可用于页面的标题、主要内容、标题说明和帮助提示、文档、错误报告，基本上遍布你网站的每个角落。

字体是一种特定的文本样式，在现代网站的外观和感觉中起着重要作用。如今，字体在整个 Web 中被广泛用于样式化和定义文本。如果没有字体，我们所看到的所有文本都会拥有相同的外观，这会让一切变得索然无味。

![](img/0151410972a5c5ea0aab26416e44b1da_29_0.png)

在现代网站中，文本及其希望应用的样式类型通常使用样式表来定义。在样式表文档中，可以设置字体和文本属性。这些属性可以应用到网站页面上使用的小范围或大范围的文本：

![](img/0151410972a5c5ea0aab26416e44b1da_30_0.png)

## 图像

除了书面文本，图像可能是网站中使用最多的媒体类型之一。

图像不仅能有效传达信息（俗话说“一图胜千言”），而且它们也是设计、营销和品牌推广的重要元素，可能让你的网站从其他网站中脱颖而出。

“图像”的定义往往可以用来描述以下元素：

- 照片
- 图形
- 示意图
- 绘图

图像可以作为单一元素放置在网页上以补充文本，也可以组合在一起形成图库，在某些网站上扮演着重要的多媒体角色：

![](img/0151410972a5c5ea0aab26416e44b1da_31_0.png)

图像可以创建、编辑并保存为多种格式。其中一些格式比其他格式更适合在 Web 上使用。

有关图像的更详细信息，请参阅第 4 章“添加和管理图像内容”。

## 音频

音频（或我们熟知的声频）可用于激发人类情感和传递信息。音频是人类娱乐的重要元素，也是视频、游戏和其他多媒体演示的构成元素。

音频的使用可以增添趣味、提供信息并娱乐大众。一个经常被忽视的事实是，音频还可以为有视觉障碍的用户提供获取所需信息的接口：

![](img/0151410972a5c5ea0aab26416e44b1da_31_1.png)

现在的互联网是一种音频丰富的传输机制，音频可以轻松地从网站和便携设备上的应用程序下载。如今大多数现代计算机配置都包含扬声器，这从某种角度看，促进了网页中音频使用的演变。也可能反过来，网页中更多的音频使用指定了计算机用户需要扬声器的要求。

话虽如此，那些曾经加载到网页上、播放着无法控制音量的圣诞 MIDI 文件的跳舞圣诞老人已经一去不复返了！现在我们谈论的是包含“点播播客”和“拨打你自己的个人电台播放列表”的网站，以及包含计时和播放控件等令人印象深刻的音频播放器。

与图像一样，声音文件可以创建并保存为多种格式，包括 AAC、MP3、MIDI、WAV 等。选择正确的音频格式用于你的网站，对于高效地向网站用户传递音频内容至关重要。

![](img/0151410972a5c5ea0aab26416e44b1da_32_0.png)

## 视频

视频可以定义为一系列静止图像组合在一起并在屏幕上显示，从而形成运动的错觉：

![](img/0151410972a5c5ea0aab26416e44b1da_33_0.png)

视频是最强大的多媒体类型之一，因为它融合了图像和音频。它还可以以标题和描述的形式包含文本。

视频一直被认为是处理起来最苛刻的媒体类型之一，因为处理大文件大小会带来很高的开销。视频的传输和存储常常使其在某些条件下成为不合适的介质。然而，情况正在迅速变化，现在视频通常被链接到应用程序或“流式传输”到应用程序中，而不是直接托管文件本身。

视频可以保存为多种格式和压缩类型。其中一些非常适用，而另一些则应避免在网站中使用。有关处理视频的更多信息，请参阅第 5 章*在 Joomla! 网站中使用视频*。

![](img/0151410972a5c5ea0aab26416e44b1da_34_0.png)

## 多媒体与网站可访问性

网站可访问性意味着有残障的人士能够理解、交互和浏览网页。

网站可访问性的主要原则之一是设计网站和 Web 软件，使其能够灵活满足不同用户的状况、偏好和环境。





随着网络成为信息共享日益重要的资源，越来越多的用户现在上网查阅资料：健康、商业、政府、休闲、媒体以及许多其他资源。希望利用网络的人必须能够平等地访问这些资源并享有平等的机会：

![](img/0151410972a5c5ea0aab26416e44b1da_35_0.png)

在构建网站时另一个重要考虑因素是，在某些国家，网络无障碍具有法律义务。除了对所有网站用户的重要考虑之外，在构建和维护网站时考虑无障碍也可能有法律原因。

## 一个多媒体网站可以是无障碍的吗？

多媒体丰富的网站也可以是无障碍网站。然而，你选择为网站实施的无障碍选项的程度是你需要为项目做出的决定。决策依据通常包括目标受众、国家法律以及项目时间和预算等因素。

网络上使用着多种无障碍标准，它们提供了无障碍指南和检查清单以供衡量。示例包括：

### Web 内容无障碍指南（WCAG）

Web 内容无障碍指南（WCAG）已成为网络无障碍的世界标准，并分为三个“合规”类别：

- 1. **A（也称为 Level 1）**
- 网站必须符合 Level 1 中的所有检查点，否则一个或多个用户群体将无法访问信息或功能。
- 2. **AA（也称为 Level 2）**
- 网站应符合 Level 2 中的所有检查点，否则一个或多个群体在访问信息或功能时将遇到重大障碍。
- 3. **AAA（也称为 Level 3）**
- 网站可以符合 Level 3 中的所有检查点，否则一个或多个群体在访问文档中的信息时会发现有些困难。

### Section 508

Section 508 是由美国联邦政府创建的标准。它包含 16 个用于测试网站的检查点。前 11 个检查点与 WCAG A（Level 1）指南完全等效。后五个检查点部分基于 WCAG 指南，但更为详尽。有关此标准的更多信息，请在网上搜索 Section 508。

网站无障碍功能最好在网站项目开始时考虑，然后在开发过程中解决。采取这种方法比事后解决这个问题要容易得多。修复无效的网站代码并创建 `title` 或 `alt` 标签可能需要花费数小时的工作时间，并且通常会超过为使内容无障碍而预留的任何预算。

你可以实施一些简单的流程来使网站更具无障碍性。当然，力求达到更高的无障碍水平当然需要更大的时间投入。

- 3. http://en.wikipedia.org/wiki/Web_accessibility: Wikipedia's page on accessibility guidelines.
- 4. http://www.section508.gov/: Section 508 requires that Federal agencies' electronic and information technology is accessible to people with disabilities.

## Web 浏览器对多媒体的支持

Web 最初是一个仅限文本的媒介，浏览器只能以单一字体、大小和颜色显示文本。

随着互联网的爆炸式增长，网站浏览器现在可以查看图像、播放音频和视频。万维网的进一步发展是“流媒体”的演进，它允许在文件完全下载之前开始媒体播放。

有许多网络浏览器可用。其中一些适用于特定用途，而另一些则以轻量级或最快的浏览器为荣。

大多数浏览器具有内置的多媒体功能，这是对网站多媒体增长的认可。其他浏览器需要插件来播放多媒体内容。**插件**是扩展浏览器功能的程序，使其与额外的媒体类型兼容。当浏览器遇到多媒体文件时，它会将数据交给插件以播放或显示该文件。

你可能听说过插件“Adobe Flash Player”，它使你能够通过网络浏览器播放 Adobe 电影，或“Windows Media Player”浏览器插件，它播放以 Microsoft 格式保存的流音频、视频和演示文稿。如果没有将这些插件安装到网络浏览器中，网站页面上的此媒体内容将无法有效查看：

这凸显了以多种格式提供多媒体材料的 importance，或者如果这不是一个选项，则为网站用户提供有关正在尝试加载的媒体的明确说明，以及在可能的情况下，指导他们去哪里找到插件以播放它。

## 摘要

在本章中，我们研究了术语“多媒体”以及该描述包含众多类型的媒体内容。

我们强调了 Joomla! CMS 平台并认识到其内置的多媒体功能，以及如何使用 Joomla! 扩展增强这些功能。正如扩展增强了默认的 Joomla! 安装，它们将在各章中得到充分的介绍。

在本章中指出网站无障碍很重要，因为在网站开发开始时考虑和集成网络无障碍要比事后尝试解决无障碍问题容易得多。有关无障碍及其与特定媒体的关系的更多信息将在各章中介绍。

## 管理你的 Joomla! 媒体

在使用 Joomla! 网站中的多媒体时，最重要的任务之一是能够上传和管理你网络服务器上的文件。一旦上传，此内容可能不需要进一步关注，或者你可能需要能够进行持续管理和文件修订。

在本章中，我们将研究使用内置的 Joomla! 媒体管理器，以及管理网站媒体和文件的替代方法。本章包括：

- Joomla! 媒体管理器概述
- 使用媒体管理器上传媒体
- 视图
- 组织你的内容
- 上传你的内容
- 使用媒体管理器管理媒体
- 管理文件和媒体的替代方法
- 第三方文件管理扩展

## Joomla! 媒体管理器概述

媒体管理器是一个有用的文件管理工具，包含在 Joomla! CMS 中。

媒体管理器工具位于你的管理区域内，可以通过控制面板上的“快速链接”图标访问，或通过菜单：站点 | 媒体管理器访问：

![](img/0151410972a5c5ea0aab26416e44b1da_42_0.png)

媒体管理器的主要目的之一是让网站管理员以及具有权限的前端用户能够轻松上传和管理其 Joomla! 站点的文件。在你没有 FTP（文件传输协议）访问网络服务器的情况下，媒体管理器可能是你唯一可用的工具，可以将新图像、视频、文档和其他文件添加到你的网站。

## 使用媒体管理器上传媒体

在初始网站开发期间，通常需要定期将新文件上传到你的 Joomla! 站点。根据网站的内容，随着你进入维护阶段，此过程可能会减少，或对于经常更新的站点而言仍然是必需的。

## 媒体管理器设置

与所有软件应用程序一样，媒体管理器工具包含一组预定义设置。在首次使用媒体管理器之前，建议你查看这些设置，因为它们提供了自定义网站媒体处理的能力。根据你的文件需求，现在调整媒体配置设置可能会在以后为你节省时间和精力。




## 媒体设置配置

如果您决定为媒体目录使用其他文件夹名称，保留服务器上当前的 `/images` 目录非常重要，因为其他组件通常会使用该目录。

- **图片文件夹路径**
  这通常是将 Joomla! 内容文章中的图片放置的路径。
  
  默认情况下，它被设置为 `<joomlaroot>/images/stories`。您可以将其更改为所需的任何路径。如果您希望通过媒体管理器访问此文件夹，请将其设置为之前提到的“媒体文件夹”的子文件夹。例如，`<joomlaroot><mediafoldername>/<imagefoldername>`。

> 如果您决定为图片目录使用其他文件夹名称，保留服务器上当前的 `/images/stories` 目录非常重要，因为其他组件通常会使用该目录。

- **限制上传**
  此功能根据用户类型限制上传。默认设置为**是**，这意味着低于“管理员”状态的用户只能获得一个文件夹选项来上传文件。该文件夹即为主要“媒体文件夹”。如果将此选项设置为**否**，则用户也允许在主媒体文件夹内的子目录中上传文件。

- **检查 MIME 类型**
  这是一个安全功能，使用 MIME Magic 或 Fileinfo 来验证上传的文件类型。通过检查 MIME 文件信息，有助于确保用户不会向网站上传恶意文件。有关**Fileinfo** 的更多信息可在 `http://www.php.net/manual/en/book.fileinfo.php` 找到。

- **合法的图像扩展名（文件类型）**
  这是您和其他用户允许上传到 Joomla! 网站的合法图像扩展名列表。默认列表包括 **bmp**、**gif**、**jpg** 和 **png** 文件。如果需要更多图像扩展名类型，请进行调整。

## 视图

打开媒体管理器时，其中一个初始可用选项是选择查看文件及其相关信息的方式。

两个选项是“缩略图视图”和“详细视图”。

1. **缩略图视图**：第一种（也是默认设置）称为缩略图视图。它提供文件的缩略图预览，并在每个文件下方提供删除按钮。缩略图视图非常适合快速浏览文件（尤其是图像），因为您可以实际看到列表以及面前的缩略图图标。然而，在处理大量文件时，此视图可能会显得繁琐。

![](img/0151410972a5c5ea0aab26416e44b1da_46_0.png)

2. **详细视图**：详细视图选项显示五列布局。其中包含有关每个文件的附加信息。详细视图中提供的信息如下：
   - 文件预览
   - 按字母顺序排列的文件名
   - 图像尺寸
   - 文件大小
   - 删除选项

![](img/0151410972a5c5ea0aab26416e44b1da_47_0.png)

在这两种视图类型之间切换只需简单点击一下。根据您所在的目录以及其中包含的文件数量，您可能会发现经常使用这两种视图。

## 组织内容

默认情况下，Joomla! 媒体管理器包含若干子目录，这些目录将显示在媒体管理器左侧的一列中。通过点击此树形结构中的文件夹，您可以在媒体管理器的右侧部分显示其内容。通过此方法或点击右侧部分的文件夹（这也将显示内容）可在媒体管理器中遍历子目录。

网站媒体内容的组织至关重要。正如 Joomla! 文章一样，文件正确的结构可以在日后需要轻松查找图片或媒体文件时，为您节省时间和避免挫败感。

当前版本媒体管理器中的一个限制是，文件不能移动到其他目录。这意味着在项目开始时正确构建文件结构非常重要，然后文件可以轻松上传到其目标位置。

## 创建新目录

在媒体管理器的顶部，您将看到一个水平栏，显示当前所在的目录位置。使用位于此栏右侧的“创建文件夹”功能，可以轻松地在媒体管理器中创建新目录。

![](img/0151410972a5c5ea0aab26416e44b1da_48_0.png)

要创建新目录，首先确保您处于希望创建新文件夹的目录中。填写“创建文件夹”按钮左侧的文本字段，然后单击此按钮以创建该名称的新目录。

页面应重新加载，显示您的新目录已就位。

> 已预先填充一个正斜杠，因此您只需输入目录名称，无需输入其他内容。

正如 Joomla! 文章一样，网站文件组织得越好，就越容易进行持续管理。

## 删除文件

与其他大多数事物一样，定期的大扫除有助于保持事物的有序和高效。

如果您不再使用某些文件，或已创建了它们的较新版本，则可以通过点击文件类型旁边的红色“X”图标轻松删除它们：

![](img/0151410972a5c5ea0aab26416e44b1da_48_1.png)

如果希望一次性删除多个文件，请勾选每个文件旁边的复选框，并点击媒体管理器右上角的“删除”按钮。

## 上传内容

根据全局配置中的“媒体设置”，您可以选择使用媒体管理器中的两种上传功能类型。

### 单文件上传

媒体管理器的默认上传工具是单文件上传工具，它位于媒体管理器的底部。此工具允许您在本地计算机上浏览单个文件类型，并将其上传到您选择的目录中的 Web 服务器。

要使用此方法上传文件，首先确保文件上传的目标位置正确。最简单的做法是使用左侧的**文件夹**目录菜单，并确保要上传文件的目标目录处于高亮状态。

![](img/0151410972a5c5ea0aab26416e44b1da_49_0.png)

要上传文件，请单击**选择文件**按钮。这将允许您在本地计算机上浏览要上传的文件。选择后，单击**开始上传**按钮将此文件上传到您 Web 服务器上选择的目录。文件上传完成后，媒体管理器将刷新，您应能在所选目录中看到新文件。

![](img/0151410972a5c5ea0aab26416e44b1da_49_1.png)

### 多文件上传

如果您在全局配置媒体设置中启用了“启用 Flash 上传器”选项，则应能够享受基于 Flash 的上传功能。

与单文件功能一样，多文件 Flash 上传器也位于媒体管理器的底部。

![](img/0151410972a5c5ea0aab26416e44b1da_50_0.png)

要上传多个文件，首先确保要上传的目录处于高亮状态。单击**浏览文件**按钮，并在本地计算机上浏览文件。重复此过程，直到列出要上传的文件列表。单击**开始上传**按钮上传文件。

该过程将显示每个文件上传的状态。

> 在编写本书时，Flash 11 的安全设置已破坏了 Joomla! 媒体管理器内置的 Flash 多文件上传工具。这可能会在未来的 Joomla! 版本中得到解决。然而，单文件上传功能是目前要使用的稳定上传工具。

## 使用媒体管理器管理媒体

如本章前面所述，网站内容的良好组织是非常重要的。

在项目开始时分析项目需求可以在未来为您节省数小时的工作。明确的目录结构以及这些文件夹的清晰描述不仅有助于您自己的管理，也有助于其他可能向网站服务器上传文件的管理员和用户。即使是默认的 `<joomlaroot>/images/stories` 结构有时也会变得具有挑战性；例如，我是把它放在 /images 文件夹中，还是 /images/stories 文件夹中？





![](img/0151410972a5c5ea0aab26416e44b1da_51_0.png)

内置的媒体管理器是 Joomla! 管理员的一个很棒的工具。事实上，如果你有一个由他人搭建的 Joomla! 网站，那么这可能是你上传新文件到 Web 服务器的唯一工具。

所以我们同意这是一个很酷的工具；它可以限制文件类型并允许你创建目录和上传文件，但不幸的是它很简单且有其局限性。其中一个缺点是上传后无法移动文件。

请记住，我们可以在主媒体目录下创建任意多的子目录。因此诀窍是在项目开始时分析你的网站用途，并相应地设置这些目录，这将为你节省时间和日后的挫败感。如果你的网站已经建立并且文件和链接指向这些文件，那么工作肯定会变得更耗时。不过对已建立的网站进行一次彻底清理是可能的，这需要创建新目录、将文件移入其中，并检查内容和链接以确保它们指向正确的目标。

## 管理文件和媒体的替代方法

根据你的情况，你可能拥有也可能不拥有对 Web 主机服务器的登录访问权限。如果没有，那么你上传文件的唯一选择之一就是使用 Joomla! 媒体管理器，或第三方 Joomla! 扩展的选项。

如果你拥有 Web 服务器访问权限，那么通过一种称为 FTP（文件传输协议）的方法，你将能够更好地控制文件管理。

FTP 是一种用于在计算机网络上交换和操作文件的标准网络协议。FTP 的常见用途是在本地计算机和 Web 服务器之间通过互联网传输文件，这正是我们想要的！

## FTP 客户端

有许多 FTP 客户端（程序）可用于传输文件。其中一些以图形方式显示文件和传输过程，使得从你的计算机到 Web 服务器的文件管理变得非常容易。

## FTP 程序

FTP 程序从免费应用程序到商业软件不等。你选择使用哪一个取决于个人喜好。这些程序大多数都提供拖放功能以及易于使用的目录导航工具：

![](img/0151410972a5c5ea0aab26416e44b1da_52_0.png)

所有主要操作系统都有出色的 FTP 程序。Windows 和 Mac 的一些流行选项是：

- Windows
  - FileZilla
  - CuteFTP
  - SmartFTP
  - WS_FTP
  - cURL
- Mac
  - FileZilla
  - Transmit
  - Cyberduck
  - CuteFTP
  - Fetch
  - FTP Command in Terminal

如果你使用流行的搜索引擎进行搜索，你将获得一份确定的 FTP 程序列表。建议下载这些程序的试用版，以查看哪一个适合你的操作系统和需求。

出于本主题的目的，我选择使用并从名为 “Transmit” 的 FTP 程序中截取屏幕截图。它是一款商业产品，但个人而言是我最喜欢的之一。对于使用 MAC 电脑的人来说，Transmit 被公认为是一款很棒的 FTP 工具。

## 使用 FTP 程序连接

几乎所有 FTP 程序都需要类似的信息才能连接到 Web 服务器。根据你使用的程序，这些字段的命名可能略有不同。

为了连接，我们需要以下信息，这些信息通常来自在你的 Web 主机控制面板上设置一个用户账户（具有 FTP 权限）：

- 1. **Server**
  这是你希望连接到的服务器地址。通常它看起来像 `ftp.yourserver.com`。根据域名设置，有时它也可以是你的域名，例如 `www.yourserver.com`。

## 2. User Name

这是你的 Web 服务器账户用户名。通常格式为 yourusername@yourdomain.com。

## 3. Password

这是你的 Web 服务器账户密码。

## 4. Initial Path

一些程序提供在登录后导航到初始目录的能力。如果你没有此选项，则不必担心。

## 5. Port

如果服务器需要在非默认端口上连接，一些程序允许你输入端口号。除非你知道自己在做什么，否则最好将其保留为默认设置。

![](img/0151410972a5c5ea0aab26416e44b1da_54_0.png)

如果连接遇到任何问题，最好联系你的 Web 主机管理员以检查你的登录访问详情。

通常有三种类型的 FTP 登录方法可用。这些是：

## 1. Anonymous FTP access

这是一种简单的连接方法，因为你不需要包含任何用户信息。有时软件公司提供更新，或企业和政府网站通过匿名连接方法提供可下载的表单和内容。

- 2. **Username**
  这是一种简化但受限的访问级别，你只需输入用户名。此类型的 FTP 登录通常用于学校和内联网环境。

- 3. **Username and Password**
  这是一种更受限的安全访问级别，用户需要用户名和密码才能访问服务器。此方法通常由 Web 主机公司使用，他们允许人们将文件上传到 Web 服务器。

如果你的 FTP 连接设置为用户名（或用户名和密码），那么你应该在屏幕上看到一个对话框。这将提示你输入安全访问详情：

![](img/0151410972a5c5ea0aab26416e44b1da_55_0.png)

连接后，你需要导航到你的 webroot 文件夹。根据你的 Web 服务器，此区域可能命名为 httpdocs、mainwebsite_html、public_html 或其他命名约定。你的 Joomla! 网站文件将位于此目录内。

当你找到主 http 文档区域并点击进入后，你将看到默认的 Joomla! 媒体文件夹名为 images。

![](img/0151410972a5c5ea0aab26416e44b1da_56_0.png)

当你成功导航到远程主媒体文件夹（或其中的任何子文件夹）后，你还需要本地导航到你希望上传的文件夹或文件。大多数 FTP 程序允许你创建新目录，并将文件从本地计算机拖放到 Web 服务器。

通过使用 FTP 程序，你可以更好地控制你的网站文件和内容。

FTP 程序允许你：

- Create files and directories
- Rename files and directories
- Move files and directories
- Delete files and directories
- Set file and directory permissions

> FTP 程序为你管理网站文件提供了最大的灵活性，但使用 FTP 程序时要小心。通过连接到 Web 服务器上的目录结构，你正在浏览核心 Joomla! 文件之间。

## 通过 Web 浏览器使用 FTP

除了浏览万维网之外，现代 Web 浏览器是包含 RSS 阅读器、使用电子邮件以及与 FTP 协作等额外功能的全面的互联网工具。

虽然你可以使用我们已介绍的专用 FTP 程序，但使用典型的 Web 浏览器（如 Firefox 或 Internet Explorer）连接到 FTP 服务器很容易。

出于本示例和屏幕截图的目的，我们将使用 Mozilla Firefox。

## 通过 Web 浏览器连接到 FTP

在某些情况下，组织的限制可能会限制 Web 浏览器 FTP 访问，但通常，如果你可以使用 FTP 程序连接到你的 Web 主机服务器，那么你应该能够通过 Web 浏览器使用 FTP 连接。

使用 Web 浏览器的好处是：嗯……它是免费的，而且你可能已经在面前使用了！

与使用 FTP 程序连接时一样，通常有三种类型的 FTP 登录方法可用。这些是：

- 1. **Anonymous FTP access**
  通过 Web 浏览器进行 FTP 的身份验证方法与使用基于软件的 FTP 程序时相同。
- 2. **Username**
  这是一种简化但受限的访问级别，你只需输入用户名。此类型的 FTP 登录通常用于学校和内联网环境。
- 3. **Username and Password**
  这是一种更受限的安全访问级别，用户需要用户名和密码才能访问服务器。此方法通常由 Web 主机公司使用，他们允许人们将文件上传到 Web 服务器。

通常有两种方法可通过 Web 浏览器连接到 FTP。这些是：

- 1. **Enter an FTP address into the address bar**
  对于大多数提供 FTP 支持的 Web 浏览器，可以输入语法：`ftp.yourserver.com` 或有时 `ftp.yourdomainname.com`，然后按 *Enter*。





如果你的 FTP 连接设置为 **Username**（或 **Username** 和 **Password**），那么屏幕上应该会出现一个对话框。这将提示你输入安全访问详细信息：

![](img/0151410972a5c5ea0aab26416e44b1da_58_0.png)

如果连接时遇到任何问题，最好联系你的网站托管管理员以检查登录访问详细信息。

连接后，你需要导航到你的 webroot 文件夹。根据你的 web 服务器，该区域可能命名为 httpdocs、mainwebsite_html、public_html 或其他命名约定。你的 Joomla! 网站文件将位于此目录内。

## 2. 直接输入地址栏

通过网页浏览器连接 FTP 的第二种方法是直接在地址栏中输入用户信息。虽然这可以为你节省一步，但我经常发现用户使用这种方法容易出错，导致花费的时间比之前连接方法更长。

不同浏览器可能有略微不同的语法，但使用 Mozilla Firefox 时，你可以直接在地址栏中输入类似以下内容：

```
http://username:password@yourserver.com
```

例如，如果我的用户名是 john，密码是 12345，那么它看起来像：ftp://john:12345@yourserver.com。

用户名通常可以包含域名。如果是这种情况，语法看起来像：ftp://john@yourserver.com:12345@yourserver.com。

## 上传和下载

一旦通过网页浏览器（通过上述任一方法）连接到 FTP，文件的上传和下载通常是相当直观的。

![](img/0151410972a5c5ea0aab26416e44b1da_59_0.png)

要上传新文件，请确保你位于正确的远程上传目录中。浏览到你计算机上的文件，然后直接将其拖放到你的网页浏览器中。根据文件大小，这可能需要一些时间来上传，然后过程应该会完成。你可以以类似的方式下载文件，或者可能需要双击文件以开始下载它。

## 网页浏览器插件

许多流行的网页浏览器为你提供扩展插件，可以增强使用浏览器连接方法时的 FTP 功能。

Firefox 网页浏览器有一个有用的扩展名为 FireFTP。该插件提供左右视图，可轻松通过网页浏览器使用 FTP 管理和移动文件。FireFTP 就像在你的计算机上拥有一个独立的 FTP 程序一样。它免费使用，并在 Windows、Mac 和 Linux 上运行。有关 FireFTP 的更多信息，可访问 http://fireftp.mozdev.org/。

以下屏幕截图显示了 FireFTP 插件：

![](img/0151410972a5c5ea0aab26416e44b1da_60_0.png)

必须提到的是，如果你定期进行文件上传，那么使用专业的 FTP 浏览器插件或 FTP 程序将为你提供额外的功能并使生活更轻松。通常你可以保存收藏的连接，这样你就可以用几次点击登录到你的 web 服务器。

现在有许多免费的 FTP 程序可用，提供文件移动、重命名选项，在某些情况下还提供服务器到服务器的文件移动，如果你有 FTP 服务器访问权限并定期传输文件，就没有理由不使用其中一个。

## SFTP

安全 FTP（SFTP）是一种使用 SSH 传输文件的程序。SFTP 对传输的命令和数据进行加密，在你的计算机和远程服务器之间创建安全连接。你可以通过 SFTP 客户端或命令行使用 SFTP。大多数 FTP 客户端确实提供 SFTP 支持，但并非所有 web 主机都支持 SFTP，因此在使用此传输方法之前最好先向他们确认。

## 第三方文件管理扩展

对于大多数用户来说，Joomla! 媒体管理器或使用 FTP 足以在你的计算机和 Joomla! 网站之间管理媒体。http://extensions.joomla.org 区域中也有许多文件管理扩展可用。其中一些提供了增强的文件上传功能，而其他一些则提供了位于 Joomla! 管理中的 FTP 程序的能力。

下面列出了两个流行的扩展，它们为管理员在 Joomla! 管理中提供文件管理功能。

> 还有许多优秀的文档管理和下载扩展可提供前端文档共享和管理。如前所述，有关更多文件管理扩展，请访问 http://extensions.joomla.org 查看可用的扩展。

## eXtplorer

eXtplorer 是一个第三方文件管理应用程序，可在 Joomla! 1.0.x 和 1.5 版本下原生运行。eXtplorer 可轻松作为 Joomla! 组件安装，但要求在 Joomla! 1.5 中以传统模式运行。

你可以使用 eXtplorer 通过 FTP 或直接文件访问来访问和修改服务器上的文件和目录。无需启动 FTP 客户端，使用此类扩展可以从 Joomla! 管理的后端执行强大的文件管理操作：

![](img/0151410972a5c5ea0aab26416e44b1da_61_0.png)

## Joomla! Flash Uploader

Joomla! Flash Uploader 类似于内置媒体管理器 Flash 上传器，但该扩展包含扩展功能。它似乎不受 Joomla! 媒体管理器 Flash 上传工具因 Flash 安全设置而目前收到的相同问题的影响。该扩展允许你轻松地从桌面上传多个文件到 Joomla! web 目录。该扩展的一个不错功能是能够为你的网站用户创建“上传配置文件”。这意味着如果你愿意，可以为不同的网站用户提供不同的上传目录——对于需要额外控制用户上传的大型网站来说这是一个方便的功能。

![](img/0151410972a5c5ea0aab26416e44b1da_62_0.png)

## 总结

你网站媒体的良好组织和结构与 Joomla! 文章一样重要。

在你的项目处于开发阶段时，你应该审查和自定义项目的媒体设置，以及定义可用的目录结构，这将使文件管理成为一项简化的任务。

使用内置的 Joomla! 媒体管理器是管理员（以及具有权限的前端用户）创建目录结构并轻松地从本地计算机上传文件到 web 服务器的绝佳方式。上传后，文件然后可以在 Joomla! 文章和组件及模块中使用。

对于开发人员（以及对服务器有额外访问权限的网站用户），使用 FTP 是管理文件和网站内容的有力方式，使你能够移动和操作文件，并访问 Joomla! 文件结构的其他区域。

# 3

# 你 Joomla! 网站中的文本、字符和字体

有些人可能会问，为什么在一本关于多媒体的书中要包含一章关于文本和字体的内容？

答案很简单：文本（及其样式）是最突出的媒体类型之一，一直以来并仍在使用于网页中。尽管它是如此重要的媒体元素，web 开发人员通常对这个主题关注甚少。

在本章中，我们将重点介绍字体、字符和文本在你的 Joomla! 网站中的使用，以及我们如何不仅通过使用另一种字体来改变网站设计，还通过这样做来影响网站用户的整体体验。

主题包括：
- 文本、字符和字体的概述
- 字体及其对网站用户的影响
- 哪种字体最好使用？
- Joomla! 模板和层叠样式表（CSS）
- 在网页中使用自定义字体的替代方法
- Joomla! 文本和排版扩展
- 浏览器对字体的支持和可访问性

## 文本、字符和字体的概述

文本及其使用是大多数网站页面最大的贡献之一。文本和字符用于向读者传达和传递信息。当显示大块文本时，重要的是对其进行样式设计以在视觉上取悦读者，同时易于阅读。

**纯文本** 是一个术语，用来描述不包含字体和样式等属性的文本。它几乎可以被所有计算机程序读取。从一个方面来说，HTML 代码、CSS 和 XML 文件都是纯文本，但它们也包含与字符一起包含的结构标签。

一个 **字符** 是一个符号。它可以是字母、数字或标点符号，以及一系列其他信息符号。字符代表信息单元。

一个 **字体**（也称 fount）传统上定义为“特定字体的一种尺寸和样式的完整字符集”。

在 Web 早期的日子里，屏幕上只有纯文本。此文本只能应用一种样式，使大多数网页在文本格式上看起来非常相似，例如 Tim Berners-Lee 于 1991 年设计的早期网页的此屏幕截图：

![](img/0151410972a5c5ea0aab26416e44b1da_64_0.png)

通常，计算机设备上用于屏幕查看的字体通常是流行离线印刷样式的转置，例如印刷书籍的样式。在早期网页中，可供使用的字体只有很少几种。使用的一个主要排版常量是名为“Times New Roman”（适用于 Windows）和“Times”（适用于 MAC）的字体。这些早期的计算机字体几乎总是衬线字体家族的成员（或衬线字体）。





衬线字体是对手写字符的模仿，其风格通常通过（书法）装饰得以丰富。衬线是添加到构成字符的线条末端的小挑。一种衬线字体示例（Times New Roman）如下；注意每个字符上的衬线（挑）：

![](img/0151410972a5c5ea0aab26416e44b1da_65_0.png)

五大主要字体家族是：

- 1. Serif
- 2. Sans-serif
- 3. Cursive
- 4. Fantasy
- 5. Monospace

在“Serif”旁边，另一种流行的字体家族是无衬线体或无衬线。无衬线字体是指在笔画末端不包含小字符装饰（衬线）的字体。该词源自法语单词“sans”，意为“没有”。

无衬线字体现在已成为在屏幕上查看的通用正文的标准字体。这部分是由于屏幕分辨率无法清晰显示（衬线）装饰，并且“无衬线家族”中已创建了许多专门用于屏幕显示的字体，例如以下名为“Helvetica”的示例。话虽如此，不要否定“衬线”家族的字体，因为这些也是流行的网络字体选项。

![](img/0151410972a5c5ea0aab26416e44b1da_65_1.png)

已针对这些字体家族中字体的可用性进行了大量测试，尤其是在计算机屏幕上显示时。由于字符必须适应屏幕上相对较小的像素网格，设计师认为无衬线字体更自然地适合数字化以供屏幕查看。

字体几个世纪以来一直是美化纯文本的流行方法。你可能会惊讶地知道，你电脑上安装的许多字体比你想象的要老。例如，维基百科引用称，衬线字体“Times New Roman”是由英国报纸“The Times”于1931年委托制作的，而“Helvetica”首次亮相于1957年。

## 字体及其对网站用户的影响

网页设计中最重要（也常被忽视）的领域之一是文本大小和字体选择的使用。许多缺乏经验的设计师未能意识到这些因素对访客首次在网站页面上的体验可能产生的影响。这反过来可能会对流量转化率和网站访客的跳出率产生很大影响。

当然，你在整个网站上使用的语言需要具有信息性、通顺且对访客有兴趣。然而，如果这种“精彩内容”被包裹在过大或过小的文本大小中，并且用户发现难以阅读，那么这些用户很可能会离开你的网站且不再返回。同样，如果你对文本大小和字体样式进行了思考和考虑，那么这可能有助于提高访客在网站页面上停留的时长以及可能的回访率。

![](img/0151410972a5c5ea0aab26416e44b1da_66_0.png)

视觉外观的偏好始终因人而异，而字体的可读性可能会决定其在特定受众上的适当使用。每种字体都会给用户带来某种形式的视觉情感，有些字体可以传达“请认真对待我”的情感，而另一些则在阅读文本时营造出“放松”的感觉。

如果你要去参加工作面试，你可能会花时间根据职位要求相应地打扮。呈现方式就是一切；同样适用于网页中使用的字体。字体吸引注意力；它们为所装饰的文本设定基调和情感。更改字体，你就可以从在网页中营造随意感转变为正式感，从杂乱无章到时尚，从老式到现代，等等。

字体是为服务文本而存在的。它们应易于阅读（尤其是在正文中使用时），并营造出一种低调的风格，不会压倒读者。许多字体网站将其字体归类为有用的组别。如果你想在选择网站字体时获得帮助，请查看 http://www.esperfonto.com/ 上的字体辅助工具。

## 哪种字体最好用？

当谈到字体和字形时，有很多选择。在热门搜索引擎中搜索“fonts”给我带来了 50,500,000 条结果，轻松超过 100,000 种不同的字体。

在你出去购买新硬盘来安装所有这些字体之前，需要注意的是，只有少数字体才被归类为易于在网络上阅读的字体。这些字体在大多数计算机操作系统上都可用，并且在屏幕上查看时看起来不错（并且易于阅读）。

如果你是设计师，那么你很可能已在电脑上安装了网络安全的主要字体组。这些字体在购买电脑时就已包含在内。通过访问一些流行的字体下载网站，你可以继续安装大量额外的字体。然而，如果最终用户在查看网站时没有在自己的电脑上安装这些有趣且定制的字体，那么在我们的网站上指定这些字体就没有意义了。如果没有相同的字体可用，他们就看不到我们所看到的内容。因此，在选择字体时，我们需要从“流行字体家族”列表中选择，以便覆盖大多数网站用户。

“网络上什么字体最好？”这个主题很有趣且冗长。请记住，对于正文文本，最好选择安全的做法，使用以下流行选择之一：

### Sans-serif fonts

该家族的字体如下：

#### Verdana

Verdana 由微软于1996年设计。其目的专门用于屏幕使用。没有衬线和宽间距使这种字体成为网络流行的选择。

![](img/0151410972a5c5ea0aab26416e44b1da_68_0.png)

#### Arial and Helvetica

Helvetica（适用于 MAC）和 Arial（适用于 MAC 和 Windows）是一种由瑞士设计师在50多年前创建的常见字体样式。尽管是两种不同的字体，但它们在属性上相似，Arial 由微软创建为该风格的版本。Arial 和 Helvetica 是使用最广泛的无衬线字体之一。

![](img/0151410972a5c5ea0aab26416e44b1da_68_1.png)

#### Trebuchet MS

Trebuchet MS 是为屏幕使用而开发的，并在 Windows 和 MAC 系统上很常见。它常用于大标题，有些人认为它对于正常大小的印刷来说没有 Verdana 可读性高。最好试用一下，看看你怎么想！

![](img/0151410972a5c5ea0aab26416e44b1da_68_2.png)

#### Lucida Sans family

Lucinda Sans Unicode（Windows）和 Lucinda Grande（MAC）并非为网络创建，但在屏幕上阅读时却出奇地清晰。Lucinda Grande 是 MAC 操作系统使用的字体选择。它们被认为是无衬线家族中最具书法特色的字体。

![](img/0151410972a5c5ea0aab26416e44b1da_68_3.png)

### Serif fonts

该家族的字体如下：

#### Times New Roman / Times

Times New Roman（Windows 和 MAC）和 Times（MAC）主要是约80年前基于离线印刷的字体。它不是屏幕的最佳字体，但在 CSS 文件中通常是很好的第二选择，因为它是如此普遍的计算机字体。

![](img/0151410972a5c5ea0aab26416e44b1da_69_0.png)

#### Georgia

Georgia 是微软委托的字体；同样，它是专门为屏幕制作的。尽管是衬线字体，Georgia 在屏幕上查看小文本和大文本时往往表现良好。设计师经常用这种字体替代 Times New Roman，因为它具有更佳的网络查看属性。

![](img/0151410972a5c5ea0aab26416e44b1da_69_1.png)

#### Palatino Linotype / Palatino

Palatino Linotype（Windows）和 Palatino（MAC）是在60多年前开发的，并非屏幕字体，但它们在网络使用中渲染得非常好。微软分发了这种字体的清晰仿制品；他们称之为“Book Antiqua”。

![](img/0151410972a5c5ea0aab26416e44b1da_69_2.png)

之前的字体是推荐用于主要正文文本的字体。然而，有方法可以在网页中显示花哨的自定义字体，让所有网站用户都能查看，而无需在系统上安装字体。




如果你正在寻找各种字体在屏幕上显示效果的示例，可以看看 http://www.typetester.org/ 上的 TYPETESTER 在线工具。这个字体工具允许你输入自定义文本，然后使用多种字体和大小选项进行格式化，并在屏幕上查看输出。有很多工具可以做到这一点，但 TYPETESTER 是其中的一个很好的例子：

![](img/0151410972a5c5ea0aab26416e44b1da_70_0.png)

## Joomla! 模板与层叠样式表

在我们开始调整 Joomla! CSS 字体之前，有必要简要介绍一下 Joomla! 模板。

### 什么是 Joomla! 模板？

Joomla! 模板是一组控制网站内容呈现的文件。模板以“正式文件结构”打包，这意味着它们需要满足某些要求才能与 Joomla! 配合使用。每个模板文件在生成最终网站设计、结构以及网站页面的交互功能方面都起着至关重要的作用。

Joomla! 模板通常以压缩包格式打包，并使用 Joomla! 内置的上传/解压功能进行安装。它们形状和大小各异，其设计方式使得在安装并激活后，它们会按照指定的结构和设计来显示你的 Joomla! 内容：

![](img/0151410972a5c5ea0aab26416e44b1da_71_0.png)

使用模板有许多好处，包括：

- 新模板可以立即应用于你的网站内容。这种灵活性意味着你可以轻松地为网站提供完全不同的设计。
- 通过使用多个模板，可以为你的网站应用多种样式。如果你希望为 CMS 的不同区域提供不同的设计布局，那么这是一个重要功能。
- 模板包含所有 CSS 和网站样式。这些都是自包含的，并且可以包含覆盖。通过进入“模板区域”，你可以轻松地对当前模板进行调整，并将自定义更改包含到此文件夹中，从而保护“Joomla! 核心框架”。

Joomla! 模板的一个重要样式元素是 CSS。这些 .css 文件包含重要的布局和样式信息，它们有助于你的 Joomla! 网站设计。

## 模板 CSS

层叠样式表是网页开发人员最好的朋友！它们使我们能够保持网站样式完全一致，并提供比纯 HTML 更多的布局和设计选项。

在 CSS 出现之前，HTML 文档中几乎所有的样式都包含在 HTML 代码中。颜色、布局、样式、字体等都必须具体列在代码中，而且经常在整个 HTML 文档中重复出现：

```
<html>
<body>
<p><font size="2" face="Verdana">
This is a paragraph of text.
</font></p>
<p><font size="5" face="Times" color="red">
This is another paragraph.
</font></p>
</body>
</html>
```

这里使用一种较旧的 HTML 方法对文本进行样式设置，其中样式位于 HTML 代码内部。

通过使用 CSS 方法，我们可以将大部分样式信息放入单独的文档（.css 文件）中。这些信息可以在 HTML 中轻松引用，从而提高效率并大大简化 HTML 代码。

HTML 代码如下：

```
<html>
<body>
<p>This is a paragraph of text.</p>
</body>
</html>
```

CSS 代码如下：

```
p {
    font-family:Helvetica ,Arial,sans-serif;
    font-size: 1em;
    color: #000000;
    text-align: left;
    width: 100%;
}
```

前面的 HTML 和 CSS 代码展示了 p 标签的样式。

CSS 信息可以嵌入到 HTML 文档中，但对于 Joomla! 模板，它通常包含在单独的文档中。Joomla! 模板通常包含多个 CSS 文件来分离设计元素。使用 CSS 时，可能会为手持设备使用单独的样式表以适应较小的屏幕分辨率，也可能仅针对 Internet Explorer 6 浏览器使用自定义样式表，等等。你的 Joomla! 模板可以包含代码，根据用户用来查看网站的设备类型，告诉浏览器加载哪个样式表。

![](img/0151410972a5c5ea0aab26416e44b1da_73_0.png)

Joomla! 模板 CSS 文件通常位于以下目录：`/templates/yourtemplate/css/template.css`。如果你使用的是预构建的 Joomla! 模板，你可能会在 CSS 目录中看到多个 CSS 文件。需要注意的是，这些 CSS 文件的名称可能因 Joomla! 模板而异，但 `template.css` 文件是主 CSS 文件的标准命名约定。某些所见即所得编辑器依赖此命名约定才能正常工作。

我们可以用一整本书来介绍 CSS 的复杂性。然而，在本章中，我们只重点介绍 font 属性及其可用值。

## CSS 字体属性

让我们来看看使用 CSS 时可以用来操作文本的众多属性：

### 字体族

字体族属性是元素字体名称的优先级列表。指定多个字体是一种好的做法，以防用户的计算机系统未安装你所指定的字体。在 CSS 中，浏览器会使用它识别的第一种字体；因此我们总是按优先级顺序列出字体族：

```
body {
    font-family: Arial,Courier,sans-serif;
    line-height: 1.3em;
    margin: 0px;
    font-size: 12px;
    color: #333;
}
```

在前面的示例中，字体 Arial 将优先于 Courier 使用。

有两种类型的 font-family 值：

- 1. **字体族名称**
  字体名称。例如：Helvetica、Courier、Arial。
- 2. **通用字体族**
  五种通用字体类别之一的名称。例如，serif、sans-serif 等。

在 Joomla! 模板 CSS 文件中，字体族通常应用于 body 标签。这指定了用于网站正文文本的主要字体。如果你需要对其他 Joomla! 文本元素进行额外样式设置，那么我们可以使用进一步的 CSS 来相应定义。

```
body {
    font-family: Arial,Helvetica,sans-serif;
}
```

在前面的示例中，浏览器会查找 Arial 字体，然后是 Helvetica，如果由于某种未知原因用户的计算机设备上没有安装这些字体，那么浏览器将替换为可用的 sans-serif 字体。

当使用名称中包含多个单词的字体族时，务必用双引号将这些字体名称括起来。虽然某些浏览器可以在没有引号的情况下读取这些字体，但如果单词之间的空格被考虑或忽略，则可能会出现一些问题。

```
body {
    font-family: Arial,"Times New Roman",sans-serif;
}
```

注意 Times New Roman 使用了引号。

### 字体大小

CSS 中的 font-size 属性设置文本的大小。你可以在 CSS 中使用的字体大小方法可以是绝对固定大小，也可以是相对大小。

- 1. **绝对大小：**
  - 将文本设置为特定大小
  - 如果已知固定输出或容器，绝对大小很有用。使用此方法，你可以让文本完全按照你的意愿放置在该定义区域内。
  - 在某些浏览器中不允许用户调整文本大小。因此，它被认为不利于可访问性。

- 2. **相对大小：**
  - 允许用户在所有浏览器中更改文本大小。
  - 字体大小基于浏览器中的默认字体设置。用户可以根据需要更改此默认值。
  - 设置相对于周围元素的文本大小。

关于使用哪种方法的争论，就像哪种字体最适合屏幕一样持续不断。使用 pt 或 px 的绝对方法已被归类为不符合 W3C 可访问性标准，但它允许你在受限容器中设置文本样式，并具有更多设计控制权。如果文本的布局很重要，那么你可能需要这种样式方法来按需对齐文本。

如果文本可以随意在页面上溢出，并且用户可以根据需要调整大小，那么相对大小是管理字体大小的首选且可访问的方法。相对大小可以使用 em 度量或 % 百分比来完成。

一般规则如下：

```
1em = 16px = 12pt = 100%
```

### 认识单位

在 CSS 中，有四种不同的单位可用于测量文本在网页浏览器中显示的大小。

### 使用 em 设置文本大小

为了适应所有浏览器并在你的网站上保持可访问性标准，大多数设计师都在尝试采用 em 相对文本大小方法。这是 W3C 标准首选的文本调整大小方法。

em 大小的工作原理是：1em 等于所讨论元素的当前字体大小。如果你没有在 CSS 的其他区域设置字体大小，那么它将采用用户的默认浏览器字体大小。浏览器中的默认文本大小是 16px，通常使用 Arial 或 Times 等字体。这意味着 1em 的默认大小 = 16px。如果你要为 body 设置字体大小，比如 18px，那么 1em = 18px。




对于一般正文文本，你可能会看到类似以下 Joomla! CSS 文件的内容：

```
body { 
font-family: Helvetica, Arial, sans-serif; 
line-height: 1.3em; 
margin: 0px 0px 0px 0px; 
font-size: 12px; 
color: #333;
}
```

一旦正文主字号已指定（如上一个示例所示），你就可以使用 em 单位将 CSS 中的其他文本元素设置为相对于该字号的值：

```
h2, .contentheading { 
padding: 0; 
font-family: Arial, Helvetica, sans-serif; 
font-size: 1.4em; 
font-weight: normal; 
vertical-align: bottom; 
color: #333; 
text-align: left; 
width: 100%;
}
```

## 使用百分比设置文本大小

使用百分比来调整大小是可行的，正如你预期百分比的工作方式一样。与 em 一样，使用百分比作为样式方法是相对的。

如果父元素的字号为 24px，而子元素的百分比为 50%，那么它将以 12px 的字号显示。

与 em 一样，使用百分比调整大小的一个流行技术是在 CSS 的 body 上设置父字号，然后使用百分比来设置其他所有样式。一切都相对于那个已设置的父字号：

```
.tool-text { 
font-size: 95%; 
margin: 0;
}
```

## 使用像素设置文本大小

像素是用于屏幕媒体的固定大小单位。一个像素等于屏幕上的一一个点，当你使用像素来设置文本大小时，你是在告诉浏览器以确切的像素高度渲染文本。使用像素定义文本大小提供了对文本大小和布局的极佳控制。然而，使用像素不允许为视力障碍用户向上缩放，也不允许为移动设备向下缩放。

使用像素设置文本大小将允许 Firefox 和 Safari 等浏览器保留一定的文本控制，但在通过 Internet Explorer 6 查看时，用户将无法调整这种绝对样式化文本的大小。总而言之，使用 px 设置字号是一种准确的方法，但请务必考虑 IE 6 缺乏可访问性的问题。

## 使用点设置文本大小

点（pt）通常与打印媒体相关联。点与像素类似，它们是固定大小的，无法按比例缩放。就像像素对屏幕分辨率准确一样，点对纸张也是准确的。

```
72pts = one inch
```

在 Joomla! 模板 CSS 中使用 pt 样式并不是好的实践，但在创建单独的 print.css 样式表时使用点是很好的。

打印 CSS 的示例可能如下所示：

```
body {
  color : #000000;
  background : #ffffff;
  font-family : "Times New Roman", Times, serif;
  font-size : 12pt;
}
```

## 字体样式

“font style” 属性设置你指定字体的样式。

其值包括：

- **Normal**
  浏览器将显示正常字体样式
- **Italic**
  浏览器将显示斜体字体
- **Oblique**
  浏览器将显示斜体（oblique）字体

在 CSS 中书写时，font-style 属性可能如下所示：

```
body {
  font-style: italic;
}
```

## 字体粗细

font-weight 属性设置字体的宽度。例如，文本应显示得有多粗或多细。

其值包括：

- **Normal**
  定义正常字符
- **Bold**
  定义粗体字符
- **Bolder**
  定义更粗的字符
- **Lighter**
  定义更细的字符

你还可以使用 100-900 的值。

这种方法定义了从粗到细的字符，并提供更进一步的控制。作为参考，值 400 等同于 normal，值 700 等同于 bold。

```
p
{
  font-weight: bold;
}
```

使用 CSS，你可以定义文档中的每个字体属性，或者将所有值定义为一个简写属性。这简单地称为 font。

CSS 中的“font” 属性可以包含“line-height”值。此元素允许你设置文本行之间的间距：

```
p
{
  font: italic bold 900 12px/14px arial;
}
```

## 在网页中使用自定义字体的替代方法

好的，我们确实喜欢某种特定的字体，并希望将其用于菜单或标题，但我们要知道读者可能没有在计算机系统上安装该字体。我们可以做什么？

最流行的方法之一是使用图像替代文本。通过使用图像方法，你可以在图形程序（在你选择的字体中）设计你的字体，然后将其放入网站页面。这意味着用户最终会以你选择的字体查看文字。对于 Joomla! 模板，你经常会看到这种方法用于菜单和模块图标等。

图像替换技术也可以使用名为 FaceLift Image Replacement (FLIR) 的 PHP / JavaScript 库来实现，发音为 "fleer"。如果用户在机器上未找到字体，该技术会自动将网页上的任何文本替换为自动生成的图片。FLIR 是开源的，不需要在服务器上使用其他工具，也不需要在用户浏览器中加载任何插件。FLIR 需要在服务器上安装 PHP 和 GD 图像库。

以下是 FLIR 在主页上运行效果的示例：

```
http://facelift.mawhorter.net.
```

![](img/0151410972a5c5ea0aab26416e44b1da_79_0.png)

Scalable Inman Flash Replacement (sIFR) 是另一种方法，用于将短段的纯文本替换为你选择的字体渲染结果。sIFR 方法使用了 Flash、JavaScript 和 CSS 的组合。sIFR 是一种日益受欢迎的新兴技术。sIFR 也是开源的，并且需要在阅读浏览器中安装 Flash 播放器插件。

实时将特定字体下载到用户浏览器的举措正在开发中，并且浏览器支持正在逐步引入，我相信在不久的将来将会可用。甚至现在有许多 sIFR 扩展可用于 Joomla！ 这些将在以下部分中提及。

## Joomla! 文本和排版扩展

目前可用的大部分专业 Joomla! 模板都直接内置了“排版”和文本/字体功能。预配置类样式已包含在 CSS 样式表中，允许最终用户在指尖拥有大量的文本样式选项。

从管理端的“你想使用什么字体？”参数，到包含的 `.class` 样式，你的模板可以通过小的改动呈现出惊艳的新文本外观：

![](img/0151410972a5c5ea0aab26416e44b1da_80_0.png)

目前大多数现代专业 Joomla! 模板都包含预排版的选项，为你的网站页面提供风格和趣味：

![](img/0151410972a5c5ea0aab26416e44b1da_80_1.png)

本着真正的开源精神，有一些可用的 Joomla! 扩展。这些扩展旨在通过各种有趣的文本功能增强 Joomla! 模板。

## 前端字号调整器 (M)

此模块允许访问者增加或减少网站页面上的字号。通过 JavaScript 和 Cookie 的使用，访问者的字体设置会被记住。

此模块包含六种不同的按钮和样式，并且你可以为该模块分配一个类，以便你可以自定义这些以适合你的网站：

![](img/0151410972a5c5ea0aab26416e44b1da_81_0.png)

要利用此扩展，请访问 http://extensions.joomla.org 并搜索 "Font Size"。

## JsIFR3 (P)

在你的 Joomla! 1.5 网站中使用任何字体。此插件将 Scalable Inman Flash Replacement (sIFR) 集成到 Joomla! 1.5 中。

此插件有一个商业版本，但这个非商业版本提供了 14 个插件参数设置，以帮助优化你的项目结果：

![](img/0151410972a5c5ea0aab26416e44b1da_81_1.png)

要利用此扩展，请访问 http://extensions.joomla.org 并搜索 "jsIFR3"。

## capDropper (P)

capDropper 是一个简单的插件，它使文章的首字母显示为装饰性首字母（drop capital）样式。

可以调整样式以适合你的模板，并且可以在文章级别手动激活或停用该插件。

![](img/0151410972a5c5ea0aab26416e44b1da_82_0.png)

要利用此扩展，请访问 `http://extensions.joomla.org` 并搜索 "capDropper"。

有许多扩展允许轻松实现“文本调整大小”和“装饰性首字母”功能，以及 sIFR 扩展的商业版本。请访问 `http://extensions.joomla.org/` 以查看最新的文本和排版扩展，它们可以为你的 Joomla! 网站增添色彩。

## 字体相关的浏览器支持和可访问性

正如本章前面提到的，用于网站页面的主要正文文本应使用特定数量的字体。选择这少数几种字体的原因是因为它们易于阅读，并且在各种用户计算机系统上可用，因此提供了触达尽可能多受众的最佳机会。

通常符合上述分类的字体被称为“Web 安全”字体。Web 安全字体是一组安装在所有版本的 Windows、MAC 和 Linux 计算机上并通用的字体。作为网站设计师，选择一种 Web 安全字体可以增加你的内容以所选格式显示的可能性。

以下是公认的 Web 安全字体列表。这些在 Windows、MAC 和 Linux 操作系统上可用：





## Web 安全字体列表

- Arial, Helvetica - 无衬线字体
- Comic Sans MS - 手写字体
- Courier New, Courier 6 - 等宽字体
- Georgia - 衬线字体
- Impact, Charcoal 6 - 无衬线字体
- Monaco 5 - 等宽字体
- Lucida Sans Unicode, Lucida Grande - 无衬线字体
- Palatino Linotype, Book Antiqua 3, Palatino 6 - 衬线字体
- Tahoma, Geneva - 无衬线字体
- Times New Roman, Times - 衬线字体
- Trebuchet MS 1, Helvetica - 无衬线字体
- Verdana, Geneva - 无衬线字体
- Symbol, Σγμμβολ 2
- Webdings, ♈♉♊♋♌♍
- MS Sans Serif 4, Geneva - 无衬线字体

微软于 1996 年启动了“Web 核心字体”计划。该字体集包含现有字体、微软委托开发的字体，以及部分从 Apple 获得许可的字体。“Web 核心字体”被归类为“Web 安全”字体类别。

“Web 核心字体”的设计目标是：

- 支持国际市场
- 在屏幕上清晰可读
- 提供多种字体风格

“Web 核心字体”计划包含以下字体。请注意，这些字体的大小均相同。

- Andale Mono
- Arial
- Comic Sans MS
- Courier New
- Georgia
- Impact
- Times New Roman
- Trebuchet MS
- Verdana
- ♈♉♊♋♌♍ (Webdings)

如前文所述，使用 CSS 中定义的通用字体族是最安全的选择。还记得我们有哪些通用字体族吗？

- 衬线字体
- 无衬线字体
- 手写体
- 幻想体
- 等宽字体

在字体族 CSS 定义的最后，应始终指定这些字体族之一，如下所示：

```
body {
    font-family: Arial,Helvetica,sans-serif;
}
```

上面的示例中，如果用户的计算机系统中没有 Arial 或 Helvetica 字体，则会加载 sans-serif 通用字体族。

随着 Web 浏览器的不断发展和字体类型的采用，实时下载和显示特定字体将突破当前“Web 安全”字体的限制。就目前而言，最好选择保守方案（即选择 Web 安全字体），并确保您的网站访问者（无论其使用何种浏览器和操作系统）都能轻松阅读并获取您重要的文本信息。

## 摘要

在 Joomla! 网页中，文本通常是页面上最突出的媒体元素。然而，如果使用了带有内置字体样式的预建模板，通常很少考虑字体类型是否适合目标受众。

本章重点介绍了多种可供使用的字体，以及哪些字体被归类为“Web 安全”字体，从而为用户提供最佳机会，让他们以您设计的样式看到网站文本。

Joomla! 模板 CSS 文件是一种重要且强大的样式机制，它使我们能够定义和设置网站文本及其他元素的样式。在 CSS 中，我们可以使用固定文本样式方法，或相对文本样式方法，允许用户根据需要调整网页文本的大小。许多 Joomla! 模板都包含增强的“排版”功能，包括字体选项和自定义 CSS 样式。此外，在 http://extensions.joomla.org/ 上还有一些优秀的第三方字体扩展。其中一些扩展包含图像替换功能，可为您的 Joomla! 网站带来令人兴奋的新字体技术。

Web 安全字体是用于主体文本的安全选择。Web 安全字体种类繁多，可给网站用户带来不同的情感反应。通过为网站选择正确的字体，您可以唤起多种情感，例如让网页文本呈现严肃、幽默或高雅的感觉。您还可以通过提供适当的字体大小和用户可调整文本大小的控件，来增加访问者的“网站停留时间”。请确保在 CSS 定义中指定字体族，这样即使您指定的字体不可用，浏览器也能选择通用字体族来显示您的网站文本。

# 4. 添加和管理图片内容

除了文本内容，图片是网页中使用最多的多媒体资源之一。它们可用于向读者提供信息和教育，辅助导航，并增强设计和用户的整体视觉体验。

无论您需要在 Joomla! 网站中创建多媒体图片画廊，还是只是将图片添加到文章和模块中，本章都将提供帮助，助您在 Joomla! 网站中有效利用图片内容。我们将讨论：

- 为什么在网站中使用图片
- 图片格式以及使用哪种格式
- 在文章和模块中包含图片
- 创建图片画廊和幻灯片
- 模板图片
- Joomla! 图片扩展
- 图片的浏览器支持和可访问性

## 图片及其在网站中的作用

研究表明，图片通过视觉方式展示概念并提供视觉记忆线索，从而增强学习效果。自人类进化以来，我们一直使用图片来描述事物、讲述故事和记录历史。

我曾遇到一些情况，当我和对方之间存在语言障碍时，我们不得不借助铅笔和餐巾纸来进行有效沟通。

图片可以传递信息，而这些信息可能需要数千字才能描述清楚。这种非文本交流方式以及图片所引发的视觉情感，使其成为补充或替代网页文本的理想媒介：

![](img/0151410972a5c5ea0aab26416e44b1da_88_0.png)

上面的图片可以很容易地用千言万语来描述，但这些文字会显得枯燥，并占用网页宝贵空间。而图片只占我们描述所需空间的一小部分，并且本身就能讲述一个故事。

图片不仅能描述故事、时间瞬间或幻想场景；它们还能提供视觉刺激，传达风格或主题，并为观看者营造氛围。许多 Joomla! 模板现在包含大量图形图像，以产生有趣的设计和效果，例如来自 http://www.yootheme.com 的这个商业示例：

![](img/0151410972a5c5ea0aab26416e44b1da_88_1.png)

如今许多模板在图形上采用圆角边缘或褪色效果，有些还在二维计算机屏幕上创建 3D 视觉效果。这些效果是通过使用阴影和图像分层技术实现的。其他模板则使用图片来创建仅靠图片才能实现的趣味导航效果。

除了图片提供的设计、导航和品牌效应之外，在我们的内容和模块中，我们还使用图片向网站访问者进行广告和沟通。作为 Web 开发人员，一个重要的技巧是要确保我们的图片在要求访问者将其加载到浏览器之前尽可能优化。这能确保良好的用户体验，因为如果网站加载缓慢或图片缺失，这会让访问者非常反感。

## 图片格式以及使用哪种格式

图片通常占 Joomla! 网站的 50%，有时甚至更多。这些加载到浏览器中的图片可以是模板设计的一部分，也可以是加载到模块和文章中的网站图片。为这些图片内容选择适当的格式有助于优化网页加载时间，这是构建多媒体丰富网站时最重要的考虑因素之一：

![](img/0151410972a5c5ea0aab26416e44b1da_89_0.png)

我们可以使用一些简单的规则来选择适当的图片格式。最终文件最重要的标准是大小。上图是使用 Mac 电脑上的 Adobe Fireworks 导出的。外部图像编辑器，如 GNU 图像处理程序（GIMP），是操作和优化 Web 图片的优秀开源解决方案。可通过访问 http://www.gimp.org 下载 GIMP。

由于每张图片都需要用户浏览器加载，您在网页中使用的图片质量和数量很容易影响页面加载时间。这反过来会导致最终用户沮丧（尤其是在拨号上网连接上），并造成网站流量流失。

## 数字图像

在进一步探讨文件大小和文件类型之前，有必要提一下数字图像。因为所有网页图片都存储在计算机设备上或通过其查看，所以它们采用数字格式（使用 0 和 1 存储的电子文件）。数字图像由微小的元素——像素组成。**像素**是所有数字图像的基础，它们是沿图像长宽方向排列的矩阵中相邻的小方块。

像素非常小，在计算机屏幕上查看图像时，很难直接看到它们：

![](img/0151410972a5c5ea0aab26416e44b1da_90_0.png)

每个像素是包含一种纯色的元素，当所有这些微小色点组合在一起时，就构成了最终图像。通常，单位面积内像素越多，图像质量越高，但在电子设备上用人眼观看时，存在一个临界点，超过该点后您实际上无法看到额外像素给图像带来的更多细节：

![](img/0151410972a5c5ea0aab26416e44b1da_90_1.png)

数字图像的物理尺寸以像素为单位，所有数字图像都有所谓的图像分辨率（图像的高度和宽度像素尺寸）。您可以以多种格式和压缩方式保存数字图像，以优化文件质量和大小。图像编辑程序（如 Adobe Fireworks）专门设计了 Web 优化和导出选项。





## 有损和无损数据压缩

数据压缩是我们数字世界的重要组成部分。文件压缩很有用，因为它有助于减少昂贵资源的消耗，例如传输带宽和计算机硬盘空间。

我们在网络上使用的几乎所有数字图像都已被调整大小或在某种程度上被压缩。在流行的软件编辑程序中编辑或保存文件时，会提供许多选项来帮助优化数字图像。

**有损和无损**压缩是描述文件压缩类型的两个术语，以及在对该文件进行压缩然后解压缩后，文件是否保留了所有原始数据。数字网络图像的首选格式（例如 JPEG 和 GIF）都属于以下压缩类型之一：

- 1. **有损**
有损数据压缩是指压缩尝试消除冗余或不必要的文件信息。一般来说，一旦文件被压缩，压缩过程中丢失的信息就无法再次恢复。因此，这是一种可降解的压缩类型，随着文件每次被压缩和解压缩，文件的质量会因代际损失而受损。
JPEG 和 MP3 文件是使用有损压缩的格式的良好示例。

- 2. **无损**
无损文件压缩使用的数据压缩保留了原始文件中压缩前每一个比特的数据。
无损压缩通常在需要文件被压缩然后再次解压缩后获得原始数据的完全相同表示时使用。例如，它用于流行的 ZIP 文件格式。PNG 和 GIF 等图像格式属于无损压缩类型。

现在，不必再深入了解这些压缩类型的更多细节，但以下图像格式都属于这两种压缩类别。通过选择正确的格式来保存和向网站用户展示，可以优化你的网站图像。

## 图像格式

现在有数百种图形图像格式，可以理解的是，网站上出现了一些奇怪的图像格式，主要是由缺乏经验的网站设计师上传的，他们只是上传了通过电子邮件收到的任何文件。

刚刚提到“数百”这个词，为了缓解紧张情绪，实际上只有四种主要格式被广泛使用并被认可为适合网络使用。这些格式被认为适合使用，是由于其格式和压缩特性（使最终文件大小和显示质量适合）。这些格式成为标准的另一个重要原因是能够在大多数计算机系统上打开或查看这些文件类型的可访问性。随着互联网将全球数百万用户联系在一起，必须发展出易于查看或保存的通用格式。

以下格式属于有损和无损数据压缩格式：

- **图形交换格式 (GIF)** 发音为“giff”。
- **联合图像专家组 (JPEG)** 发音为“jay-peg”
- **便携式网络图形 (PNG)** 发音为“ping”
- **可缩放矢量图形 (SVG)**

将数字图像从一种格式转换为另一种格式通常是在常见的图形编辑包中执行“另存为”。在此保存或导出过程中，你通常有选择质量、文件大小和像素分辨率的选项。

### GIF

GIF 图像格式属于称为“索引颜色”格式的家族。这意味着它们使用固定数量的颜色来构成图像。文件大小通常与位深度或所用颜色数量相关。这些通常是较小的数字，例如 256 或“8 位”颜色，并且通常仅包含两种颜色。为了获得最佳文件大小，你通常希望在仍能保留可接受图像的同时选择尽可能低的位深度。

由于其有限的颜色特性，GIF 图像通常非常适合少量纯色平坦颜色，并且通常是徽标、按钮和图形文本的首选格式：

![](img/0151410972a5c5ea0aab26416e44b1da_93_0.png)

GIF 图像对于包含大量颜色的图像，例如照片来说是一种低效的格式。转换为 GIF 格式的照片通常质量较差，看起来斑驳，或由于颜色范围有限而显示锯齿状边缘：

![](img/0151410972a5c5ea0aab26416e44b1da_93_1.png)

### JPEG

JPEG 文件格式旨在高效压缩逼真的真彩色或灰度图像，例如照片。这种压缩类型是为设计而生，是保存和压缩你的照片图像的完美选择。

JPEG 格式通过近似像素块并在压缩过程中保留对人眼最明显的关键细节来发挥其魔力。JPEG 图像可以高度压缩，使你能够以较小的文件大小保存高质量的图片。然而，JPEG 格式属于有损压缩类型，这意味着每次你读取、修改和保存 JPEG 图像时，你都会丢失一些信息。这种退化在图像中包含文本时非常明显，例如徽标。即使在保存 JPEG 图像时将质量滑块设置为“100”，数据丢失仍然会发生；因此它是无损文件压缩的绝佳示例。

JPEG 是网站上多色或调色图像的优秀图像格式，但如果你希望频繁编辑或更改图像，它并不是保存图像的好格式：

![](img/0151410972a5c5ea0aab26416e44b1da_94_0.png)

### PNG

PNG 是一种相对较新的文件格式，是作为 GIF 格式的开源继任者而创建的。作为无损压缩类型的一员，PNG 图像最适合可能需要编辑并再次保存的图像。与 GIF 格式一样，PNG 文件格式在图像包含大面积均匀颜色区域时表现出色。PNG 格式也非常适合使用透明度，这对于可能叠加在背景颜色上方的徽标或图形来说很重要：

![](img/0151410972a5c5ea0aab26416e44b1da_94_1.png)

Mozilla Firefox、Internet Explorer 7 和大多数主要浏览器现在都提供 PNG 支持。许多购买的 Joomla! 模板将包含用于徽标、圆角模块标题以及按钮/图标的 PNG 图像。

### SVG

SVG 是由 **万维网联盟 (W3C)** 创建的一种新的矢量文件格式。它是为了在网络上显示矢量图形而创建的标准格式。

SVG 是一种基于文本的图形格式，图像及其行为在 XML 文件格式中定义，这意味着它们可以轻松搜索、脚本化并压缩：

```
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
"http://www.projectamplify.com/DTD/svg01.dtd">
<svg width="5cm" height="5cm" viewBox="0 0 700 600"
 xmlns="http://www.projectamplify.com/1000/svg" version="1.1">
<desc>Example script01 - invoke an ECMAScript function from an onclick event</desc>
<!-- ECMAScript to change the radius with each click -->
<script type="text/ecmascript"><![CDATA[
function circle.click(evt) {
 var circle = evt.target;
 var currentRadius = circle.getAttribute("r");
 if (currentRadius == 100)
 circle.setAttribute("r", currentRadius*2);
 else
 circle.setAttribute("r", currentRadius*0.5);
}
]]>
```

由于其基于文本的文件格式，SVG 图像可以通过多种方式与用户交互，并且图像可以与脚本一起使用以在网页和移动设备上触发事件。

除 Internet Explorer 外的所有主要浏览器都提供 SVG 文件支持。然而，Internet Explorer 可用的浏览器插件。此时，关于 SVG 的扩展并不多，但有一个有用的 Joomla! 插件称为 XHTML Headers。该插件有助于查看浏览器是否支持基于 XHTML 的技术。有关更多信息，请访问 http://extensions.joomla.org 并搜索 XHTML Headers。





## 在内容文章和模块中插入图片

在接下来的章节中，我们将介绍如何在 Joomla! 内容文章和模块中插入图片。本节假设您对 Joomla! 内容文章的工作原理有基本的了解。

## 使用 Joomla! 文章图片按钮添加图片

通过使用图片按钮，可以轻松地将图片添加到新的和现有的 Joomla! 文章（和模块）中。图片按钮是一个扩展编辑器插件，在 Joomla! 新安装时默认配置为开启。当您处于编辑文章界面时，编辑器按钮会显示在内容编辑器窗口下方：

![](img/0151410972a5c5ea0aab26416e44b1da_96_0.png)

需要注意的是，此插入图片的方法与使用 WYSIWYG 编辑器工具栏上的插入图片按钮的方法不同。

要将图片插入到您的 Joomla! 文章中，您需要处于文章编辑模式。通过以下方式之一打开您的文章进行编辑：

- 1. 查看控制面板时，点击"新建"文章图标。
- 2. 点击顶部菜单中的"文章"选项，然后点击"文章管理器"菜单选项。选择您要编辑的文章，然后点击右上角工具栏中的"编辑"图标。
- 3. 如果您以适当的编辑权限登录到 Joomla! 网站的前端，选择您要编辑的文章，然后点击"编辑"工具栏图标。

接下来，我们需要选择图片的放置位置。这通过相对文章中的文本插入图片代码来实现。在编辑器中点击文本内容，您可以将鼠标光标定位到希望图片显示的位置：

![](img/0151410972a5c5ea0aab26416e44b1da_97_0.png)

编辑文章界面上方将加载一个插入图片界面，即 Joomla! 媒体管理器：

![](img/0151410972a5c5ea0aab26416e44b1da_97_1.png)

插入图片界面将显示 Joomla! 媒体管理器中的所有媒体。它包含以下功能：

- 1. **文件夹导航**

通过使用屏幕左上角的**目录**下拉菜单进行导航。您可以使用此菜单深入子目录，也可以直接点击插入图片主窗口中的可用文件夹。**向上**按钮可快速返回上一级目录：

![](img/0151410972a5c5ea0aab26416e44b1da_98_0.png)

> 您不能导航到高于主"媒体目录"的位置，该位置在全局配置设置中定义。

- 2. **选择图片**

只需点击您要使用的图片，**图片 URL** 位置将自动填充该图片在您服务器上的位置：

![](img/0151410972a5c5ea0aab26416e44b1da_98_1.png)

如果您希望使用来自其他 Web 服务器或网站的图片，请在图片 URL 字段中输入该图片位置的完整 URL：

![](img/0151410972a5c5ea0aab26416e44b1da_98_2.png)

### 3. **设置图片属性**

通过"插入图片"窗口插入图片时，我们需要考虑三个重要字段（以及**对齐**和**标题**框）：

![](img/0151410972a5c5ea0aab26416e44b1da_99_0.png)

"插入图片"窗口包含以下字段：

- **图片 URL** 字段保存图片的位置。这可以是相对于您网站域名的相对 URL（例如，`/images/image.jpg`），也可以是图片的绝对 URL 路径（例如，`http://www.mydomain.com/images/test.jpg`）。
- 图片**对齐**下拉选项允许您根据图片周围的文本设置图片的对齐方式。如果设置为左对齐，图片将浮动在文本块的左侧；如果设置为右对齐，图片将浮动在文本的右侧。如果此选项留空，则不会设置图片对齐方式。
- 图片**描述**字段是 Joomla! 描述，相当于使用 HTML 时的 alt 标签。Alt 描述用于描述图片的"替代文本"。alt 属性告知查看者如果图片未加载，他们在页面上错过了什么。这对于使用屏幕阅读器的人，或选择在 Web 浏览器中关闭图片浏览网页的人特别有用。
- 图片**标题**字段指定关于图片的附加信息。当用户将鼠标悬停在图片上时，此信息会作为工具提示文本显示给用户。
- 图片**标题**复选框将使用图片标题，并将此描述作为图片标题输出。这通常显示在图片下方，可以使用 CSS 来调整其外观和位置。

插入图片界面还内置了一个实用的文件上传工具。因此，如果您的图片尚未通过 FTP 或 Joomla! 媒体管理器上传，您可以在编辑内容文章时选择上传。

上传之前，请确保您已选择要上传新图片的目标目录。要上传，请点击插入图片界面底部的**选择文件**上传按钮：

![](img/0151410972a5c5ea0aab26416e44b1da_100_0.png)

将出现一个界面，您需要在其中导航到需要上传的文件。选择文件后，点击**开始上传**按钮开始上传过程。

插入图片窗口顶部将出现一条消息，告知您上传是否成功。如果上传出现问题，请检查您的媒体目录和子目录是否设置了"读写"权限以允许上传。

文件上传后，您可以点击选择该文件，填写图片属性字段，然后点击插入图片窗口右上角的**插入**按钮。

## 使用编辑器图片按钮添加图片

您的 Joomla! 安装中有一个或两个可用的 WYSIWYG（所见即所得）编辑器。可以在用户管理器中基于用户更改编辑器类型。也可以在全局管理中设置站点范围的配置：

![](img/0151410972a5c5ea0aab26416e44b1da_100_1.png)

建议下载这些编辑器，看看哪一个（如果有的话）更适合您的编辑环境。几乎所有这些编辑器都提供插入图片到内容文章的功能，通常通过编辑器工具栏上的图标实现。

在本示例中，我们将使用 Joomla! 默认的 TinyMCE 编辑器：

![](img/0151410972a5c5ea0aab26416e44b1da_101_0.png)

首先选择您要编辑的文章，或创建新文章并进入编辑文章界面。

与使用 Joomla! 图片按钮技术时一样，我们需要将光标定位在文章中的图片显示区域。在编辑器工具栏中，您会看到许多用于执行编辑器功能的图标。查找带有小照片图像的图标：

![](img/0151410972a5c5ea0aab26416e44b1da_101_1.png)

点击此图标后，将打开一个弹出窗口，您可以在其中输入图片位置和属性。此窗口与前面提到的 Joomla! 图片按钮技术之间的主要区别在于，我们需要在此区域手动输入图片位置，并且无法使用此窗口上传图片：

![](img/0151410972a5c5ea0aab26416e44b1da_101_2.png)

可用的字段如下：

| 字段 | 描述 |
|---|---|
| 图片 URL | 图片位置的路径。 |
| 图片描述 | 图片的替代文本。 |
| 对齐 | 图片的对齐方式。 |
| 尺寸 | 图片的高度和宽度。 |
| 边框 | 图片的边框属性。 |
| 垂直间距 | 图片周围的垂直间距。 |
| 水平间距 | 图片周围的水平间距。 |

使用此方法将图片输入到内容文章意味着您必须手动输入网站图片的位置（默认为 /images/stories/）和其他属性到提供的字段中，但它使您无需使用 HTML 手动输入图片代码。

## 使用自定义 HTML 代码放置图片

根据您是否为 Joomla! 帐户选择了 WYSIWYG 编辑器，您可以选择使用编辑器编辑内容文章和模块，也可以选择使用 HTML 代码视图格式化您的网站内容。

WYSIWYG 编辑器通常会在您的网站内容中添加额外的字符，尤其是当内容编辑者直接从 Microsoft Word、电子邮件应用程序等文档中剪切并粘贴内容时。它们可以让没有 HTML 经验的用户拥有添加和更新网站内容的出色功能，但可能会用不需要或冗余的代码使代码变得杂乱：

![](img/0151410972a5c5ea0aab26416e44b1da_102_0.png)




使用HTML和CSS进行图像定位，可以让你根据自己的需求对齐和样式化网站内容。让我们看看如何使用纯HTML将图像添加到内容中。

就像使用所见即所得（WYSIWYG）编辑器插入图像一样，使用HTML时我们执行类似的过程，只不过这次是手动输入图像代码。在HTML中，所有图像都使用<img>标签定义。通过为该标签添加额外属性，可以让它执行相应功能。使用所见即所得编辑器时，我们需要确保图像代码放置在页面上我们希望其显示的位置：

![](img/0151410972a5c5ea0aab26416e44b1da_103_0.png)

在编辑窗口中，将鼠标光标定位到要输入图像的位置。在我们的示例中，我们将图像插入到第二段（中间段落）文本中（注意每个文本块周围都有一个<p>（段落）标签）。浏览器会在文档中图像标签出现的位置显示你的图像。如果我们将图像放在两个段落之间，浏览器会先显示第一段，然后是图像，再显示第二段。在我们的示例中，我们希望图像显示在第二段的开头，与文本内容对齐。

我们将其分解为一个示例，首先从在文档中插入基本的<img>标签开始：

```
<p>This is an interesting paragraph about inserting an image around text</p>
<p><img>This is an interesting paragraph about inserting an image around text </p>
<p>This is an interesting paragraph about inserting an image around text </p>
```

接下来，我们需要为图像提供位置。这可以通过使用src属性（src代表source，即来源）来完成。该属性的值是你希望显示的图像的来源位置：

```
<p>This is an interesting paragraph about inserting an image around text</p>
<p><img src="http://www.mydomain.com/image.jpg" />This is an interesting paragraph about inserting an image around text </p>
<p>This is an interesting paragraph about inserting an image around text </p>
```

请注意，我们现在已在图像标签的末尾添加了`/>`。这是正确结束我们的<img>标签并使代码符合XHTML规范所必需的。接下来，我们需要为图像提供alt（替代文本）描述。这对于告知查看者页面正在加载什么内容非常重要。请记住，一些网页用户会关闭图像并使用屏幕阅读器，因此，对于你添加到网站中的所有图像，alt标签都非常重要：

```
<p>This is an interesting paragraph about inserting an image around text</p>
<p><img src="http://www.mydomain.com/image.jpg" alt="My Page" />This is an interesting paragraph about inserting an image around text </p>
<p>This is an interesting paragraph about inserting an image around text </p>
```

如果我们现在点击内容文章上的“应用”（Apply），这段代码将被保存，如果图像位置正确，图像就会显示。上面的代码会先加载图像，然后文本会加载在下方：

![](img/0151410972a5c5ea0aab26416e44b1da_104_0.png)

如果你希望文本环绕图像，我们需要为图像设置一个align属性（对齐方式）。align属性的选项可以是left、right、middle、top和bottom，这会根据图像周围的任何文本块，使图像按照这些值进行对齐：

```
<p>This is an interesting paragraph about inserting an image around text</p>
<p><img src="http://www.mydomain.com/image.jpg" alt="My Page"
```

图像现在看起来像这样：

![](img/0151410972a5c5ea0aab26416e44b1da_106_0.png)

我想包含的最后一种属性是title属性（标题）。当查看者将鼠标光标悬停在图像上时，它会为图像提供一个工具提示标题。标题对于提供附加信息非常有用，并且确实会影响搜索引擎优化（SEO）。现在代码变成：

```
<p>This is an interesting paragraph about inserting an image around text</p>
<p><img src="http://www.mydomain.com/image.jpg" alt="My Page" align="left" height="60" width="100" hspace="5" title="A Nice Clock Image" />is an interesting paragraph about inserting an image around text </p>
<p>This is an interesting paragraph about inserting an image around text </p>
```

图像现在看起来像这样：

![](img/0151410972a5c5ea0aab26416e44b1da_106_1.png)

如果你想在文章和模块中插入大量图像，则需要对每个要加载的图像执行这些步骤。可以剪切粘贴之前的代码，每次只更改值，但这可能会很耗时。

一些图像属性，例如图像内边距（vspace和hspace），可以在CSS中设置，为所有内容图像创建站点范围的样式表类。这段CSS代码可能如下所示：

```
img { border: 0 none; margin: 3px }
```

前面的示例将为你网站的所有图像设置边框为0、外边距为3，因此你可能需要更详细地指定CSS属性，以适应你的模板。

## 创建图库和幻灯片

Joomla! 是一个内容管理系统（CMS），主要用于组织和管理网站内容。它内置了许多多媒体功能，但其主要焦点是提供两个作用：强大的CMS功能，以及一个设计良好的框架，允许向系统添加额外功能，从而创建强大的功能。

这些附加功能被称为**扩展**，旨在插入Joomla!核心框架并扩展其功能。关于Joomla!核心CMS，我们已经了解了如何将图像包含到内容项和模块中。然而，图库和幻灯片要求的不仅仅是简单的图像管理，因此需要借助扩展的功能来提供这些特性：

![](img/0151410972a5c5ea0aab26416e44b1da_107_0.png)

现在可用于Joomla!的多媒体扩展数量惊人。我们有能够创建复杂图库、流式传输视频以及编译点唱机式音频功能的扩展。

在仔细考虑本书本节内容的最佳方法后，我们得出了一个选项。那就是重点介绍一些最流行和最有用的图库和幻灯片扩展，并希望这些内容能够帮助你了解使用Joomla!扩展可以实现的高度复杂的图像管理能力。

## 图像管理扩展及其安装方法

在继续介绍第三方扩展功能之前，让我们快速了解一下图像扩展是如何添加到你的Joomla!站点的。

与Joomla!中的大多数操作一样，扩展的安装过程经过了开发团队的仔细考虑，并且非常容易执行。大多数Joomla!扩展都带有自己的安装说明，而这些通用指南将帮助你在站点上安装它们。

> 在安装任何内容之前，请确保复制文件集并备份站点数据库。

出于本示例的目的，我将安装我最喜欢的Joomla!多媒体扩展之一——由RocketTheme开发的RokBox扩展。RokBox是一个由MooTools驱动的幻灯片插件，可让你通过专业的弹出屏幕灯箱效果显示多种媒体格式：

![](img/0151410972a5c5ea0aab26416e44b1da_108_0.png)

安装步骤如下：

- 1. 下载所需的扩展，并将下载内容保存到计算机上。文件将采用.zip格式，也可能是.gzip或.tgz格式，这些格式可能需要在安装前解压：

![](img/0151410972a5c5ea0aab26416e44b1da_109_0.png)

- 2. 登录你的Joomla!管理员控制面板，导航至“扩展”菜单，然后进入“扩展管理器”。页面将默认显示“安装”界面：

![](img/0151410972a5c5ea0aab26416e44b1da_109_1.png)




3. 使用本页上的选择文件按钮浏览计算机上的扩展 .zip 文件，并在找到文件后，使用上传文件并安装按钮。Joomla! 安装管理器将执行所有工作，如果您的扩展已正确构建，安装程序将把所有文件和文档放置到其正确的目录中：

![](img/0151410972a5c5ea0aab26416e44b1da_110_0.png)

4. 安装成功消息将会显示，以告知您扩展已成功安装。

> 某些扩展的成功文本可能包含指向扩展配置页面的额外链接。

## 图片画廊扩展

以下是用于创建图片画廊的 Joomla! 扩展列表：

### 简单图片画廊

简单图片画廊扩展是一个插件：

![](img/0151410972a5c5ea0aab26416e44b1da_110_1.png)

该扩展的工作方式是您在文章中添加一个特殊标签——{gallery}myphotos {/gallery}——以正确标记 myphotos 文件夹中的所有照片进行显示：

![](img/0151410972a5c5ea0aab26416e44b1da_111_0.png)

然后扩展开始工作。创建的画廊包含诸如每个缩略图上的时尚阴影、用于显示原始图像的光箱弹出窗口，以及为每张图像自动生成的缩略图等功能。

通过利用特殊标签方法，您可以在内容文章和网站中显示多个画廊。这为全站范围的简单画廊功能提供了极大的灵活性。

### Expose Flash 画廊

Expose 是一个组件、模块画廊扩展，提供语言支持。它是一个基于 Flash 的工具，用于在网络上展示引人注目的图像幻灯片：

![](img/0151410972a5c5ea0aab26416e44b1da_111_1.png)

该扩展基于名为 "Expose" 的独立画廊软件，该软件已发展以提供对 Joomla! 网站的支持。该扩展提供众多功能，包括相册的嵌套、为相册分配缩略图、在线图片上传、离线画廊管理、水印添加以及播放幻灯片。

使用 Expose，您可以直接链接到图像、图像类别或完整的相册：

![](img/0151410972a5c5ea0aab26416e44b1da_112_0.png)

### Phoca Gallery with Slideshow

Phoca Gallery 是 Joomla! 的功能齐全的图片画廊和幻灯片扩展。它是一个带有插件和模块的组件。它也提供语言支持：

![](img/0151410972a5c5ea0aab26416e44b1da_112_1.png)

Phoca Gallery 允许您创建图片类别和子类别，并将图片放入其中。图片可以属于多个类别，这提供了在显示图片时更大的灵活性。

画廊会自动为图片生成缩略图，并提供诸如水印、用户访问、YouTube 支持以及众多画廊效果等附加选项，以多种方式显示您的图片画廊：

![](img/0151410972a5c5ea0aab26416e44b1da_113_0.png)

Phoca Gallery 随附一系列可用的插件和模块。可以将图片画廊和幻灯片同时呈现在您的文章和模块中。甚至还有一个搜索插件，可帮助 Joomla! 在您的图片类别中进行搜索。

### Ozio Gallery2

Ozio Gallery 是 Joomla! 组件，可用模块：

![](img/0151410972a5c5ea0aab26416e44b1da_113_1.png)

它可用于借助 Flash 功能和画廊皮肤开发具有有趣效果的复杂图片画廊。这两个功能有效地将此图片画廊组件转变为多种不同的图片画廊选项：

![](img/0151410972a5c5ea0aab26416e44b1da_114_0.png)

Ozio Gallery 为您提供直接链接到内容的灵活性，或通过使用 Ozio Gallery 模块将画廊发布到模块位置。该产品包含众多参数，应该能满足任何需要有趣的基于 Flash 的图片画廊功能并支持幻灯片的人的需求：

![](img/0151410972a5c5ea0aab26416e44b1da_115_0.png)

该扩展还提供从 **flickr** 账户显示照片的能力，并在您的 Joomla! 网站中显示您的 **flickr** 图像：

![](img/0151410972a5c5ea0aab26416e44b1da_115_1.png)

### JoomGallery

JoomGallery 是具有语言支持的 Joomla! 组件、插件和模块：

![](img/0151410972a5c5ea0aab26416e44b1da_116_0.png)

JoomGallery 提供许多功能，包括定义画廊的子类别、批量上传图片、用户对类别的访问权限、可配置的水印、幻灯片支持，等等：

![](img/0151410972a5c5ea0aab26416e44b1da_116_1.png)

## 图片幻灯片

用于创建幻灯片的 Joomla! 图片扩展列表如下：

### RokSlideshow

RokSlideshow 是一个 Joomla! 模块，允许您显示一系列图片和标题，并以可配置的方式在它们之间转换。

![](img/0151410972a5c5ea0aab26416e44b1da_117_0.png)

幻灯片由 Mootools 驱动，提供显示图片之间的淡入淡出、燃烧、擦拭和推送效果。总的来说，该模块能够实现 30 种不同的转换效果。

可以显示图片信息，图片可以包含超链接、标题和描述：

![](img/0151410972a5c5ea0aab26416e44b1da_118_0.png)

RokSlideshow 非常灵活且易于自定义。它是在 Joomla! 网站上显示图片幻灯片的绝佳方式，也是 Flash 图片画廊播放器的优秀替代品。

### 简单图片旋转器

简单图片旋转器模块提供简单的图片旋转机制。它是 Flash 图片旋转脚本的轻量级 JavaScript 替代方案，但在淡入淡出转换方面提供了类似的感觉。上一张图片将在几秒钟内淡出到下一张图片，依此类推。它做到了它所说的！

![](img/0151410972a5c5ea0aab26416e44b1da_119_0.png)

### Boncko Cooliris (PicLens) 画廊插件

如果您为图片画廊使用 Cooliris，那么 Boncko Cooliris 插件等扩展将非常适合您：

![](img/0151410972a5c5ea0aab26416e44b1da_119_1.png)

您可以通过使用特殊标签 {boncko}/img_dir/{/boncko} 在文章中轻松添加 Cooliris 图片画廊。此插件将从 /img_dir/ 目录插入所有图片和 Flash 视频文件到您的 Joomla! 内容中，使您的图片画廊看起来很棒。

这意味着您可以将图片画廊存储并组织在 Joomla! 站点外部，并吸入您想要的内容。Boncko Cooliris 插件甚至可以在您的图片画廊处于活动状态时播放音频曲目。

其中一些已被突出显示，因为它们是流行的 GPL 扩展，为创建图片画廊和幻灯片功能提供了极大的灵活性。

在 http://extensions.joomla.org 上有许多其他图片画廊扩展可用：

![](img/0151410972a5c5ea0aab26416e44b1da_120_0.png)

每周都有许多新扩展可用。其中一些是免费的 GPL，一些是商业扩展，还有一些需要俱乐部会员资格才能访问。无论它们是免费还是商业实体，我希望您能为您的图片画廊和幻灯片需求找到解决方案。

Joomla! 扩展通常提供在文章或模块中创建画廊和幻灯片的图片解决方案。您可能还有许多其他图片加载到网页中，这些通常包含在您的模板中。

## 添加和管理图片内容

### Joomla! 模板图片

Joomla! 模板控制您 Joomla! 网站的整体外观和布局。它们提供将通用元素组合在一起的框架，并提供模板布局与模块和组件交互的粘合剂。

Joomla! 为您的网站的前端和后端（管理）都使用模板。安装 Joomla! 时，它附带一组默认模板，如您所知，网络上可以找到数百个，如果不是数千个 Joomla! 模板。其中一些功能丰富且是商业模板，一些是不收费的并在各种许可下发布。

模板利用定义的<|message|>
结构，这允许所有模板元素正确地相互交互：

![](img/0151410972a5c5ea0aab26416e44b1da_121_0.png)

您可以制作自己的 Joomla! 模板，现在在 http://www.joomla.org 上有许多教程可用，还有一些专门的 Joomla! 模板开发书籍，例如 *Joomla 1.5 Template Design, Tessa Blakey Silver, Packt Publishing*。更多信息可在 http://www.packtpub.com 查看。

Joomla! 模板的一个重要作用是包含并加载站点样式表。这些有助于在站点中定位模板元素。现代 Joomla! 模板中这些元素中日益增多的部分是脚本、样式信息和图片。

让我们快速看一下可用于 Joomla! 的流行内置模板之一：

![](img/0151410972a5c5ea0aab26416e44b1da_121_1.png)




## 添加和管理图片内容

很容易看到许多图片共同构成了这个模板。我们有用于标志的图片，以及用于方框的圆角图片。以下是该模板在禁用图片的情况下加载在浏览器中的样子：

![](img/0151410972a5c5ea0aab26416e44b1da_122_0.png)

前面的示例使用的是相对简单的模板，但你可以看到模板图片如何协同作用，极大地增强了网站设计。Joomla! 模板的发展正在每月不断前进，许多公司每月都会推出极其专业的模板供购买。

这些商业模板通常包含预设的颜色主题和样式，提供了在几秒钟内更改网站配色方案和布局的能力。这种灵活性为管理员提供了强大的功能来为其网站创建新的外观，但也意味着一旦管理员决定了颜色选择等因素，一些模板可能包含大量从未使用过的脚本和图片。过去通往模板图片文件夹的简单路径，现在通常变成模板中包含图片文件的多个目录：

![](img/0151410972a5c5ea0aab26416e44b1da_122_1.png)

## 如何调整你的模板图片

Joomla! 模板的图片通常位于 `<你的Joomla主文件夹>/templates/<你的模板>/images` 目录中。如果你想调整模板中的某些图片，最简单的方法是在网站浏览器中查看模板，然后对图片进行必要的调整。

值得一提的是，许多Web开发人员都在本地开发他们的网站。通过在计算机上设置localhost服务器环境，可以轻松创建本地开发流程。MAMP for MAC和XAMPP for Windows等工具提供了运行本地服务器环境所需所有组件的简便软件安装。如果你还没有在本地工作，请花两分钟了解这些程序，因为它们不仅节省了你的时间（因为不需要连接实时Web服务器），而且在开发时也提供了信心，因为你永远不会在实时网站上操作：

![](img/0151410972a5c5ea0aab26416e44b1da_123_0.png)

> 有关localhost开发的更多信息，请参见第9章《Joomla!多媒体项目》。

查找和操作网站图片最简单的方法之一是使用浏览器元素检查器工具（如Firefox的Firebug或Safari的Web Inspector），或者直接在Web浏览器中右键点击图片，并选择**查看图像**。此选项可能因使用的浏览器而异，但大多数浏览器都提供了查找图像位置（URL）或在新浏览器标签页中打开图像的功能：

![](img/0151410972a5c5ea0aab26416e44b1da_124_0.png)

你的图像URL可能类似于：`<你的Joomla主文件夹>/templates/<你的模板>/images/logo.png`。这种图像URL方法让你知道路径，以便你可以找到所需的图像。

有数百种可用的图像处理程序，我们无法在这本书中详细介绍图像处理。然而，如果在调整图像时遵循以下规则，你就走在正确的轨道上：

- 复制原始文件。例如，如果你的文件名为`logo.png`，则将原始文件命名为`logo.png.orig`（或类似名称），以便你始终保留该原始文件的副本。或者，如果你将经常编辑你的网站，可以考虑使用SVN或Git等版本控制系统。
- 在编辑文件之前，记录文件名、大小和格式。
- 除非你是专业人士，否则始终保留与原始图像相同的图像格式。如果此图像在CSS或原始代码中被引用，你的新图像名称将不同，图像将不再显示。
- 保存图像时，请尽量确保文件大小与原始大小相同或尽可能接近，通过图像优化来实现。
- 如果你更改了图像尺寸，请检查是否有CSS变量或其他代码可能指定了物理文件尺寸。

对于大多数模板，你将调整的第一个文件之一是模板标志。更改标志的过程与前面的步骤完全相同。

我简要提到了Web检查工具的使用。其中最常见的工具之一是可用于Firefox Web浏览器的工具。这叫做Firebug。请访问 http://getfirebug.com/ 获取更多信息。Firebug允许你在任何Web页面上编辑、调试和检查CSS、HTML和JavaScript：

![](img/0151410972a5c5ea0aab26416e44b1da_125_0.png)

Firebug允许你在浏览器中实时检查Web元素并编辑这些元素。例如，如果你想将标志左右移动或上下移动一点，只需检查图像标志元素，然后调整定位该元素的相应CSS。这只是Firebug工具功能的简要示例，但当你需要解决错误等问题时，它的实用性就显现出来了。如果你热衷于增强和开发你的Joomla! 模板，我建议像Firebug这样的工具，它会真正帮助你！

值得注意的是，Internet Explorer和Safari也有它们自己的“元素检查器”版本。

## 浏览器支持和图像可访问性

几乎所有Web浏览器现在都提供图像支持。然而，在网站内容中使用图像时有一些需要考虑的因素。W3C可访问性指南强调了为非文本内容提供文本支持（或文本等效物）的重要性。

文本等效物的威力在于它们能够为残障群体以及使用各种技术查看网页内容的人提供“信息和支持”的能力。

使用图像时，使用文本替代是一种确保尽可能广泛的受众能够访问你的Web内容的可靠方法。文本替代可以被屏幕阅读器以视觉方式读取，文本大小可以调整以便查看，内容可以打印出来以盲文阅读等等，这就为提供替代文本内容提供了有力的论据。

其次，当我们使用图像时，重要的是指定alt和title标签信息，这样在浏览器中关闭图像的用户可以看到解释应该加载什么的“替代文本”。标题标签为用户提供清晰的工具提示，这两个标签的结合可以增强可访问性和搜索引擎优化：

![](img/0151410972a5c5ea0aab26416e44b1da_126_0.png)

直到最近，像Internet Explorer这样的浏览器不提供图像透明度支持，也不支持PNG文件扩展名。Web开发人员必须使用变通方法才能向使用这些浏览器查看其网站的用户显示PNG文件。现在Internet Explorer 7和8都将PNG支持作为标准。SVG是一种相对较新的格式，对于Internet Explorer浏览器而言，它也需要浏览器插件。然而，Web浏览器每天都在改进，其中对各种图像格式的支持也是如此。

## 总结

图像是重要的通信元素。它们用于讲述故事、展示事实、广告和销售，并以情感感受在视觉上吸引用户。我们的Joomla! 模板通常包含许多图像，这些图像创建出赏心悦目且复杂的设计。

图像在网站中的使用发展迅速，从扁平的单色图像发展到支持透明度和无损压缩的高保真高质量格式。没有它们，Web就不会一样了。宽带和连接速度的提高使得精美的图像库脚本得以发展，其中许多被整合到强大的Joomla! 图像扩展中。现在是在你的Joomla! 站点上显示图像和画廊的最佳时机。

调整现有模板图像或为网站创建新图像时要仔细考虑。图像文件大小和优化是对网站用户的重要考量。使用良好的优化工具和经验将有助于为你的网站提供高质量图像，同时减少页面加载时间。

由于用户能力和残障的多样性、连接速度的差异以及访问万维网的硬件种类繁多，考虑图像的正确使用并为那些无法或选择不查看图像的人提供替代方案是非常重要的。使用alt和`title`标签以及height和width属性都是将图像加载到你的网站页面时的最佳实践。




# 在您的 Joomla! 网站中使用音频

音频是一种强大且富有感染力的媒体元素。大多数人类使用音频作为主要的交流和听觉形式。它被定义为我们主要的感官之一。对于听力良好的人来说，我们的日常生活被成千上万的声音所包围，这些声音在有意识和无意识层面影响着我们。

互联网与音频有着非常成熟的关系。互联网一直是音频的完美传输媒介，这些元素帮助重新定义并迅速发展了各自的技术和行业。

在本章中，我们将探讨在您的 Joomla! 网站中使用音频。您将了解：

- 音频是什么
- 音频与互联网
- 音频格式
- 在 Joomla! 文章中嵌入和显示音频
- 为 Joomla! 创建音频播客
- 第三方音频扩展
- 浏览器支持和音频可访问性

### 什么是音频？

**音频**是“声音”，可被人类耳朵探测到。声音是空气的压力波，我们之所以能听到，是因为我们的耳朵设计为对这些波敏感。

对于人类来说，听觉通常在大约 20 Hz 到 20 kHz（20,000 Hz）的范围内被检测到。20 Hz 是我们能解读的最低音调（bassi est sound），20 kHz 是最高音调。众所周知，随着人类年龄增长，我们听觉感知的较高范围往往会退化。

互联网的一个重要元素是我们用来与之交互的工具。个人电脑现在已成为多媒体套件，提供强大的音频录制和播放功能，这些功能过去只有专业录音棚才能使用。由于电脑屏幕是视觉媒介，音频的使用对于“视力受损”的人来说是完美的传输媒介，网络音频已成为向某些残疾人士传递信息的一种解决方案：

![](img/0151410972a5c5ea0aab26416e44b1da_131_0.png)

上图取自 `http://www.w3.org`。它展示了网页中音频摘录的可访问性方法。

### 音频格式

就像视频一样，存在数百种音频格式用于录制和播放数字声音和音乐文件。其中许多依赖软件，需要特定程序才能播放该特定文件。当试图为最广泛的受众提供音频时，这些格式用处不大，因此我们将选择范围缩小到几种知名且支持良好的、适合网络使用的格式。

在查看要使用的文件类型之前，重要的是要提到音频是一种数据密集型媒介，其在压缩和存储方式上取得了惊人的发展。这种“数据压缩”使用音频编解码器执行。**编解码器**一词代表压缩/解压缩，即数据发生的处理过程。

### 音频压缩

音频压缩与视频压缩非常相似。两者都使用类似的技术来操纵数据，然后以适合用户播放的容器形式提供。

重要的是要指出，音频文件格式和音频编解码器（或压缩）是两件不同的事情。

### 音频编解码器

音频的压缩和存储方式是通过使用编解码器，这对最终文件大小有重大影响。专业的音频编解码器已问世，可提供质量和文件大小选项的平衡。由于网络托管成本可能变得昂贵以传输数据，编解码器有效地减少了网站上音频文件所需的存储空间和带宽。

编解码器使用算法去除被认为“不重要”的数据，因此文件大小会下降，因为这些额外信息不再存在。一个很好的音频压缩例子是去除超出我们听觉能力范围的数据。因此，如果我们有超出或低于我们听觉能力的非常高或低频数据，对我们来说就没用，所以我们就去掉它。

大多数音频编解码器属于有损压缩家族。这意味着压缩过程中移除的信息无法再恢复。有损算法提供更高的压缩率，主要用于消费类音频设备。它们也引起我们的兴趣，因为衡量最小文件大小（同时保留质量）是在处理网络音频时一个重要的考虑因素。

音频编解码器不一定与其最终文件名或类型相关。音频文件类型如 .wav（在 Windows 电脑上常用）可以使用不同的编解码器编码，例如 GSM、MP3 或 PCM 编解码器。音频文件类型使用不同的编解码器进行压缩。这通常是由于编解码器的许可和所有权问题。只是为了增加混乱，有些编解码器实际上与文件类型同名，例如 .mp3 文件格式始终使用“MPEG Layer-3”编解码器。

### 音频文件格式

音频文件格式的作用与我们的视频容器文件类似。音频文件包含数据文件，但通常还包含重要的头信息，如作者、优先级、注释和其他数据。

一些在网络环境中最常用的音频文件格式是：

| 格式 | 描述 |
|---|---|
| **MIDI** (.midi) | MIDI 是 **Musical Instrument Digital Interface** 的缩写。MIDI 文件非常小，并且得到计算机软件和浏览器的良好支持。然而，MIDI 文件通常是基于数据的音频而非录制的波形，因此使它们很小、易于分发，但不适合音乐文件。 |
| **AIFF** (.aif/.aiff) | 由 Apple 开发，AIFF 代表 **Audio Interchange File Format**。这种文件格式在网络音频中越来越受欢迎。AIFF 文件使用 QuickTime Player 播放，该播放器可在 Mac 和 Windows 上使用。AIFF 文件可以很好地压缩，同时保持出色的质量。 |
| **MP3** (.mp3) | **MPEG Layer-3** 格式或许是在网络上使用最广泛的音频文件格式之一。MP3 文件是 MPEG 文件的音频部分，可以将音频压缩到原始文件大小的大约十分之一。这种在保持良好质量的同时的压缩，导致了它们广泛流行。MP3 文件可以在许多设备上播放，使其成为跨平台能力的绝佳选择。 |
| **AAC** (.aac/.mp4/.3gp/.m4a) | 由 Apple 设计为 MP3 的继任者，**AAC** 通常在相似大小下实现更好的音频质量。AAC 最为人所知的用途是在 Apple iPod 上，它是 iTunes 商店和 Playstation 3 使用的格式。 |
| **WAV** (.wav) | 由 IBM 和 Microsoft 开发，WAV 代表 **Waveform**，是 Windows 电脑上的标准音频格式。WAV 不压缩音频数据，这使其不适合网络使用，但通常用于存储未压缩的音频。虽然 WAV 本身不压缩文件，但 WAV 文件可以使用其他编解码器如 MP3 进行压缩。 |
| **WMA** (.wma) | WMA 代表由 Microsoft 拥有的 **Windows Media Audio**。其质量和压缩与 MP3 文件相当，但在所有设备上的可访问性有限。 |
| **RealAudio** (.rm/.ram) | **RealAudio** 是 RealNetworks 拥有的格式。它曾被广泛使用，并帮助塑造了网络上流媒体音频的使用。RealPlayer 是免费的，但由于其专有格式，这种格式似乎有所衰落。 |

### 比特率

比特率指的是数据流（在此为压缩音频）传输的速度。这通常以每秒比特数（千比特每秒，Kbps）记录。

在一段时间内能够传输的音频数据越多，音频质量就越好。逻辑上，在录制音频时使用的比特率越高，可用的音频数据就越多，这会导致文件大小更大。

我们需要意识到这一点，因为在导出音频时，可用的选项之一是选择比特率：

![](img/0151410972a5c5ea0aab26416e44b1da_134_0.png)

目前网络音频使用的一个基准是立体声音频每秒 128 比特（**128 kbps**）。根据您的音频数据（可能是语音播客或音乐文件），您可以高于或低于此数值，如果您的音频为单声道格式，比特率可以再次降低。如果您的音乐文件非常重要，并且您认为大多数受众拥有良好的宽带连接，那么提高到 160 Kbps 可能是个选择。低于 128 会影响音频质量，但始终值得尝试，看看在保持所需质量的同时文件大小可以降低到多低。

由于有大量音频软件可用，并且现代家用电脑都包含音频系统，您可能已经拥有将音频转换为网站所需格式的工具。诀窍在于以不同格式和压缩率导出文件，然后相互比较，听取差异，并查看文件大小对比。





## 如何压缩和编码音频

现在有大量音频编辑和压缩工具可供使用，帮助你操作音频文件。

如果你有特定需求，快速在互联网上搜索即可找到流行的软件。如果你有一个音频文件，想将其优化用于网页，或转换为另一种音频格式，那么可以查看一个 Windows 的开源解决方案 LameXP，它可以从 `http://www.snapfiles.com/get/lamexp.html` 下载。

LameXP 是 Lame MP3、Ogg Vorbis 和 Nero ACC 编解码器的前端图形界面。它提供了一个易于使用的界面，可通过拖放文件将文件转换为 MP3、OGG、Wave 和 ACC 格式：

![](img/0151410972a5c5ea0aab26416e44b1da_135_0.png)

LameXP 功能强大但易于使用。下载并安装软件包后，打开它并按照以下简单步骤转换和优化你的音频：

- 1. 在设备上安装必要的编解码器。WAV 文件需要一个，某些其他文件也需要。信息框会引导你完成这些步骤。
- 2. 将文件拖入 LameXP 窗口，或点击 **Add File** 按钮添加音频文件。
- 3. 使用顶部选项卡选择你的 **Output Folder** 和其他文件设置。**Compression** 选项卡值得注意。在此选项卡中，你可以选择想要的编码类型，以及编码文件的 **Bitrate**。你可以使用 **Quality** 设置或选择一个 **Bitrate**，选项会根据你选择的单选按钮而变化。
- 4. 使用 **Encode Now!** 按钮开始编码过程：

![](img/0151410972a5c5ea0aab26416e44b1da_136_0.png)

将鼠标悬停在可用选项上会提供进一步的信息，帮助你做出选择。由于音频的特性，建议使用不同的设置编码文件，然后收听这些结果并决定哪种比特率/质量最适合你的音频。结果会根据文件中的音频类型而有所不同。诸如访谈之类的语音音频在压缩下的反应与包含音乐曲目的音频不同。通过几次测试，你会在音频质量和最终文件大小之间找到一个满意的折中。

最终，尽可能在保留所需质量的同时减小文件大小。

## 在 Joomla! 文章中嵌入和显示音频

Joomla! 的一项非常强大的功能是能够将自定义 HTML 包含到 Joomla! 文章中。这意味着我们可以完全自定义文章和模块中音频内容的输出，这正是手动将音频插入 Joomla! 网页所需的。

### 源文件

在向 Joomla! 网站投放音频内容之前，我们需要决定音频的类型和要选择的格式。

正如我们从之前的格式列表中看到的，.mp3 文件格式是一个不错的选择，因为它可以通过众多音频播放器和设备播放。

音频软件应用程序中的导出功能允许各种格式和压缩选项。创建多媒体文件时有一定的试错过程，我只能建议尝试多种设置，然后比较结果，以找到你想要的音频质量，同时获得可管理的大小。

### 编码

你的源素材可能以原始格式或与你决定使用的格式不同的格式存在，因此将其转换为流行的网页格式是一项重要步骤。为最终使用导出音频可能本身就是一本书。如果你遇到困难，那么应使用网页标准，即 MP3 格式。以下为网页音频 MP3 导出设置的指南：

| Quality | Settings |
|---|---|
| MP3 Voice Podcast | Mono, 64 Kbps, 44.100 kHz |
| MP3 Low Quality | Stereo, 96 Kbps, 44.100 kHz |
| MP3 Medium Quality | Stereo, 128 Kbps, 44.100 kHz |
| MP3 High Quality | Stereo, 160 Kbps, 44.100 kHz |
| MP3 Higher Quality | Stereo, 192 Kbps, 44.100 kHz |

你可以看到比特率的增加提供了每秒更多的数据和更好的音质。不幸的是，这会以更大的文件大小为代价。

### 上传

音频可以通过 FTP 上传到你的 Web 服务器，也可以使用内置的 Media Manager。

#### 通过 FTP

打开你的 FTP 程序并登录到你的 Web 服务器。在大多数图形化 FTP 程序中，你会有两个视图；一个用于本地文件，一个用于远程文件。

在你的远程文件集中，导航以查看 Joomla! 根目录结构。在此级别创建一个新文件夹。在此示例中，我将它命名为 audio。浏览你的本地计算机查找要上传的音频文件并将其发送到新的 audio 目录：

![](img/0151410972a5c5ea0aab26416e44b1da_138_0.png)

> 建议在新的 audio 目录中创建一个空白的 .html 文件并上传它。这将有助于阻止任何人直接查看新 audio 目录内的所有文件。如果你无法创建新的 .html 文件，请复制 images 目录中已有的 Joomla! .html 文件。

![](img/0151410972a5c5ea0aab26416e44b1da_138_1.png)

#### 通过 Media Manager

要通过 Media Manager 上传，请登录管理后台并访问 Content | Media Manager 链接。使用左侧的目录结构导航到现有目录，或使用 Create Folder 功能创建新的 audio 目录：

![](img/0151410972a5c5ea0aab26416e44b1da_138_2.png)

上图显示了 Media Manager 页面，左侧为文件夹结构，右侧为文件。

> Media Manager 位置默认为 Global Configuration 设置中的设置。通常这是 `/images/stories` 目录。如果你希望在与 `/images` 相同级别创建新的根目录（正如我们使用 FTP 方法所做的那样），那么你需要更改 Global Configuration 中的默认 Media Manager 路径，然后返回 Media Manager 区域创建新目录。你的音频文件夹不必位于 Joomla! 文件系统的根目录内，但如果你运行的是繁忙的网站，将图像、视频和音频分开存放有助于组织。

Global Configuration 还包含其他媒体设置，例如最大文件上传大小，在使用 Media Manager 上传音频时可能需要考虑。你 Web 主机服务器上可能存在“PHP max file upload”限制，因此如果 Media Manager 遇到困难，FTP / SFTP 将是一个不错的选择。

![](img/0151410972a5c5ea0aab26416e44b1da_139_0.png)

创建音频目录后，只需使用 Media Manager 的文件上传功能将文件上传到所选目录即可。

#### 编辑器

WYSIWYG 编辑器最令人沮丧的事情之一是它们会删除或调整你精心输入的代码。有些比其他更糟，但为了在 Joomla! 文章中输入任何非简单的 HTML 自定义代码，关闭你的 WYSIWYG 编辑器将符合你的利益。如果你不这样做，那么你的自定义音频代码很可能无法工作。

虽然我刚刚建议关闭编辑器，但我知道它们有时很有用，有些用户离不开它们。一个简单的技巧是创建一个新的超级管理员用户并将其命名为“noeditor”。然后将该用户的 **Editor** 选择设置为 **Editor – No Editor**。使用这种方法，你可以随时以“noeditor”用户登录。这可以避免经常为一个用户开关编辑器。

## 创建文章

新文章在“Article Manager”视图中创建：

- 点击控制面板上的 **Article Manager** 图标，或在 **Components | Articles** 菜单链接下。
- 点击该页面右上角标记为 **New** 的图标。这将打开 **Add Article** 页面。
- 新文章的几个要求之一是页面 **Title**，以及文章中的一些文本。填充这两项后，你就可以点击该页面右上角的 **Save** 或 **Apply** 按钮。

## 音频 HTML 代码

音频可以在 Joomla! 文章和模块中以两种方式呈现：

### 直接下载

你可以直接提供指向音频文件的超链接，允许用户下载文件，下载完成后可在浏览器窗口中通过插件播放，或通过 QuickTime 或 Windows Media Player 等音频播放器应用程序在用户机器上播放。

下载方式是音频内容在 Web 流媒体和嵌入内容变得普遍之前提供的传统方式。直接下载很有用，如果你希望允许用户下载并保存音频内容为文件。提供音频下载的代码是一个指向文件的简单超链接，看起来像这样：

```html
<a href="/audio/sound.mp3" title="Audio of Boats Download (1MB)" >
Listen to my Audio File (1MB)</a>
```

页面看起来像：

![](img/0151410972a5c5ea0aab26416e44b1da_141_0.png)





# 文档中各元素

JSON 计数：4

```json
{
  "direction": "horizontal",
  "contents": [
    {
      "type": "paragraph",
      "markdown": "...",
      "bbox": [0.1, 0.1, 0.5, 0.2]
    }
  ]
}
```

在 Joomla! 文章中嵌入音频所需的最小有效代码如下所示：

```
<object width="350" height="50">
<param name="src" value="audio/sound.mp3">
<param name="autoplay" value="true">
<param name="controller" value="true">
<param name="bgcolor" value="#F4F4F4">
<embed src="audio/sound.mp3" autostart="true" loop="false" width="350" height="50" controller="true" bgcolor="#F4F4F4">
</embed>
</object>
```

让我们分解这段代码中的各个元素：

| object | 用于在网页中嵌入多媒体。 |
| height and width | 用于定义对象的高度和宽度。 |
| param | 控制对象的参数。 |
| src | 文件来源。 |
| autostart | 设置为 true 或 false，以控制音频是否自动播放。 |
| controller | 设置为 true 或 false，以显示或隐藏控制器。 |
| bgcolor | 用于设置播放器的背景颜色。 |
| embed | 使用 embed 标签将播放器放置到页面中。 |
| pluginspage | 将用户带到插件下载页面，在那里他们可以下载相应的播放器。 |

你会注意到 `<embed>` 标签中的内容与 `<object>` 标签中的部分内容相似，这是为了兼容所有浏览器。

前面的代码会将 MP3 文件嵌入到我们的 Joomla! 文章中，前提是音频文件名为 `sound.mp3`，并以 MP3 格式保存在名为 `audio` 的目录中，该目录位于 Joomla! 文件结构的根目录下（与 `images` 目录处于同一顶层）。

![](img/0151410972a5c5ea0aab26416e44b1da_143_0.png)

前面的代码会使用用户计算机上的默认媒体播放器来播放音频文件。这虽然可行，但无法灵活控制播放器的行为。因此，我们可以在代码中指定播放器，从而控制音频文件的播放器。

现在有许多基于 Flash 的 MP3 播放器；我特别喜欢的是 http://www.flash-mp3-player.net 上的这款，它是一个功能强大的免费 Flash MP3 播放器，并且具备一些很棒的功能。该网站甚至提供了一个简易代码生成器功能，可以让你轻松生成将播放器嵌入网页的正确代码。如果你使用 flash-mp3-player 作为音频播放器，HTML 代码大致如下：

```
<object type="application/x-shockwave-flash" data="http://flash-mp3-player.net/medias/player_mp3_maxi.swf" width="200" height="20">
<param name="movie" value="http://flash-mp3-player.net/medias/player_mp3_maxi.swf" />
<param name="bgcolor" value="#ffffff" />
<param name="FlashVars" value="mp3=audio/sound.mp3" />
</object>
```

> 诸如 Flash MP3 Player 之类的音频播放器提供了一种播放外部音频媒体的工具。要播放音频文件，需要正确设置音频文件的路径、文件名及其权限。

![](img/0151410972a5c5ea0aab26416e44b1da_144_0.png)

这段代码将以极少的代码知识和工作量，在 Joomla! 文章中加入一个精美的 Flash 音频播放器。如前所述，市面上有许多播放器和在线服务，各有各的功能。其中大多数都是基于 Flash 的播放器，因为近 98% 的网民电脑和浏览器都装有 Flash 播放器。

如果你希望进一步操作 HTML 音频代码，`<object>` 标签中还可以包含许多参数。遗憾的是，参数实在太多，本书无法一一介绍。如需更详细的参数列表，请访问 W3 Schools 网站：http://www.w3schools.com/TAGS/tag_object.asp。

## 使用 Joomla! 插件添加音频

我们已经完成了使用原始 HTML 代码向 Joomla! 文章添加音频文件的繁重工作，现在来看看 Joomla! 音频扩展的强大功能。

Joomla! 音频扩展为核心 CMS 提供了多种多样的扩展功能。安装并配置插件后，添加音频只需简单的“几次点击”即可完成。本章后面将重点介绍一些具体的 Joomla! 音频扩展，但下面的示例将向你展示它们对管理员来说有多么强大。

本示例所选的扩展是 JosDewplayer：

![](img/0151410972a5c5ea0aab26416e44b1da_145_0.png)

JosDewplayer 是一个适用于 Joomla! 的 GPL 许可 MP3 音频播放器。它采用基于 Flash 的播放器界面，可获取 MP3 文件并在 HTML 页面中流式播放。这些文件可以位于你的服务器上，也可以位于远程位置；最新版本支持多文件播放。

要获取适用于 Joomla! 的 JosDewplayer，请访问 http://extensions.joomla.org/ 并搜索“JosDewplayer”。

> 在向 Joomla! 网站安装任何第三方代码之前，请确保在安装扩展前备份你的网站数据库和文件集。

## 在 Joomla! 网站中使用音频

登录你网站的后台管理，然后点击顶部菜单中的**扩展 | 扩展管理器**：

![](img/0151410972a5c5ea0aab26416e44b1da_146_0.png)

使用**选择文件**上传按钮浏览本地计算机，找到 `josdewplayer_plugin.zip` 文件。选中后，使用**上传并安装**按钮将该扩展安装到你的 Joomla! 网站。

安装成功后，前往**扩展 | 插件管理器**配置新插件。此插件属于**内容**类型插件，因此你应在此类型下查找“JosDewplayer”。点击该文本即可打开插件编辑页面：

![](img/0151410972a5c5ea0aab26416e44b1da_146_1.png)

左侧的一组字段可让你根据需要重命名插件、启用或禁用插件，并为站点用户设置访问级别。在这些字段下方是**描述**字段，其中包含使用该插件的基本说明。右侧的一组参数可让你自定义**自动播放、背景颜色和自动重播**功能。**插件参数**通常只需设置一次；在本示例中，我们将发布插件，其他参数设置保持不变。

![](img/0151410972a5c5ea0aab26416e44b1da_146_2.png)

> 确保插件已启用，才能正常工作。

![](img/0151410972a5c5ea0aab26416e44b1da_147_0.png)

让我们回到之前创建的 Joomla! 文章，即包含原始 HTML 音频代码和 MP3 播放器代码的那篇。现在我们的扩展已经完成所有工作，我们只需在文章中输入一个插件能够识别的特殊标记——`{play}`——，然后输入音频文件的 URL 链接，最后再次关闭该特殊标记——`{/play}`。在 Joomla! 文章中输入以下代码：

```text
{play}/audio/sound.mp3{/play}
```

要创建指向该文件的绝对链接，请使用以下代码（将链接替换为音频文件的完整 URL）：

```text
{play}http://localhost/Joomla1.6/audio/sound.mp3{/play}
```

你可以根据音频文件所在的位置使用相对链接或绝对链接。如果链接到外部音频内容，绝对链接非常有用；这也是 JosDewplayer 的一个不错的功能。

在插件参数中全局设置的自动播放、背景颜色和自动重播功能，可以在每次使用标记时被覆盖。例如，要更改默认颜色，可以使用以下代码：

```text
{play}audio/sound.mp3|#000000 [BGCOLOR]{/play}
```

音频播放器现在将显示为：

![](img/0151410972a5c5ea0aab26416e44b1da_147_1.png)

可用参数列表位于插件参数的**描述**字段中。只要你的音频文件路径正确，几秒钟内就能将音频播放器放入 Joomla! 文章。现在，查看你网站的前端页面，即可看到音频通过 JosDewplayer 插件播放。只需插入此插件特有的简单标记，你就可以在网站中任意多次使用该播放器。使用 Joomla! 音频插件为网站页面添加音频，就是这么简单。





在我们开始构建播客之前，让我们先快速了解一下 Joomla! 提供的内置 RSS 功能。

## 什么是 RSS？

术语 RSS 代表 **Real Simple Syndication**。它在基本形式上是一种简单的方法，让人们能够及时了解您网站上的新内容。

RSS 现在在网络上非常流行，其用途示例包括：每周最新的笑话、查看是否发布了廉价机票，以及从网站获取最新的新闻发布。

RSS 订阅源可以通过 RSS 阅读器读取，RSS 阅读器可以是基于桌面的、基于网络的，甚至可以在手持设备（如手机）上运行。标准的 XML 文件格式允许信息被发布并被许多程序查看。

用户通过将其 RSS 阅读器中的 **Uniform Resource Identifier (URI)** 输入，或通过点击网页浏览器中的 RSS 图标（如果可用）来订阅可用的 RSS 订阅源，这将启动向该特定 RSS 订阅源的订阅流程。一旦订阅，RSS 阅读器会定期检查订阅源并下载找到的任何更新，因此可以通知用户该网站的新闻，而无需用户访问网站：

![](img/0151410972a5c5ea0aab26416e44b1da_149_0.png)

## Joomla! 与 RSS

Joomla! 1.5 具有创建 RSS 订阅源的一些不错功能，甚至还有更好的功能可以将 RSS 订阅源获取并在 Joomla! 网站内显示订阅源信息。在第 6 章 *在 Joomla! 网站中包含视频* 中，我们将介绍如何从包含视频内容的 Joomla! 分类中手动创建 RSS 订阅源。可以采用类似的过程来生成音频播客订阅源，但 Joomla! 不会创建友好的播客 RSS 订阅源，因此我们将利用名为 FeedBurner 的第三方 RSS 服务来帮助我们完成此操作。

## FeedBurner

FeedBurner 是 Google 于 2007 年推出的 RSS 网络订阅源管理服务。它是一项免费服务，为博主、播客和其他网络出版商提供客户 RSS 管理工具：

![](img/0151410972a5c5ea0aab26416e44b1da_150_0.png)

我们将利用 FeedBurner 服务来帮助提供播客友好的订阅源，这些订阅源链接回我们的 Joomla! 内容。

## 创建新的播客订阅源

以下是使用 Joomla! 和 FeedBurner RSS 服务创建播客的分步教程。

## 选择和准备您的音频内容

使用 Joomla!，您可以从服务器交付自己的音频播客文件。在本示例中，我们将把一个自包含的音频文件（称为 `audio`，格式为 `.mp3`）上传到我们的 Web 服务器。

如果您使用 FTP，只需将 MP3 文件从计算机移动到新目录即可。如果您使用媒体管理器，请确保您位于希望所在的目录，并使用文件上传功能将音频加载到 Web 服务器上的所需目录。

一旦音频上传完成，我们就可以将其包含在 Joomla! 文章中。

## 禁用 WYSIWYG 编辑器

为了将正确的 HTML 放入我们的 Joomla! 文章中，最好确保内置的 WYSIWYG 内容文章编辑器被禁用。您可以按照以下步骤轻松完成此操作：

- 登录您的管理后台。
- 点击名为 **Site | User Manager** 的菜单项。
- 选择要编辑的用户。
- 在该用户的参数下，将 **Editor – No Editor** 选择为 **Editor** 字段的选项。
- 点击 **Apply** 或 **Save** 保存设置。

编辑器也可以在全局范围内禁用，方法是访问您的全局配置并将 **Default Editor** 字段更改为 **Editor – No Editor**。

## 创建分类

按照以下步骤操作：

- 登录您的 Joomla! 管理后台并进入创建分类区域，该区域位于 **Components | Articles | Categories** 下。
- 在右上角，点击 **New** 图标，这将加载 **Category Manager: Add Category** 屏幕。
- 为您的分类命名。对于我们的示例，我将把它命名为 **Podcast**。**Alias** 可以留空，但请确保分类已发布。如果您愿意，可以为此分类添加描述。

目前我们只需要做到这一点，因此通过点击页面右上角的保存按钮完成流程。

您将看到新分类已创建并显示在分类列表中。

## 创建用于显示音频的文章

下一步是创建一个新的 Joomla! 文章，其中将包含您的音频内容。

- 如果您已在分类屏幕中，点击文章视图的链接。
- 如果没有，则点击控制面板上的文章管理器图标或转到 Components | Articles 菜单项。
- 点击页面右上角的创建新文章按钮，输入文章标题，并选择之前创建的 Podcast 分类。
- 我们现在需要在文章中包含 HTML 代码（请参阅本章前面提到的嵌入和在 Joomla! 文章中显示音频部分）。

在将复杂的 HTML 代码输入 Joomla! 文章时，请确保您的 WYSIWYG 编辑器未在 Joomla! 文章中打开。

一旦音频代码就位，点击 Apply 按钮（右上角）以应用更改并保存新文章。

## 创建菜单链接和 RSS 订阅源

一旦您创建了音频文章（或多篇文章）并将其放置在适用的“音频播客”分类中，就该利用 Joomla! RSS 订阅源菜单功能了。

创建指向分类的菜单链接是通过转到您希望放置新链接的菜单，例如 Menus | MainMenu，然后点击屏幕右上角的新图标来完成的：

![](img/0151410972a5c5ea0aab26416e44b1da_152_0.png)

由于本节是关于为 RSS 项目创建菜单链接的，我们不会花时间介绍所有菜单参数，但对于我们的 RSS 菜单链接，最重要的参数是：

- **Title:** 为您的菜单链接输入标题。对于我们的示例，我将菜单链接命名为 **Audio Podcast**。
- **Type:** 点击 **Change Type** 按钮以打开菜单 **Type** 选项，这将打开一个包含菜单类型选项的光箱。通过点击相应的链接创建一个名为 **Link to Category** 的新菜单类型。弹出光箱将消失，适当的菜单字段将被填充。
- **Parameters (Basic) - Category:** 您需要选择希望菜单链接指向的分类。
- **Parameters (Advanced):** 确保 **Show a Feed Link** 单选按钮设置为 **Yes**。

如果您想填写菜单项的任何其他参数或属性，请随意填写，但前述的是创建我们的新 **Link to a Category** 菜单项的主要要求。

点击该页面上的 **Save** 保存新的菜单项。

## 它是如何工作的

如果您查看网站的前端页面，您现在应该会看到您创建的新菜单链接。如果看不到，请确保您的菜单链接已发布，并且您的菜单模块已发布以在首页显示菜单。

点击此菜单项，这将带您进入分类页面，显示指向您播客文章的链接。

由于大多数现代网页浏览器，如 Safari 和 Firefox，都内置了 RSS 阅读器，一旦您进入此分类页面，您应该会在浏览器中看到 RSS 图标。在 Mozilla Firefox 和 Safari 中，这将是浏览器 URL 字段右侧的图标。点击它以查看此特定网页的 RSS 订阅源：

![](img/0151410972a5c5ea0aab26416e44b1da_153_0.png)

点击图标后，您可能会收到多个选项：RSS 2.0 或 Atom 1.0。这些是聚合语言，将在本书记述的后续章节中介绍。目前，两者都会显示您的新 RSS 订阅源，并允许用户订阅和查看它们。

## FeedBurner

现在有趣的部分开始了。我们将使用 Google 的 FeedBurner 服务创建一个播客友好的订阅源。

FeedBurner 服务位于 http://feedburner.google.com。如果您还没有，您需要使用 Google 创建一个免费账户才能使用此服务。

当您登录 Google 账户后，访问上述 URL，您将被提示输入 RSS 订阅源位置的框：

![](img/0151410972a5c5ea0aab26416e44b1da_154_0.png)

> 将您播客分类的 URL 粘贴到此框中，而不是实际的 RSS 链接本身。然后确保勾选此框右侧的 I am a podcaster! 框并点击 Next 按钮。

如果您输入的链接是通往 Joomla! 分类页面的有效 URL 链接，FeedBurner 将接受它并带您进入下一步。如果您在链接方面遇到问题，请在 Joomla! 网站中创建一个新的菜单链接，该链接直接指向您的“Podcast”分类，点击该链接，复制 URL，然后将其粘贴到 FeedBurner 中。这应该是所需的一切，FeedBurner 现在将报告它已从该 URL 找到两个有效的 RSS 链接——RSS 2.0 和 Atom 1.0。这些是不同的聚合语言，对于本示例，我将使用 RSS 2.0 选项：

![](img/0151410972a5c5ea0aab26416e44b1da_154_1.png)





接下来我们需要为我们的 feed 提供一个用户可以查看的描述性 **标题**，并提供 **Feed 地址**，该地址将以 `http://feeds.feedburner.com/` 开头，然后在末尾加上我们的后缀。对于本示例，我将使用标题 **Amplify Podcasts** 和 Feed 地址 **amp_podcast**。填写完这些信息后，点击 **Next**：

![](img/0151410972a5c5ea0aab26416e44b1da_155_0.png)

除非您输入到字段中的信息已被分配，否则您应该会看到全新的闪亮播客 feed：

![](img/0151410972a5c5ea0aab26416e44b1da_155_1.png)

## *在您的 Joomla! 网站中使用音频*

FeedBurner 服务为您提供额外的功能来自定义您的 feed，甚至可以自动将其提交到 iTunes。我们需要利用其中之一，因此点击 **Next**。

在此页面的顶部，确保选中 **Create podcast enclosures from links to: any rich media file**。它应该是默认设置：

![](img/0151410972a5c5ea0aab26416e44b1da_156_0.png)

您可以为播客提供描述、一些关键词，甚至将其添加到 iTunes 和 Yahoo! 搜索引擎。然后点击 **Next**。

在此最后一页，FeedBurner 为您提供强大的统计数据，让您了解谁在订阅您的播客以及他们点击 feed 中内容的频率：

![](img/0151410972a5c5ea0aab26416e44b1da_156_1.png)

点击 **Next**，我们在这里就全部完成了！

接下来，我们需要让人们知道您的新播客 feed。在您的 FeedBurner 页面中，您应该会看到一个标签为 Publicize。点击它，然后在左侧菜单中点击标签为 Chicklet Chooser 的项目：

![](img/0151410972a5c5ea0aab26416e44b1da_157_0.png)

滚动到页面底部，您可以在此处复制一些 HTML 代码：

```
Copy the HTML below for use in your own page templates:
<a href="http://feeds.feedburner.com/amp_podcast" title="Subscribe to my feed" rel="alternate" type="application/rss+xml"><img src="http://www.feedburner.com/fb/images/pub/feed-icon32x32.png" alt="" style="border:0"/></a><a
```

复制代码，返回您的 Joomla! 站点，并创建一个新的模块，将此 HTML 代码粘贴进去，链接到您的新 feed。在您的播客页面上发布它，用户将能够直接订阅您的 feed：

![](img/0151410972a5c5ea0aab26416e44b1da_157_1.png)

继续从您的网站制作播客现在是一个简单的过程。当您想要创建新的播客时，只需创建一个新的 Joomla! 文章并将音频播放器嵌入其中，或创建指向播客材料的链接并将此文章保存到您的“Podcast”分类中。Joomla! 和 FeedBurner 将完成其余工作，自动为这篇新文章创建新的 RSS feed 条目。

## 使用扩展进行播客

还有其他方法可以从我们的 Joomla! 站点进行播客。这些方法涉及使用 Joomla! 第三方扩展的强大功能，而其中最好的工具之一是 Joseph LeBlanc 的“Joomla! Podcast Suite”：

![](img/0151410972a5c5ea0aab26416e44b1da_158_0.png)

Podcast Suite 扩展包含一个 Joomla! 组件、模块和插件，提供了触手可及的广泛播客工具。您可以快速启动第一个播客，并轻松构建和管理这一持续进行的工作。

我们将在下一节中重点介绍 Podcast Suite 扩展，该节涵盖第三方音频扩展。

## 第三方音频扩展

Joomla! 扩展为我们提供了多种强大的音频解决方案，可以由管理员安装和配置，几乎不需要开发人员的知识。

现在有大量的扩展可用于 Joomla!。对于 Joomla! 站点管理员来说，最困难的决定通常不是找不到一个能满足您需求的扩展，而是从可用选项中找出最合适的。以下部分重点介绍了最受欢迎的非商业第三方音频扩展。

### Podcast Suite

Joomla! 的 Podcast Suite 独树一帜。它“名副其实”，您可以在短短十分钟内在 Joomla! 中展示播客：

![](img/0151410972a5c5ea0aab26416e44b1da_159_0.png)

插件会显示 ID3 标签信息，包括标题、专辑名称、曲目长度和文件大小。它还生成一个链接，使用户可以下载 MP3 文件以及在网页中使用嵌入式 MP3 播放器播放文件。

插件通过在您的 Joomla! 文章中插入易于使用的插件标签来调用。标签如下所示：{music}your/path/to/directory{/music}，它会自动扫描目录中的文件，并在表格列表中输出它们。

**Plugin Parameters** 允许您调整表格的宽度和颜色，替换下载图像图标，并调整列标题：

![](img/0151410972a5c5ea0aab26416e44b1da_161_0.png)

要下载 MP3 Browser，请访问 http://extensions.joomla.org 并搜索“MP3 Browser”。

![](img/0151410972a5c5ea0aab26416e44b1da_163_0.png)

正如您从相关图像中看到的那样，该扩展还具有语言支持。

> 如果您从开发人员网站下载 com_jukebox.zip 文件，请确保先解压缩，然后再上传到扩展管理器。下载文件包含组件和模块，两者都需要安装。

组件允许您为音乐文件创建分类。您可以添加描述、专辑封面和其他音乐信息，以及使用便捷的上传功能上传音频文件和专辑封面：

![](img/0151410972a5c5ea0aab26416e44b1da_163_1.png)

包含的模块将允许您轻松显示音频文件的列表：

![](img/0151410972a5c5ea0aab26416e44b1da_163_2.png)

要下载 Jukebox，请访问 http://extensions.joomla.org 并搜索“Jukebox”。

## 1 PixelOut Audio Player

1 PixelOut Audio Player 插件利用了 1 PixelOut Flash Audio Player，并将其打包，以便在 Joomla! 中作为插件轻松使用：

![](img/0151410972a5c5ea0aab26416e44b1da_164_0.png)

与大多数 Joomla! 插件一样，其美妙之处在于使用的简单性。1 PixelOut Audio Player 只需您在 Joomla! 文章中输入一个代码标签即可，基本上就完成了。此标签调用播放器加载，您将在 Joomla! 文章中看到一个外观精美的音频播放器。

该插件包含许多参数，真正使其成为首选播放器。这些参数不仅允许您定义音频文件的默认路径，而且您可以完全自定义播放器的颜色以适应您的模板：

![](img/0151410972a5c5ea0aab26416e44b1da_164_1.png)

要下载 1 PixelOut Audio Player 插件，请访问 http://extensions.joomla.org 并搜索“1 PixelOut Audio Player”。

## Zina

Zina 是一个功能丰富的 Joomla! 音频组件，为您的音乐收藏提供图形界面、个人点唱机、播客工具和 MP3 播放器。

Zina 已构建为 Joomla! 组件，可以集成搜索功能，这意味着您的音频收藏可以通过 Joomla! 搜索框进行搜索。

如果您想展示音乐专辑并提供这些专辑的试听歌曲，那么 Zina 可能是一个不错的选择：

![](img/0151410972a5c5ea0aab26416e44b1da_165_0.png)

要下载 Zina，请访问 `http://extensions.joomla.org` 并搜索“Zina”。

## 浏览器支持和音频可访问性

音频是娱乐和向网站访客传递信息的绝佳工具，现在在网络上非常常见。事实上，共享音频文件现在已成为网络上最受欢迎的活动之一。

音频正成为显著增强网页可访问性的有用设备。对于视力受损的人来说，通过音频描述提供视觉内容的能力极其宝贵。

问题出现在没有提供正确的文本替代时，这会使页面上的音频内容对某些受众无法访问。您的音频对听力受损者来说将无法访问，因此可以提供准确的文本描述来描述音频文件的内容。

## 良好实践技术

以下是使用音频在 Joomla! 页面中的一些良好实践技术：

### 文本转录

提供您网页上所有内容的文本转录是良好的做法。例如，图像的 `alt` 标签提供了替代文本的文本转录，允许用户知道该主题的图像正在加载到页面区域中。





如果你的音频内容是关于会议或产品新闻，请提供该内容的文本转录，并通过超链接使其可在另一个区域访问，或者更好的是，在同一页面的某个位置提供。文本转录可以从几个词长到数百个词；你的良好判断对于准确描述你的音频内容非常重要。

## 替代内容

包含网页中音频的 HTML 也可以包含替代内容。object 元素可用于包含各种类型的信息；远不止图像标签 <img />。

替代内容不仅可以帮助使用文本浏览器的用户和屏幕阅读器，还可以被搜索引擎收录。

## 字幕

字幕可能并不总是合适的。然而，对于新闻或培训类的音频内容，可能适合添加字幕。给音频文件添加字幕实际上将其转变为视频文件，因为它将包含音频和视觉内容。许多软件应用程序都提供简单的文本字幕功能来补充音频轨道。

## 链接到下载

在网页上提供指向播放内容所需的音频播放器类型的链接非常有用，并且可能对用户收听音频内容产生很大影响。

## 音频控件

提供用于操控音频播放的控件是很好的做法，现在几乎已成为一种期望。音频控件让访客能够控制轨道的播放速度、暂停以及调节音量级别。

诸如播放条、暂停和音频按钮等简单功能，都能使播放器比例如无控制功能的背景音频轨道更易于访问。

“在 Web 上使用哪种音频格式最好？”是一个常见问题，没有任何一种格式能对所有人实现 100% 兼容。诀窍是选择一种适合你网站访客绝大多数人的格式，然后再尝试以其他方式满足其他用户。

根据众多研究，Adobe Flash 现在是个人计算机系统上最流行、最广泛的流媒体播放器之一。几乎所有现代网络浏览器现在都已安装 Flash，并且统计数据显示超过 98% 的个人电脑可以播放 Flash 文件。这就是为什么 Joomla! 扩展目录中的大量音频播放器都是基于 Flash 的音频播放器。

如果你的音频内容很重要，请花时间将其导出并以多种格式提供。这为你的网站用户提供了选择，例如，那些无法运行 Flash 的用户仍能在其他播放器中收听。

## 摘要

尽管 Joomla! 没有提供原生的音频功能，但能够将自定义 HTML 代码输入到我们的网站内容中，为我们在需要包含音频时提供了极大的灵活性。

将 Joomla! 与 FeedBurner RSS 工具结合使用，为从你的网站提供播客提供了一种自动化且易于访问的方式，能够轻松生成新的播客并监控内容的统计数据。

Joomla! 扩展为你的网站添加了增强的播客和音频播放器，并且本章中列出的扩展的功能无疑会匹配并超越其他 CMS 的音频功能。

音频格式及其可访问性的选择乍一看似乎令人困惑，但有几种标准格式（如 MP3）很受欢迎且安全，能够覆盖最广泛的受众。为了遵守 W3C 可访问性指南，始终尝试为你的音频提供替代方案，这可以是总结音频内容的文本描述。

# 在你的 Joomla! 网站中使用视频

互联网上的视频正在蓬勃发展！视频压缩的改进、连接速度的提升以及硬件的改善等相对较新的因素，都促成了互联网成为交付视频内容更可靠、更易访问的环境。

本章重点介绍在你的 Joomla! 网站中使用视频媒体，并包括以下主题：

- Video on the Internet
- Video formats
- Embedding and displaying video in Joomla!
- Creating video podcasts for Joomla!
- Third-party video extensions
- Browser support and accessibility

### Video on the Internet

术语视频指的是对一系列静止图像的记录、处理和显示，这些图像代表运动中的场景。

![](img/0151410972a5c5ea0aab26416e44b1da_169_0.png)

视频可能是当提到多媒体一词时最引人注目的“媒体类型”之一。视频是一种数据密集型媒体格式，即使是短视频片段也需要兆字节的数据。直到最近，这样的限制一直是其在网络上广泛使用的制约因素！

在过去几年中，网站上视频的使用迅速增加，视频已被用于销售、营销以及支持和培训目的。出现了一些完全专注于托管和向用户展示视频剪辑的新网站，而视频潜力的这种演变归因于以下促成因素：

### 带宽限制

消费者宽带的增长极大地促进了网站上视频的演变。现在很大一部分网站访客拥有通过网络访问高质量视频内容所需的带宽。这还不是最好的，但仅此一点就已成为“网络功能”增长的最大影响因素之一。某些地区的带宽已变得足够好，以至于 Blockbuster 电影流媒体和电视现在将互联网作为广播媒介来定位。

### 视频与其他网络内容的集成

直到最近，在网络上播放的大多数视频除了在简单盒式播放器中播放简单视频外，没有提供丰富的媒体功能。倒回几年，所有网站上的视频链接都必须先下载到用户的计算机，然后才能播放。

随着视频格式的演变、流媒体视频（可在下载时播放的内容）以及 Adobe Flash Player 等技术的发展，视频的交付及其与其他网络内容的交互带来了更佳的用户体验。

### 为网络创作和播放视频

由于可用的浏览器和操作系统种类繁多，网站上的视频往往要求用户暂停正在进行的操作并下载插件才能访问视频内容。现代浏览器已配备良好，能够播放多种格式的视频内容，通常无需额外的插件。

![](img/0151410972a5c5ea0aab26416e44b1da_171_0.png)

另一个促成因素是创建内容的便捷性。过去，你必须是一名经验丰富的视频录制/编辑人员并使用专业工具才能制作视频。最近，新的计算机操作系统预装了某种形式的视频编辑软件。创建视频并将其上传到网站的便捷性对其作为流行媒体格式的广泛使用产生了深远影响。

随着迄今为止的演变，视频和互联网非常契合。我们可以获得尽可能接近真实生活的体验，它是教育性的或娱乐性的，并且我们可以在最有趣的游乐场之一——网络上获得这一体验！

随着 YouTube 和 Google Videos 等分发渠道的使用，网站上视频的使用发生了显著演变。与其自行托管视频内容（这通常会导致网络托管成本增加），现在可以轻松地将外部视频内容从分发站点流式传输到你的网站。广播和共享这种视频内容的便捷性是网络视频热潮的另一个主要促成因素。

## 视频格式

有许多视频格式可用。每种格式都会影响对不同受众的可访问性，其中一些是由微软和苹果等公司许可的格式。视频格式各自包含自己的算法，以不同的方式处理和处理电子视频数据：

![](img/0151410972a5c5ea0aab26416e44b1da_171_1.png)





有些格式比其他格式更适合某些用途（如在 DVD 上存储视频或在 iPod 上播放视频），我们将介绍哪些视频格式适合基于网络的环境。

## 视频压缩

在使用电子设备上的视频时，一个重要问题是文件大小。这通常是互联网上使用视频的限制因素。然而，视频现在变得对消费者友好，甚至手机也能轻松录制视频。

视频是一种数据密集型媒体格式，在计算机硬盘上占用大量空间。将其带入网络场景，网络托管服务器成本可能会因托管和播放视频文件所需的额外磁盘空间而变得昂贵。

解决办法是视频压缩。通过压缩视频，更容易分发和存储它。数字视频压缩可以通过压缩数据而不会影响最终产品的感知质量来实现。大多数视频压缩方法都属于“有损”数据压缩格式。这意味着每次压缩视频时，某些信息就会被删除。一旦导出，这些信息就会丢失，无法再恢复：

![](img/0151410972a5c5ea0aab26416e44b1da_172_0.png)

压缩的作用是去除被认为“不重要”的数据，因此由于这些不再存在的信息，我们的文件大小会下降。视频压缩的一个很好的例子是从视频片段中删除无法识别的颜色。一部电影中可能有数百万种颜色（相似颜色的细微色调），但人类只能感知到其中略多于 1000 种，因此压缩并删除这些不需要的数据对我们是有益的。

丢弃太多数据（过度压缩）时，变化就会变得明显。在压缩视频时，目标是尽可能多地压缩，直到你能注意到质量损失，然后从这一点稍微回退一点，这样你就可以在最小的质量损失下实现最佳的数据压缩和文件大小。

## 视频编解码器

从前，视频编解码器是“AV 极客”和“硬核非法视频下载者”的东西，但信不信由你，如果你使用或观看电影，无论是在电影院、电视上还是在手机上，你可能经常使用它们。

**视频编解码器**是一种设备或软件，可实现视频压缩，并对该数据进行解压缩。术语 **编解码器**代表压缩/解压缩，各种编解码器用于音频和视频媒体格式。

由于媒体文件大小庞大，数学编解码器被构建用于对信号进行编码以便传输，然后再次解码以供观看。现在使用着数百种编解码器，其中许多已包含在现代计算机设备中。一些常见的视频编解码器是 VC-1、MPEG-4 和 H.264。

需要注意的是，视频编解码器不是视频容器（或包装）文件，即最终文件格式。你可能已经注意到，你的视频文件没有像 `.vc-1` 或 `.h.264` 这样的扩展名。这是因为视频文件被打包在容器或包装中，这些容器包含各种文件信息，例如额外的音频和导航信息，以及视频文件本身。编解码器负责压缩，而容器文件保存该文件的信息。视频容器文件的例子有 FLV（Flash Video）、MOV（Apple QuickTime）等。每个容器都使用某种类型的视频编解码器进行压缩。有关视频编解码器和容器文件的更多信息，可在 http://en.wikipedia.org/wiki/Category:Video_codecs 上找到。

我们很快就会研究这些容器文件，因为它们是保存网络视频的重要格式。

## 比特率

比特率描述了每单位时间内处理的信息量。比特率是视频和音频压缩的重要组成部分，所有主要的视频压缩格式都使用各种比特率在压缩期间压缩数据。

![](img/0151410972a5c5ea0aab26416e44b1da_174_0.png)

比特率的单位是每秒比特数（bps 或 bit/s），可使用诸如（千）Kbps 或（兆）Mbps 等前缀。当从视频编辑应用程序导出文件时，你可能会看到比特率作为导出选项。重要的是要记住，比特率越高，文件包含的信息越多， resulting in a higher quality video file。比特率越高，编码/解码所需的 effort 越大，文件大小也越大。有关视频比特率的更多信息，可在 http://en.wikipedia.org/wiki/Bit_rate 上找到。

## 视频容器

有许多压缩标准用于将视频文件大小压缩到高效状态。压缩后的视频文件将被包装在容器文件中，其中包含播放视频所需的所有相关信息。为了播放此视频，查看系统需要理解文件使用的视频编解码器以及视频容器。

以下是网络上最常用的视频容器列表：

- AVI
- FLV
- MOV
- MPEG
- WMV
- RM

以下是这些容器格式的简要介绍：

### AVI

AVI 代表 **Audio Video Interleave**，这是一种由微软于 1992 年开发的知名视频容器格式。AVI 文件质量极佳，但这通常以文件大小为代价。它是视频编辑人员保存和查看视频的热门选择，因为质量高。

AVI 文件有许多压缩编解码器，因此如果系统不包含特定编解码器，则其中一些无法播放。这可能意味着需要特定的更新才能观看某些 AVI 电影。然而，大多数新计算机系统应该能够在无需额外软件的情况下播放 .avi 电影文件。

### FLV

FLV 代表 **Flash Video**。它是 Adobe Systems 拥有的格式，专为网络视频播放而设计。它提供高压缩率和高品质视频。

FLV 是一个流行的格式，因为它可以轻松嵌入网页中进行实时流媒体播放，并且在大多数计算机操作系统中通过 Adobe Flash Player 及其网页浏览器插件得到支持。

目前，FLV 是 YouTube 和 MySpace 等热门视频广播网站的首选格式，被认为是在网络上投递视频最受欢迎的格式之一。

Flash 电影是网络视频的绝佳格式，但它们也有其局限性。一些公司不允许在其 IT 网络上安装 Flash Player。还有一些关于全 Flash 制作网站的搜索引擎优化限制。

### MOV

**QuickTime**（文件扩展名为 .mov）是 Apple 开发的多媒体播放和存储技术。QuickTime 文件充当容器，将 Flash、文本、音频和视频组合成一个文件。

QuickTime 电影通常质量极佳，该格式提供良好的压缩率。QuickTime 播放器随 Apple Mac 电脑一起提供，但 Windows 用户可能需要下载 QuickTime 播放器才能查看 MOV 文件。

### MPEG

MPEG 代表 **Moving Picture Experts Group**，该组织成立于 1988 年，作为一个工作组来定义音频和视频媒体类型的标准格式。

MPEG 文件是网络使用的常见格式，提供良好的质量压缩和文件大小。自 1980 年代以来，MPEG 家族已多次增加，最新的最流行格式是 MPEG-4（.mp4）。

MPEG 文件可以用 Windows Media Player 或 Apple QuickTime Player 播放，这意味着它们可能是跨平台最合适的格式之一。这导致它们在整个网络上得到广泛使用。最新格式 .mp4 提供卓越的压缩，质量极佳且文件大小小。

### WMV

WMV 代表 **Windows Media Video**，由微软创建并授权，作为 RealVideo 的竞争对手。其质量与其他类似编解码器相当，并得到所有微软工具的支持，这使其成为使用 Windows 计算机创建视频时的自然选择。

任何不使用 Windows 计算机的人都需要下载 Windows Media Player 或等效播放器才能播放 .wmv 文件。因此，为了覆盖最广泛的受众，使用更合适的容器来制作基于网络的视频。

### RM

**Real Audio/Video** 是流媒体视频的先驱之一。它是由 RealNetworks 开发的专有视频格式，于 1997 年首次发布。其流媒体格式和良好的压缩率提供了相对较小文件大小中的高质量视频。

RealVideo 通常与音频打包并组合在容器（.rm）文件中。RealMedia 格式曾经非常流行，但专有技术导致该格式衰落，取而代之的是更易获得的解决方案。

还有许多其他视频容器可用于不同的环境。有关这些的更多信息可在 http://en.wikipedia.org/wiki/Video_container 上找到。





## 在 Joomla! 网页中嵌入和显示视频

除了 Joomla! 开箱即用的众多出色功能外，一个非常强大的功能是允许将自定义 HTML 代码包含在 Joomla! 文章中。这意味着我们完全可以根据需要定制文章中内容的输出形式，而这正是将视频插入 Joomla! 网页时所需要的，无需使用扩展插件。

### 视频源

在为网页准备视频时，最重要的选择之一就是使用哪种视频格式。

不幸的是，没有任何单一的视频格式能够百分之百保证适合所有网站访客。由于他们使用的计算机操作系统、屏幕分辨率和网页浏览器各不相同，因此需要仔细考虑，以确保大多数用户都能观看视频，而不需要额外下载或安装插件。

我们在本章前面已经列出了视频格式。在提供这些视频格式支持时，三大主要参与者如下：

| Adobe Flash | SWF 电影 |
| Apple QuickTime | MOV, MPEG-1/2/4, AVI（取决于所使用的编解码器） |
| Windows Media | WMV, MPEG-1, AVI（取决于所使用的编解码器） |

Adobe Flash 声称其在市场中的占有率高达 98%，并且被 Google、YouTube 和 Yahoo! 等视频分发网站广泛用作视频格式选择。如果高比例的用户能够观看 Flash 内容，那么这就是考虑使用 Flash 作为网站视频格式的一个绝佳理由。

在某些情况下，Flash 的安装可能受到限制，例如在公司计算机网络上。为了适应此类受众，将内容同时提供为 MPEG-4 视频格式可能是一个合适的选择。

### 编码

网络视频的一个基本步骤是为其网络传输做好准备。您的源材料格式可能与您决定使用的格式不同，如果是通过手机、DVD 摄像机或其他设备录制的，则其格式很可能非常不适合用于网络。

目前有许多视频导出和转换软件应用程序可用。虽然详细阐述这些内容会非常有益，但关于视频编辑和优化已经写了不少整本书。以下是从视频编辑软件导出电影时可以作为指南的一些基本信息。这些设置非常适合使用宽带连接浏览您网站的用户：

| Adobe Flash 格式 | 帧大小和宽高比：(4:3) 384 x 480, (16:9) 270 x 480。 |
|  | 帧速率：12-15 帧/秒。 |
|  | 去隔行：应设置为“是”。 |
|  | 音频比特率：64 Kbps（语音）至 128 Kbps（音乐）。 |
| Apple QuickTime 格式 | 帧大小和宽高比：(4:3) 640 x 480 或 320 x 240, (16:9) 640 x 352。 |
|  | 帧速率：15 帧/秒。 |
|  | 去隔行：应设置为“是”。 |
|  | 音频比特深度：16 位。 |
| Windows Media 格式 | 帧大小和宽高比：(4:3) 640 x 480 或 320 x 240, (16:9) 640 x 352。 |
|  | 帧速率：15 帧/秒。 |
|  | 去隔行：应设置为“是”。 |
|  | 音频比特深度：16 位。 |

原始视频需要编码为流媒体格式，以生成合适的 QuickTime、Windows Media、Flash、RealMedia 或 MPEG-4 文件。视频编码所需的长度和时间取决于导出设置、软件和计算机处理能力。现在有许多免费和商业软件应用程序可用于执行视频编辑、导出和文件转换。

值得一提的一个开源编码工具是 MediaCoder，网址为 http://www.mediacoderhq.com/download.htm。MediaCoder 是一个免费的通用媒体转码器，几乎支持所有流行的编解码器。MediaCoder 还可以转码音频，为您提供一把媒体编码的“瑞士军刀”。在 Internet 上搜索即可为您提供适合您计算机平台的编辑工具，并且有很多可供选择。

### 上传

无论您选择使用 FTP 还是内置的媒体管理器，将视频上传到网络服务器都是一个简单的过程。

#### 通过 FTP

打开您的 FTP 程序并登录到网络服务器。在大多数图形化 FTP 程序中，您会有两个视图，一个用于本地文件，另一个用于远程文件：

![](img/0151410972a5c5ea0aab26416e44b1da_179_0.png)

在远程文件集中，导航以查看 Joomla! 根目录结构。在此级别下，创建一个名为 videos 的新文件夹。在本地计算机中查找要上传的视频文件，并将其发送到新的 video 目录中。

> 创建一个空白的 .html 文件并将其上传到新的 video 目录是一种良好的做法。这有助于阻止任何人直接查看新 video 目录中的所有视频文件。如果您无法创建新的 .html 文件，请复制一个位于 images 目录中现有的 Joomla! .html 文件。

#### 通过媒体管理器

为了通过媒体管理器上传文件，请登录管理后台并访问 **内容 | 媒体管理器** 功能。使用左侧的目录结构导航到现有目录，或使用 **创建文件夹** 功能创建一个新的 video 目录：

![](img/0151410972a5c5ea0aab26416e44b1da_180_0.png)

> 媒体管理器的位置默认使用全局配置中的设置。通常这是在 /images 目录内。如果您希望在 /images 同一层级创建一个根目录（就像我们使用 FTP 方法所做的那样），则需要更改全局配置中的默认媒体管理器路径，然后返回媒体管理器区域创建新目录。

![](img/0151410972a5c5ea0aab26416e44b1da_181_0.png)

全局配置还包含其他媒体设置，例如最大文件上传大小，这在上传视频时可能需要考虑。

创建 videos 目录后，只需使用媒体管理器的文件上传功能将文件上传到所选目录即可。

### 编辑器

WYSIWYG 编辑器最令人沮丧的事情之一是，它们会剥离或调整您格式化良好的代码。有些编辑器在这方面比其他编辑器做得更糟，但为了您的利益，在 Joomla! 文章中输入除简单 HTML 自定义代码以外的任何内容时，最好关闭 WYSIWYG 编辑器。

虽然我刚建议您关闭编辑器，但我知道它们有时很有用，而且有些用户离不开它们。一个简单的小技巧是创建一个超级管理员用户，并将其命名为类似 "noeditor" 的名称。然后为该用户设置编辑器选项为 **编辑器 - 无编辑器**。使用这种方法，您可以随时以 "noeditor" 用户身份登录。这节省了频繁切换编辑器开和关的麻烦。

可以通过访问 **站点 | 用户管理器** 页面按“用户基础”调整用户编辑器设置。点击您希望编辑的用户，将会有一个标有 **编辑器：** 的参数设置：

![](img/0151410972a5c5ea0aab26416e44b1da_182_0.png)

选择您需要的选项，并在用户编辑页面上点击 **保存**。也可以在更高层级的全局管理设置中设置编辑器。同样，访问 **站点 | 全局配置** 菜单以调整此设置。进入全局配置页面后，您会看到一个标有 **默认编辑器** 的设置。选择您需要的选项，并在全局配置页面上点击 **保存** 以保存更改：

![](img/0151410972a5c5ea0aab26416e44b1da_182_1.png)

在 Joomla.org 上，有许多可以轻松安装的编辑器扩展，它们为您的网站内容编辑提供了一些很棒的功能。这些扩展对代码的处理方式各不相同，但作为一个普遍规则：在 Joomla! 文章中输入复杂 HTML 代码时，最好关闭所有编辑器。一旦自定义 HTML 代码被包含在文章页面并保存，开启 WYSIWYG 编辑器并重新访问文章时要格外小心。当您的 WYSIWYG 编辑器加载源代码时，它可能会剥离掉您所有的努力，留下一个无法正确显示的文章。

为了帮助使 Joomla! 编辑器对 HTML 更加友好，您可以调整编辑器过滤器的安全性。方法如下：

- 在 Joomla! 管理后台中，访问文章管理器屏幕。
- 在页面的右上角，您将看到一个编辑全局文章“参数”的选项。



- 点击此项，然后在屏幕底部，你将看到编辑器过滤列表。
- 选择你的过滤组（例如，超级管理员），并在过滤类型下选择“白名单”。

你也可以配置特定的 HTML 以从编辑器过滤列表中忽略。确保在页面顶部保存这些新配置。

## 创建文章

新文章在“文章管理器”视图中创建：

- 点击控制面板上的 **Article Manager** 图标，或在 **Components | Articles** 菜单链接下。
- 进入文章管理器后，点击该页面右上角标记为 **New** 的图标。这将打开 **Add Article** 页面。
- 新文章的少数要求之一是页面 **Title**，以及文章内的一些文本。一旦这两项已填写，你将能够点击该页面右上角的 **Save** 或 **Apply** 按钮。

## 视频 HTML 代码

视频可以在你的 Joomla! 文章中有两种呈现方式：

### 直接下载

你可以直接提供指向视频文件的超链接，这将允许用户下载视频文件，下载完成后，可以在浏览器窗口中使用插件查看，或通过 QuickTime 或 Windows Media Player 等视频播放应用程序在用户机器上播放。

下载方式是视频内容在网络流媒体和嵌入内容变得普遍之前所采用的方式。直接下载在你希望允许用户下载并保存视频内容时很有用。提供视频下载的代码是一个指向文件的简单超链接，看起来像这样：

```
<a href="/videos/boats.mov" title="Video of Boats Download 1.2MB" >
My Movie about Boats (1.2MB)</a>
```

页面看起来像：

![](img/0151410972a5c5ea0aab26416e44b1da_184_0.png)

> 在创建指向视频文件的链接时，始终在链接旁边显示文件大小和其他信息是良好的做法。这些信息可以帮助用户了解他们将收到什么内容，以及预期的下载时间。

### 流媒体

流媒体是将压缩的多媒体内容传递并实时显示给用户的一种过程。网络流媒体的例子包括互联网广播和电视广播。

在流媒体视频的情况下，一旦下载了足够的数据，用户播放器（QuickTime Player、Windows Media Player）就可以开始播放视频，同时视频流的剩余部分继续下载。此过程意味着用户不必等待整个文件下载完成即可观看特定内容。

流媒体视频通常从服务器上的预录视频文件发送，但流媒体也可以涉及“现场视频广播（或馈送）”。现场视频馈送被转换为压缩视频信号，然后可以通过能够多播或同时向多个用户发送文件的网络流媒体服务器传递。

### 嵌入代码

与大多数 HTML 代码一样，不同浏览器以各种方式解释它。为了确保我们的视频在所有浏览器中都能正常播放，我们需要使用 W3C 推荐的方法，即同时使用 `<object>` 标签和 `<embed>` 标签（推荐用于 Netscape 和旧浏览器）。

使用这两个标签可以让我们的视频在所有浏览器上都能正常播放，这为我们提供了将视频提供给最广泛受众的最佳机会。

<object> 标签用于将音频、视频和 Flash 等多媒体对象包含到网页中。在 object 标签内有参数和其他标签，它们都有各自的功能（对不同浏览器），用于正确显示你的内容。

在你的 Joomla! 文章中嵌入视频的最小有效代码看起来像这样：

```
<object width="320" height="240" classid="clsid:02BF25D5-8C17-4B23-BC80-D3488ABDDC6B" codebase="http://www.apple.com/qtactivex/qtplugin.cab">
<param name="src" value="/videos/videofile.mov">
<param name="controller" value="true">
<param name="autoplay" value="false">
<embed src="/videos/videofile.mov" width="320" height="240" autoplay="false" controller="true" pluginspage="http://www.apple.com/quicktime/download/">
</embed>
</object>
```

让我们看看这段代码的元素：

| object | 用于在网页中包含多媒体。 |
| height and width | 用于定义对象的高度和宽度。这通常与视频文件大小相同或稍大，有时我们需要一些额外空间来显示视频周围的视频控件。 |
| classid and codebase | 定义用于自动安装 ActiveX 元素的特定指令。 |
| param | 控制对象的参数。 |
| src | 文件的来源。 |
| controller | 设置为 true 或 false 以显示或隐藏控制器。 |
| autoplay | 设置为 true 或 false 以自动开始播放视频。 |
| embed | 使用 embed 标签将播放器放置在页面上。 |
| pluginspage | 将用户带到插件下载页面，在那里他们可以下载特定的播放器。 |

你会注意到 <embed> 标签内的内容与 <object> 标签内的内容相似。使用这两个标签将导致所有主要浏览器能够正确互操作你的内容。

让我们在工作示例中看看我们的代码。如果我们有一个名为 boats.mov 的视频，已保存为 320 x 240 比例的 .mov 文件，并且该文件位于名为 videos 的目录中，该目录位于我们的 Joomla! 文件结构的根级别（与 images 目录位于同一顶层）。嵌入该视频的代码将如下所示：

```
<object width="320" height="240"
    classid="clsid:02BF25D5-8C17-4B23-BC80-D3488ABDDC6B"
    codebase="http://www.apple.com/qtactivex/qtplugin.cab">
<param name="src" value="/videos/boats.mov">
<param name="controller" value="true">
<param name="autoplay" value="false">
<embed src="/videos/boats.mov" width="320" height="240"
    autoplay="false" controller="true"
    pluginspage="http://www.apple.com/quicktime/download/">
</embed>
</object>
```

可以在 <object> 标签内存在许多参数，太多无法在本手册中涵盖，但前面列出的参数是常见参数。有关更详细参数的列表，请访问 W3 Schools 网站：http://www.w3schools.com/TAGS/tag_object.asp。

## 使用文章插件包含视频

现在我们已经完成了使用原始 HTML 代码将视频包含到 Joomla! 文章中的艰苦工作，我们将看看 Joomla! 视频扩展包含视频的强大功能。

Joomla! 视频扩展为核心 CMS 提供了多样化的扩展功能。一旦插件安装并配置好，添加视频可以是一个简单的几步点击过程。Joomla! 视频扩展将在本章后面重点介绍，但现在我们将看看它们对管理员有多强大。

一个扩展的例子是 Flow Player：

![](img/0151410972a5c5ea0aab26416e44b1da_186_0.png)

Flow Player 是一个 GPL 许可的网页视频播放器。Flow Player 利用基于 Flash 的播放器界面，将众多视频格式流式传输到你的 HTML 页面中。它是一个相当先进的工具，具有带宽等先进的流媒体技术。

Flow Player for Joomla! 已经被开发出来，利用了这个精彩的播放器，并使其在你的 Joomla! 网站中易于使用。有可用的插件版本，允许你在文章中播放媒体，以及模块版本，可以放置在任何模块位置。

要获取 Flow Player for Joomla!，请访问 http://extensions.joomla.org/ 并搜索“Flow Player Plugin”。一旦你将此扩展下载到你的计算机，我们就可以开始了！

> 在将任何第三方代码安装到你的 Joomla! 网站之前，请确保在安装扩展之前备份你的网站数据库和文件集。有一些优秀的备份扩展，如 JoomlaPack 和 JoomlaCloner。一旦配置好，它们可以帮助你只需点击几下即可轻松备份你的 Joomla! 网站。

登录你的网站管理并点击 Extensions | Extension Manager。

使用选择文件上传按钮浏览你的本地计算机并找到 plg_flowplayer.zip 文件。选择后，使用上传文件并安装按钮将此扩展安装到你的 Joomla! 网站。

![](img/0151410972a5c5ea0aab26416e44b1da_187_0.png)

成功安装后，你应该前往 Extensions | Plugin Manager 以配置新插件。此特定插件被归类为内容插件，因此你将寻找类似 Content - FlowPlayer 的内容。点击文本以加载插件编辑页面。

![](img/0151410972a5c5ea0aab26416e44b1da_187_1.png)





## 创建视频播客

为 Joomla! 创建视频播客可以让您向众多用户展示和推广您的网站或产品。由于播客的特性，只要制作得当，用户甚至无需访问您的网站即可观看视频播客。

对我们来说，为包含视频文章的分类创建一个常规 RSS 源很容易。这将为我们提供一个 RSS 源，通知订阅者我们网站上有新内容。遗憾的是，Joomla! 的 RSS 源功能有限，这种方法无法将视频内容封闭在 RSS 源内。因此，我们的用户如果不回到网站，就无法下载视频文件进行观看。

![](img/0151410972a5c5ea0aab26416e44b1da_189_0.png)

创建一个真正的视频播客需要将视频内容封装在 RSS 附件（enclosures）中，RSS 附件是一种将多媒体内容附加到 RSS 源的方式。由于 Joomla! 无法提供此功能，我们需要使用第5章“在您的 Joomla! 网站中使用音频”中提到的方法。

外部 RSS 源创建服务“FeedBurner”可以生成能够包含我们多媒体内容的 RSS 源。有关该过程的更多详细信息，请参阅第5章的相关主题。以下是使用 FeedBurner 服务创建视频播客的简要步骤：

- 1. 选择并准备您的视频内容。一开始建议您手动插入视频代码，因为某些视频扩展插件在此过程中可能有效，也可能无效。
- 2. 创建一篇视频文章，并将其放入指定的“视频”分类中。
- 3. 创建一个“链接到分类”菜单链接，以便 Joomla! 可以生成该分类的 RSS 源。
- 4. 在前端查看 Joomla! 菜单链接，并复制 RSS 源 URL。

![](img/0151410972a5c5ea0aab26416e44b1da_189_1.png)

- 5. 访问 http://feedburner.google.com ，将 URL 粘贴到源创建框中，并确保您勾选了“我是播客制作者”复选框。
- 6. 按照 FeedBurner 的其余流程操作，为您的源指定标题和地址，并确保选择“从链接创建播客附件：任意富媒体文件”。
- 7. 从 FeedBurner 获取您新的视频播客源，然后大功告成！

因此，我们在这里所做的是利用 FeedBurner 的强大功能，基于 Joomla! 生成的 RSS 源创建了一个增强版的 RSS 源。

一旦您的 FeedBurner 源成功创建并测试通过，您就可以在同一个 Joomla! 分类中添加新的视频文章，这些文章将自动添加到视频播客源中。

随着大量屏幕捕获工具和视频编辑程序的出现，利用视频来传播您的信息是很容易实现的。由于视频是最接近真实生活的媒体体验之一，使用它可以让您的网站访客获取信息并得到娱乐，而且可能让您比竞争对手领先一步。

## 第三方视频扩展

正是使用 Joomla! 的视频扩展为我们提供了一些非常强大的视频功能。以至于 Joomla! 的扩展范围从将简单视频放入内容文章，一直到提供迷你 YouTube 风格的视频分发网站。

本节列举了一些为 Joomla! 开发的功能强大、广受欢迎、非商业性的第三方视频扩展。也有许多商业解决方案；其中一些列在 Joomla! 扩展目录中。

### AllVideos

AllVideos 是一个适用于 Joomla! 的一体化媒体解决方案。AllVideos 是一个插件，可以轻松嵌入来自 YouTube、Vimeo 等众多分发网站的视频。此外，它还可以直接播放来自您自己的服务器或远程服务器上的几乎所有视频/音频文件类型。

![](img/0151410972a5c5ea0aab26416e44b1da_191_0.png)

AllVideos 具有以下功能：

- 1. 支持数十个视频分发网站。例如 YouTube、Google Videos、Vimeo、Metacafe、Brightcove、Dailymotion、Revver、Spike 等！
- 2. 您可以流式播放自己常用的视频格式，包括 `.flv`、`.swf`、`.mp4`、`.mov`、`.wmv`、`.wma`、`.mp3`、`.3gp`、`.divx`。
- 3. 您可以从自己的服务器或远程服务器嵌入视频内容。
- 4. 使用简单。设置插件参数，使插件发布，即可从您的网站流式播放视频内容。
- 5. 您可以使用可换皮肤的模板系统自定义此插件的输出显示。

值得一提的是，该扩展已有一个开发分支，名为 AllVideos Reloaded。这两个扩展都很受欢迎，并持续开发，各自拥有一系列功能。请在 http://extensions.joomla.org 上查看它们（列在多媒体类别下，或搜索“AllVideos”），以查看对比。

### hwdVideoShare

hwdVideoShare 是一个视频分享画廊扩展，由一个组件和一个插件组成。该扩展的目的是为您提供视频画廊和分发网站（如 YouTube）的功能。

![](img/0151410972a5c5ea0aab26416e44b1da_192_0.png)

该组件需要 FFMPEG、MENCODER 和 FLVTOOL2 才能运行，并支持 MPG、MPEG、AVI、DIVX、MP4、FLV、WMV、RM、MOV、MOOV、ASF、SWF 和 VOB 视频格式。

还支持来自 YouTube 和 Google Videos 等第三方来源的视频，并与上传的媒体无缝协作，用户可以评分、评论、举报以及将视频添加到收藏夹。

该软件使用 Joomla! 插件管理器来支持各种附加插件：

- 模板插件可用于使用自定义主题为画廊设置样式
- 语言插件可用于翻译该软件
- 视频播放器插件可用于更换视频播放器
- 第三方视频插件可用于从热门视频网站导入视频
- 系统插件可用于在特定视频事件上触发有用的任务

该扩展还与其他产品的开发人员集成，提供更多功能。要下载此扩展，请访问 `http://extensions.joomla.org` 并搜索“hwdVideoShare”。

### Simple Flash Video Player

Simple Flash Video Player 是一个适用于 Joomla! 1.5 的模块，基于 JW 播放器。它易于使用，并具有以下功能：

- 播放来自 URL 的单个视频文件，或播放来自播放列表的无限视频文件
- 播放 YouTube 视频
- 支持 Adobe Flash Player 可以处理的任何格式（FLV、MP4、MP3、AAC、JPG、PNG 和 GIF）

![](img/0151410972a5c5ea0aab26416e44b1da_193_0.png)

该模块包含以下可调参数：

- 更改播放器设置
- 视频文件属性
- 播放列表设置
- 显示预览图像
- 在影片加载时插入您自己的徽标
- 外部页面的 URL
- 自动开始、重复和随机播放设置

要下载 Simple Flash Video Player 模块，请访问 `http://extensions.joomla.org` 并搜索“Flash Video”。

### RokBox

RokBox 是一个由 MooTools 驱动的 JavaScript 幻灯片和多媒体插件，以一种引人注目的 lightbox 类型弹出效果显示您的内容。

RokBox 可以显示多种媒体格式，包括图像、视频、来自分享网站的视频和音频。它是一种新型插件，可以开箱即用地提供多种媒体内容功能。

![](img/0151410972a5c5ea0aab26416e44b1da_194_0.png)

RokBox 带有两个默认主题，但其设计使您可以根据站点设计定制主题，为您的站点媒体创建定制的多媒体显示。

功能包括：

- 兼容 MooTools v1.11
- 四种可自定义的转场类型：Fade、QuickSilver、Growl、Explode。
- 支持 PDF、图像、FLV、QuickTime（.mov、.m4v）、Windows Media 视频（.wmv）、Flash（.swf）、YouTube、DailyMotion、Metacafe、Google Videos、Vimeo、音频（.mp3、.wav、.m4a）、本地和远程站点
- 支持相册/分类





- 能够以像素或百分比弹出 RokBox，并在相对于浏览器窗口的全屏模式下显示
- 支持字幕，包括标题和描述的支持
- 提供两种预定义主题：浅色主题和深色主题
- 支持自定义主题，能够按主题自定义样式和 RokBox 配置
- 在对本地图像使用 RokBox 时能够自动生成缩略图
- 支持从你的 Joomla! 内容生成缩略图或内联链接

RokBox 是一个很棒的插件。其简洁性、美观以及媒体类型的多样性，使其成为你网站的一个非常有用的扩展。

要下载 RokBox，请访问 http://extensions.joomla.org 并搜索 "RokBox"。

## Shadowbox

与 RokBox 类似，Shadowbox 是一个在线媒体查看器应用程序，支持网络上最流行的媒体发布格式。

Shadowbox 完全用 JavaScript 和 CSS 编写，并且高度可定制。使用 Shadowbox，网站作者可以在所有主要浏览器中展示各种媒体，而无需将用户导航离开链接页面。

![](img/0151410972a5c5ea0aab26416e44b1da_195_0.png)

Shadowbox 可以支持图像、音频和视频。可用的视频格式包括：

- QuickTime
- Flash
- Windows Media

你可以使用 Shadowbox 在文章和模块中的链接中显示媒体，因为正是超链接中的 rel 属性激活了该插件。这意味着你可以在任何你想要的超链接上使用 Shadowbox 功能：

```
<a href="/myimage.jpg" rel="shadowbox">My Image</a>
```

与 RokBox 一样，Shadowbox 是你 Joomla! 网站的多用途媒体插件，因此它可能就是你多媒体需求所需的一切。该扩展可以在 http://extensions.joomla.org 上通过搜索 "Shadow" 下载。

还有许多其他视频扩展和解决方案可用于 Joomla!。其中许多提供了强大的功能，但属于商业许可，未在本节中涵盖。如果你需要特定的 Joomla! 视频功能，在 http://extensions.joomla.org 上搜索或通过流行的搜索引擎搜索将为你提供此处列出的额外解决方案。

## 浏览器支持和视频可访问性

视频是娱乐和向网站访问者传递信息的绝佳工具，现在在网络上比以往任何时候都更常见。

乍一看，似乎视频并不是在网页中加载时在可访问性方面理想的媒体。然而，如果使用正确，视频实际上可以通过成为向网站用户传递音频和视觉内容的额外手段来增强网页的可访问性。由于这些结合的媒介，视频正成为显著增强网页可访问性的有用设备。

问题在于，当没有提供正确的文本替代时就会出现问题，这使得页面的视频内容对某些受众无法访问。例如，没有音频内容的视频对视力障碍者来说是无法访问的，或者对于既视力又听力受损的人来说，完全没有用处。

文本等价物应尽可能始终提供。例如，如果你的视频是关于“如何使用我们的产品”，请确保提供视频内容的文本描述，要么在同一页面上，要么作为链接到另一个区域。

## 良好实践技术

以下是使用 Joomla! 页面中的视频时的一些良好实践技术。

### 文本转录

为你在网页上所做的一切提供文本转录是良好的做法。例如，图像的 alt 标签提供了替代文本的文本转录，允许用户知道该页面区域正在加载关于该主题的图像。

如果你的视频是关于演讲或新闻，提供该内容的文本转录并通过超链接使其可访问到另一个区域，或更好的是，在同一页面的某个位置提供。文本转录可以从几个词长到数百个词；你的良好判断对于准确描述视频内容将非常重要。

### 替代内容

就像图像一样，包含视频到你网页中的 HTML 也可包含替代内容。object 元素可用于包含各种类型的信息，远不止图像标签 `<img />`。

一个例子是，如果你想在网页上提供 Windows .wmv 视频，那么使用 Apple Mac 的用户将无法观看，除非他们已下载适用于 MAC 的 Windows Media Player。使用 object 元素，你可以指定如果用户无法播放 .wmv，则应加载该电影的 MPEG 版本或替代格式。如果 MPEG 电影不受支持，你可以指定改为提供文本转录。

替代内容不仅可以帮助使用文本浏览器和屏幕阅读器的人，还可以被搜索引擎获取。

### 字幕

视频内容上的字幕对听力受损者或电脑上没有扬声器的人有用。字幕可能并不总是合适。然而，对于新闻或培训类视频内容，字幕通常只会增强视频内容。

## 视频控件

提供用于操纵视频播放的控件是很好的做法，现在几乎已成为一种期望。视频控件让你的访客能够控制视频的播放节奏、暂停和重看视频，以及打开和关闭音频等等：

![](img/0151410972a5c5ea0aab26416e44b1da_198_0.png)

播放条、暂停和音频按钮等简单功能都能使这段视频比没有控制功能的视频更易于访问。

网络浏览器变得越来越复杂，内置了强大的多媒体功能。在过去的日子里，访问一个网页然后不得不去下载浏览器插件才能查看该页面上显示的多媒体内容的情况并不少见。

这些网络浏览器插件是可选软件，可增强或为网络浏览器添加特定功能。无法播放你特定视频格式的用户需要为其浏览器下载插件才能播放该视频格式。

一些最受欢迎的视频浏览器插件是：

- Adobe Flash Player
- Apple QuickTime Player
- Real Media Player
- Windows Media Player

“在网络上使用哪种视频格式最好？”这个问题的问题在于，没有一种格式对每个人都是 100% 兼容的。诀窍是选择一种适合你大部分网站访问者的格式，然后尝试以其他方式为其他用户提供服务。

根据众多研究，Adobe Flash 现在是网络上显示视频最流行和最广泛的格式之一。几乎所有现代网络浏览器现在都已安装 Flash，统计数据显示超过 98% 的个人电脑可以播放 Flash 文件。

如果你选择不使用 Flash，我建议将你的电影保存为 MPEG-4 格式 .mp4。这些也将可被大多数用户在许多设备上播放。

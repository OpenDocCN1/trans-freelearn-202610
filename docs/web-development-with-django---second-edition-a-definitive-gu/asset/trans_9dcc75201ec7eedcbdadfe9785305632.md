

**图 16.19 – 已完成的评论预览**

在截图中，页面已向下滚动以显示 Previous/Next 按钮。请注意 Previous 按钮处于禁用状态，因为我们处于第一页。

如果点击 **Next**，你应该会看到下一页的评论。如果点击 **Next** 足够多次（取决于你有多少评论），你最终会到达最后一页，然后 **Next** 按钮将被禁用：

![](img/4840a43efd335abd10b93708225692d6_739_0.png)

**图 16.20 – Next 按钮被禁用**

如果没有评论，则应看到 **No reviews to display** 消息：

**图 16.21 – No reviews to display 文本**

在页面加载评论时，你应该会看到 Loading... 文本；不过它可能只会显示一瞬间，因为数据是从你自己的计算机加载的：

**图 16.22 – Loading... 文本**

**注意**

此活动的解决方案可以在 `https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions` 找到。

## 摘要

在本章中，我们介绍了 JavaScript 框架并描述了它们如何与 Django 协同工作以增强模板并添加交互性。我们介绍了 JavaScript 语言并涵盖了一些主要特性、变量类型和类。然后我们介绍了 React 背后的概念以及它如何使用组件构建 HTML。我们使用纯 JavaScript 和 `React.createElement` 函数构建了一个 React 组件。之后，我们介绍了 JSX 并看到它如何通过让你直接在 React 组件中编写 HTML 使组件的开发变得更容易。介绍了 promise 和 `fetch` 函数的概念，我们看到了如何使用 `fetch` 从 REST API 获取数据。本章以一个练习结束，该练习使用 REST API 从 Books 中检索评论并以交互式组件的形式渲染到页面上。

在下一章中，我们将了解如何将 Django 项目部署到生产 Web 服务器。

## 索引

### A

- **Add user page**
  - creating 152
- **Admin app login template**
  - repurposing 438–440
- **admin files**
  - discovering, in Django 472, 473
- **admin module** 473
- **admin site**
  - custom views, adding 488–491
  - custom views, restricting 487
  - templates, customizing 481, 482
  - text, customizing with AdminSite attributes 480, 481
  - views, adding 485
- **admin Site**
  - customizing 472
- **AdminSite**
  - class 473, 474
  - object, customizing 183–187
  - object, examining from Python shell 179
  - subclassing 180–183
- **AdminSite attributes**
  - reference link 481
  - used, for customizing admin site text 480, 481
- **AdminSite class** 473, 474
- **Angular**
  - reference link 680
- **API endpoint**
  - creating, for top contributors page 544, 545
- **API view**
  - creating 536–539
- **application programming interface (API)** 531, 603
- **Argon2** 441
- **arrays** 684
- **Arrow Functions** 687
- **assertions** 594
  - types 597
  - utilizing 594, 595
- **authentication**
  - implementing 550
  - initiating, with django-allauth 673
  - token-based authentication 551
- **authentication application** 668
- **authentication app’s templates**
  - implementing 433–437
- **authentication app’s views**
  - implementing 433–437
- **authentication-based content**
  - conditional blocks, using in templates 452, 453
- **authentication data**
  - used, for enhancing templates 450
- **authentication decorators** 444-447
  - adding, to views 447-450
- **authentication provider** 667
- **authentication redirection** 444-447
- **authentication schemes**
  - reference link 551
- **automated test cases** 592
- **automation testing** 592
  - functional testing 593
  - integration testing 593
  - regression testing 593
  - smoke testing 593
  - unit testing 593

### B

- **Babel** 698-700
  - reference link 697
- **Backbone.js**
  - reference link 680
- **base template**
  - login links, toggling 450-452
  - logout links, toggling 450-452
- **basic authentication** 551
- **BCrypt/SHA256** 441
- **binary file formats**
  - for data exports 569
- **book catalog**
  - creating, with CBV 517-522
- **book cover**
  - displaying 425, 426
- **Bookr**
  - custom admin site, creating 475-478
  - models and views, testing 618
  - most recent reviews, fetching on 713-718
- **Bookr admin site**
  - logout template, customizing 483, 484
- **Bookr APIs**
  - used, for implementing token-based authentication 551-556
- **Bookr application** 1
- **Book Search page**
  - session storage, using 467, 468
- **book search scaffold** 61, 62
- **Book Search view**
  - implementing, with Django Form 314-318
- **Bootstrap** 140
  - using, for template styling 140, 141
- **browsers’ sent values**
  - security and trust 379-381
- **built-in search**
  - using, to build custom admin dashboard 492, 493
- **bulk create operations** 115
  - multiple records, creating 116
- **bulk update operations** 115
  - used, for updating multiple records 117

### C

- **cache invalidation**
  - file serving 243-245
- **Cascading Style Sheets (CSS)** 140, 211
- **class-based API views**
  - creating 541-543
- **class-based views (CBVs)** 124, 125, 515, 516, 539, 540
  - in-built views 516
  - testing 615
  - used, for creating book catalog 517-522
  - used, for performing CRUD operations 523
- **classes, JavaScript** 686, 687
- **code**
  - simplifying, with ViewSets 545
- **code debugger** 56-60
- **collectstatic management command**
  - static file, collecting for production 235, 236
  - using 233, 234
- **comma-separated values (CSV)** 120
- **comments** 135
- **common template variables**
  - accessing 486
- **complex lookups**
  - performing, with Q objects 117
- **complex query**
  - used, for performing Q objects 118
- **components**
  - building, in React 689-693
- **conditional blocks**
  - using, for authentication-based content in templates 452, 453
- **constants** 684
- **content delivery network (CDN)** 222, 366, 689
- **context processor** 371, 372
- **create, read, update, and delete (CRUD)** 65
- **crispy filter** 659, 660
- **crispy template Tag** 660-662
- **Cross-Site Request Forgery (CSRF)** 272, 431, 520
  - security 272-275
- **CRUD operations**
  - Django admin app, using for 151, 152
- **CRUD operations, performing**
  - with CBVs 523
- **Create view** 523, 524
- **Delete view** 526-528
- **Read view** 524
- **Update view** 525, 526
- **CRUD operations, with Django admin app**
  - Add user page 152, 153
  - delete 158, 159
  - retrieve 154, 155
  - update 156, 157
  - user account, creating 154
- **CSS enhancements** 254-256
- **CSV file**
  - data, reading 566-568
  - data, reading from 558, 559
  - data, writing 566-568
  - generating, with Python’s csv module 563-566
  - Python, used for writing to 562, 563
  - reading, with Python 559-562
- **CSV files, in Python**
  - working with 558
- **custom admin dashboard**
  - building, with built-in search 492, 493
- **custom admin site**
  - creating, for Bookr 475-478
- **custom filter**
  - function, implementing 499
  - using, in templates 499
- **custom inclusion tag**
  - building 512-515
- **custom simple tag**
  - creating 507-509
  - template context, passing 509, 510
- **custom template filter** 497, 498
  - creating 498, 500-503
- **custom view**
  - adding, to admin site 488-491
  - restricting, to admin site 487
  - URLs, mapping 486, 487

### D

- **data**
  - reading, from CSV file 558, 559
  - storing, in sessions 461, 462
- **database records**
  - populating 120, 121
- **databases**
  - using 66
- **data exports**
  - binary file formats 569
- **date_hierarchy filters**
  - adding 196-199
- **date list_filter**
  - adding 196-199
- **debugger** 55, 56
- **decoupled architecture** 532
- **default admin site**
  - overriding 478-480
- **delete operation** 75
- **directory**
  - setting up, to store template filters 498
- **Django** 472, 679
  - admin files, discovering 472, 473
  - AdminSite class 473, 474
  - password storage 441
  - plotly, integrating with 583
  - read by user books, exporting as XLSX file 589
  - used, for serving uploaded files 397, 398
  - users and groups, adding through admin app 160-165
  - users and groups, managing 159, 160
  - users and groups, modifying through admin app 160-165
  - user's reading history, visualizing on user's profile page 583-588
  - views 515
  - visualizations, integrating with 583
- **Django admin app**
  - interface, customizing 177
  - using, for CRUD operations 151, 152
- **django-allauth** 667-670
  - authentication, initiating with 673
  - features 673
  - GitHub auth setup 673
  - Google auth setup 673
  - installing, with pip3 670, 671
  - reference link 673
  - setting up 671, 672
  - SocialAccount 670
  - SocialApplication 670
  - SocialApplicationToken 670
- **Django app** 3, 4, 19-21, 77
  - creating 4-6
- **django-configurations** 625-627
  - configuring, from environment variables 628, 629
  - installing, with pip 629
  - manage.py, modifying 627
  - setting up 629-632
- **Django Cookie Consent**
  - reference link 455
- **Django Cookie Law**
  - reference link 455
- **django-crispy-forms** 658
  - crispy filter 659, 660
  - crispy template Tag 660-662
  - installing, with pip3 658
  - using, with SearchForm 662-666
- **Django Debug Toolbar** 638-655
  - installing, with pip3 656
  - setting up 655-658
  - usage scenarios 638, 639
- **Django development server** 3, 19


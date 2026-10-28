

在此过程中，我们首先从Django的admin模块中导入了AdminConfig类。该类用于定义哪个应用应被用作默认管理站点，并覆盖Django管理站点的默认行为。

针对我们的用例，我们创建了一个名为BookrAdminConfig的类，它是Django的AdminConfig类的子类，并覆盖了default_site属性，使其指向我们的自定义管理站点BookrAdmin类：

```
default_site = 'bookr_admin.admin.BookrAdmin'
```

完成此操作后，我们需要在Bookr项目中将我们的应用设置为管理应用。为此，打开Bookr项目的settings.py文件，在INSTALLED_APPS部分，将'reviews.apps.ReviewsAdminConfig'替换为'bookr_admin.apps.BookrAdminConfig'。settings.py文件应如下所示：https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter10/Exercise10.02/bookr/settings.py.

3. 将应用映射为管理应用后，最后一步涉及修改URL映射，使'admin/'端点使用admin.site属性来查找正确的URL。为此，打开bookr项目下的urls.py文件。请查看urlpatterns列表中的以下条目：

```
path('admin/', admin_site.urls)
```

将其替换为以下条目：

```
from django.contrib import admin

urlpatterns = [
    ...
    path('admin/', admin.site.urls)
]
```

请记住，admin_site.urls是一个模块，而admin.site是Django的内部属性。

完成上述步骤后，让我们重新加载Web服务器，并通过访问http://localhost:8000/admin来检查管理站点是否加载。如果加载的网站如下图所示，那么我们现在就有了自己的自定义管理应用用于管理界面：

![](img/4840a43efd335abd10b93708225692d6_502_0.png)

图10.5——自定义Bookr管理站点的主页视图

如你所见，一旦我们用自定义管理应用覆盖admin.site，之前使用admin.site.register属性注册的模型就会自动显示出来。

这样一来，我们现在有了一个自定义基础模板，可以利用它来构建Django管理站点其余的自定义功能。随着本章的深入，我们将发现一些有趣的自定义方法，使管理仪表盘成为我们应用程序的集成部分。

## 使用AdminSite属性自定义管理站点文本

就像我们可以使用admin.site属性来自定义Django应用程序的文本一样，我们也可以使用AdminSite类公开的属性来定制这些文本。在练习10.02——覆盖默认管理站点中，我们了解了如何更新管理站点的site_header属性。类似地，还有许多其他属性可以修改。下面描述了一些可以被覆盖的属性：

- site_header：在管理站点每个页面顶部显示的文本（默认为“Django Administration”）。
- site_title：在浏览器标题栏中显示的文本（默认为“Django Admin Site”）。
- site_url：用于“查看站点”选项的链接（默认为/）。当站点运行在自定义路径上时，该属性会被覆盖，重定向应直接将用户带到子路径。
- index_title：应显示在管理应用程序索引页面上的文本（默认为“Site administration”）。

> 注意
> 有关所有AdminSite属性的更多信息，请参阅官方Django文档：https://docs.djangoproject.com/en/4.1/ref/contrib/admin/#adminsite-attributes.

如果我们想在自定义管理站点中覆盖这些属性，过程非常简单，如下所示：

```
class MyAdminSite(admin.AdminSite):
    site_header = "My web application"
    site_title = "My Django Web application"
    index_title = "Administration Panel"
```

正如我们到目前为止在示例中所看到的，我们为Bookr创建了一个自定义管理应用程序，然后将其设为项目的默认管理站点。一个有趣的问题是，既然我们迄今为止所自定义的属性也可以直接使用admin.site对象进行自定义，那么与修改admin.site属性相比，什么时候应该创建自定义管理应用程序呢？

事实证明，有人会选择自定义管理站点可能有多种原因——例如，他们想要更改默认管理站点的布局，使其与应用程序的整体布局保持一致。当你为企业创建Web应用程序时，这一点非常常见，因为内容的同质性非常重要。以下是一小段需求列表，这些需求可能会促使开发人员继续构建自定义管理站点，而不是简单地修改admin.site变量的属性：

- 需要覆盖管理界面的索引模板
- 需要覆盖登录或退出模板
- 需要向管理界面添加自定义视图

## 自定义管理站点模板

与site_header和site_title等出现在管理站点中的一些可自定义公共文本一样，Django也允许我们通过设置AdminSite类中的某些属性来自定义用于渲染管理站点不同页面的模板。

这些自定义可以包括修改用于渲染索引页面、登录页面、模型数据页面等的模板。利用Django提供的模板系统，可以轻松完成这些自定义。例如，以下代码片段展示了如何向Django管理仪表盘添加新模板：

```
{% extends "admin/base_site.html" %}

{% block content %}
<!-- Template Content -->
{% endblock %}
```

在这个自定义模板中，有几个重要方面需要我们理解。

在通过修改仪表盘内某些页面的外观或向仪表盘添加一组新页面来定制现有Django管理仪表盘时，开发人员可能不想从头重新编写每一段HTML，以保持Django管理仪表盘的基本外观和感觉。

通常，在自定义管理仪表盘时，我们希望保留Django组织仪表盘上显示的不同元素的布局，这样我们就可以专注于修改页面中对我们重要的部分。页面的基本布局以及页眉和页脚等常见页面元素都定义在Django管理站点的基础模板中，该模板也是默认Django管理网站内所有页面的母版模板。

当我们想保留Django管理页面中公共元素的组织和渲染方式时，我们需要扩展这个基础模板，以便我们的自定义模板页面能提供与Django管理仪表盘内其他页面一致的用户体验。这可以通过使用模板扩展标签并扩展Django提供的admin模块中的base_site.html模板来实现：

```
{% extends "admin/base_site.html" %}
```

完成此操作后，下一个任务是为自定义模板定义我们自己的内容。Django提供的base_site.html模板为开发人员提供了一个基于块的占位符，以便他们为模板添加自己的内容。要添加此内容，开发人员必须将其自定义页面元素的逻辑放在{% block content %}标签内。这实质上覆盖了base_site.html模板中由{% block content %}标签定义的任何内容，遵循了Django中的模板继承概念。

现在，让我们看看如何自定义用于渲染退出页面的模板，即当用户在管理面板中单击“退出”（Logout）按钮时。

## 练习10.03——为Bookr管理站点自定义退出模板

在本练习中，您将自定义用于渲染退出页面的模板，即当用户在管理站点上单击“退出”按钮时。当为银行用户开发的网站可能希望在用户点击“退出”后将其重定向到说明页面，以向他们显示说明，确保其银行会话安全关闭时，这种覆盖会非常有用：

- 1. 在您应该已在前面章节中创建的templates目录下，再创建一个名为admin的目录，该目录将用于存储自定义管理站点的模板。

> 注意
> 在继续之前，请确保templates目录已添加到settings.py文件（位于bookr/project下）的DIRS列表中。

- 2. 现在，目录结构设置已完成，Django也已配置为加载模板，下一步涉及编写您想要渲染的自定义退出模板。为此，让我们在步骤1中创建的templates/admin目录下创建一个名为logout.html的新文件，并在其中添加以下内容：

```
{% extends "admin/base_site.html" %}

{% block content %}
<p>You have been logged out from the Admin panel.</p>
<p><a href="{% url 'admin:index' %}">Login Again</a>
or <a href="{{ site_url }}">Go to Home Page</a></p>
{% endblock %}
```

在上述代码片段中，我们做了几件事。首先，对于我们的自定义退出模板，我们将使用与django.contrib.admin模块所提供的相同的母版布局。因此，请考虑以下内容：

```
{% extends "admin/base_site.html" %}
```

当我们编写此代码时，Django会尝试在django.contrib.admin模块提供的templates目录中查找并加载admin/base_site.html模板。

现在，我们的基础模板已经准备好被扩展，接下来我们要做的就是尝试通过以下代码块来覆盖内容块的HTML：

```
{% block content %}
...
{% endblock %}
```


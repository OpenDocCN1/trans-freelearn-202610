
现在让我们详细讨论图 24.3：

-   **Components**：这些由 JSP 页面以及这些 JSP 页面中的控件组成。组件是 WDK 框架中的可重用单元，执行诸如检出、检入、导出等 Docbase 操作。每个 WDK 组件由一个 XML 配置文件、一个关联的行为 Java 类以及至少一个 JSP 页面组成。
-   **Controls**：这些代表 UI 功能（按钮或链接），并提供标准 Web 功能和操作，例如验证、格式化等。控件可以在多个 JSP 页面中重用，并在 UI 表单中对这些元素进行更改时引发事件。
-   **Forms (JSP pages)**：WDK 将 JSP 页面建模为服务器上的 Form 对象，以实现两个目的：
    -   维护 HTML 表单的状态
    -   维护浏览器历史记录
-   **Actions**：操作要么由 Docbase 操作（例如 check-out）触发，要么通过 UI 表单元素（例如按钮、菜单项等）触发。操作将 UI 事件与应用程序功能关联起来。
-   **Events**：每当用户操作导致 UI 表单中元素状态发生变化时，就会引发事件。事件要么通过关联 JSP 页面中的 JavaScript 在客户端处理，要么由关联组件在服务器端处理。

其中一些术语现在可能还不是很清楚。然而，您可以查阅 Documentum WDK 开发指南以获得更好的理解。随着我们在本章中继续进行示例，这些概念肯定会变得更加清晰。

## 24.3 一个简单的 WDK 配置示例

不再浪费时间，让我们从一个非常简单的 WDK 配置示例开始。

我们将覆盖 Web Publisher 中的默认新建内容（属性）屏幕，并为新闻文章模板引入新属性。

### 24.3.1 配置更改前的新建内容屏幕

如果您使用模板类别在 Web Publisher 中创建新内容，则会向您显示新建内容（属性）屏幕以提供内容的属性（元数据）信息，例如：

-   内容名称（属性：`object_name`）
-   描述性名称（属性：`title`）
-   主题（属性：`subject`）
-   生效日期（属性：`a_effective_date`）
-   到期日期（属性：`a_expiration_date`）

图 24.4 展示了该默认屏幕在 Web Publisher 中的外观。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_389_0.png)

图 24.4：Web Publisher 中的默认新建内容屏幕

### 24.3.2 配置更改后的修改新建内容屏幕

图 24.5 显示了新闻文章模板的同一新建内容屏幕，其中包含新添加的属性。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_390_0.png)

图 24.5：为新闻文章新增属性后的修改新建内容屏幕

以下步骤将解释如何使用 WDK 框架在 Documentum 中实现此简单配置。

1.  从 Web Publisher 服务器中的以下位置复制新建内容组件的 XML 配置文件（newcontent_component.xml）：
    `\wp\config\library\newcontent.`
    请注意，本章所有示例中，我们将假设 Web Publisher 应用程序的名称为 wp。
    在 Web Publisher 服务器中 custom 文件夹内的 config 文件夹中创建以下目录：
    `\library\newcontent.`
    将复制的 newcontent_component.xml 放置在以下位置：
    `\wp\custom\config\library\newcontent.`
    我们将在下面的步骤中查看对复制的 newcontent_component.xml 文件需要做的更改。
2.  从 Web Publisher 服务器中的以下位置复制新建内容 JSP 文件（newContent.jsp）：`\wp\library\newcontent.`
    在 Web Publisher 服务器中 custom 文件夹内创建以下目录：
    `\library\newcontent.`
    将复制的 newContent.jsp 重命名为（例如）custom_newContent.jsp 并将其放置在以下位置：
    `\wp\custom\library\newcontent.`
    我们将在下面的步骤中查看对 custom_newContent.jsp 文件需要做的更改。
3.  按如下所示修改 `\wp\custom\config\library\newcontent` 下的 newcontent_component.xml 文件：
    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="no"?>
    <!------------------------------------------------------------------------------>
    <config version='1.0'>
        <scope wpcontext="wpview">
            <!-- the document list component definition -->
            <!-- <component id="newcontent"> -->
            <component id="newcontent"
            extends="newcontent:wp/config/library/newcontent/
                    newcontent_component.xml">
                <!-- Component Contract -->
                <params>
                    <param name="objectId" required="false"/>
    ```
    
    ```
    </params>
    <!-- Component Layout -->
    <pages>
    <!--<start>/wp/library/newcontent/newContent.jsp</start>-->
    <start>/custom/library/newcontent/custom_newContent.jsp</start>
    <return>/wp/library/componentReturn.jsp</return>
    </pages>
    
    <!-- Component Behavior -->
    <class>com.documentum.wp.library.newcontent.NewContent</class>
    <nlsbundle>com.documentum.wp.library.newcontent.NewContentNlsProp</nlsbundle>
    
    <!-- Component specific Configuration -->
    <!-- Allow the user to import a file from file system. Default is true. -->
    <allow_import>true</allow_import>
    
    <enable_full_text_index>true</enable_full_text_index>
    
    <!-- Component specific Configuration -->
    <helpcontextid>newcontent</helpcontextid>
    </component>
    </scope>
    </config>
    ```

    该 XML 文件的结构如下所示：

    ```
    <?xml version="1.0" encoding="UTF-8" standalone="no"?>
    ```

    这是标准的 XML 声明

    ```
    <config version='1.0'>
    ```

    这是 XML 配置文件的根元素。

    ```
    <scope wpcontext="wpview">
    ```

    这是配置文件的范围，定义了此配置定义有效的上下文。

    在标签 <scope wpcontext="wpview"> 中，wpcontext 是限定符名称，wpview 是限定符值。上下文匹配针对限定符值进行。限定符在相应 WDK 应用程序层的 app.xml 文件中指定。

    以下是 WDK、Webtop、Web Publisher 等各种 WDK 应用程序层提供的一些限定符：

    -   Docbase 名称
    -   Docbase 类型
    -   权限
    -   应用程序
    -   Web Publisher 上下文
    -   Web Publisher 角色

    如有必要，您甚至可以在 WDK 应用程序中引入自定义限定符。Documentum 根据限定符在应用程序的 app.xml 文件中指定的顺序解析范围。

    ```
    <component id="newcontent"
    ```

    这是 XML 配置文件中的主要元素。除了 component 之外，其他有效元素可以是 action（Action 组件）和 application（app.xml 文件）。id 属性使用应用程序中的唯一标识符标记组件/操作名称。您可以通过在主要元素中使用 extends 属性来扩展其他组件/操作/应用程序。

    ```
    <params>
        <param name="objectId" required="false"/>
    ```

    这些行定义从配置传递到关联行为类的参数。

    ```
    <pages>
    ```

    <pages> 定义了问题组件布局的关联 JSP 页面（页）。您可以看到我们已注释掉默认 newContent.jsp 的路径，而是提供了我们自定义 JSP 文件 custom_newContent.jsp 的路径。

    ```
    <class>com.documentum.wp.library.newcontent.NewContent</class>
    ```

    该行定义组件的关联行为 Java 类。在此特定配置示例中，由于我们只是修改新建内容组件的 xml 配置文件和 JSP 文件，因此我们没有扩展默认的 NewContent.class。

    ```
    <nlsbundle>com.documentum.wp.library.newcontent.NewContentNlsProp</nlsbundle>
    ```

    该行定义用于本地化组件字符串的关联 NLS 属性文件。请注意，在 WDK 框架中，从另一个组件扩展的组件会继承为基组件的 NLS 属性文件定义的字符串。

    其他 XML 元素所示的代码特定于 newcontent 组件，不适用于所有 XML 配置文件。

4.  按如下代码片段所示修改 \wp\custom\library\newcontent 下的 custom_newContent.jsp 文件。

    请注意，在 JSP 文件中，我们为新闻文章对象类型（cust_newsarticle）的以下自定义属性添加了第 137-151 行：cust_news_summary、cust_news_is_press_rel、cust_news_edit_approvers 和 cust_news_display_order。

    这些是 Docbase 属性，其行为由 WDK 框架默认处理。我们在 JSP 中引入这些新属性的方式与下面所示的 keywords 和 authors 等属性的默认实现类似（请参阅突出显示的行）。

    ```
    <dmfx:docbaseattribute object="<%=NewContent.CONTENT_OBJECT%>"
    attribute="keywords"
    pre="<tr><td align='right' valign='top'><b>"
    col1=":</b></td><td>&nbsp;</td><td>"/>
    <tr><td colspan='4' height='5'></td></tr>
    ```

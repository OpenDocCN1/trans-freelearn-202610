

## 示例应用

```html
editor=isEdited;
}
}
});
BookDatabase.getChapterContent(bookTitle,chapterTitle,{
    callback:function(chapterContent)
    {
        var editChapterArea=dwr.util.byId('editChapterArea');
        var html='<h3>'+chapterTitle+'</h3>';
        if(editor!="")
        {
            html+='Edited by '+editor+'<br/>';
        }
        html+='<textarea '+readonly+' id="chapterContentTextArea" rows="10" cols="60">'+chapterContent+'</textarea><br/>';
        if(readonly=="")
        {
            html+='<input type="button" value="Save" onclick="saveChapterContent(\''+bookTitle+'\',\''+chapterTitle+'\');">';
            html+='<input type="button" value="Cancel" onclick="cancelChapterContentEdit(\''+bookTitle+'\',\''+chapterTitle+'\');">';
        }
        else
        {
            html+='<input type="button" value="Cancel" onclick="closeChapterContentEdit();">';
        }
        editChapterArea.innerHTML=html;
    }
});
    return false;
}

function cancelChapterContentEdit(bookTitle,chapterTitle)
{
    BookDatabase.cancelChapterEdit(bookTitle,chapterTitle);
    var editChapterArea=dwr.util.byId('editChapterArea');
    editChapterArea.innerHTML='';
    return false;
}

function closeChapterContentEdit()
{
    var editChapterArea=dwr.util.byId('editChapterArea');
    editChapterArea.innerHTML='';
    return false;
}

function saveChapterContent(bookTitle,chapterTitle)
{
    var editChapterArea=dwr.util.byId('chapterContentTextArea');
    var content=editChapterArea.value;
    BookDatabase.saveChapterContent(bookTitle,chapterTitle,content,{
        callback:function()
        {
            setFeedback('Content saved.');
        }
    });
    return false;
}

function setFeedback(feedback)
{
    var feedbackArea=dwr.util.byId('feedbackArea');
    feedbackArea.innerHTML=(new Date())+"<br/>"+feedback;
}

function startNewBook()
{
    var bookTitle=prompt("Book title:");
    if(bookTitle!=null)
    {
        BookDatabase.addNewBook(bookTitle,refreshBookLists);
    }
    return false;
}

function addChapter(bookTitle)
{
    //dialog to ask book title
    var chapterTitle=prompt("Chapter name:");
    if(chapterTitle!=null)
    {
        BookDatabase.insertChapter(bookTitle,chapterTitle,{
            callback:function()
            {
                getChaptersForTitle(bookTitle);
            }
        });
    }
    return false;
}

function refreshBookLists()
{
    getBookLists();
}

function clearContentAreas()
{
    var area=dwr.util.byId('feedbackArea');
    area.innerHTML='';

    area=dwr.util.byId('bookTitleContentArea');
    area.innerHTML='';

    dwr.util.removeAllRows('bookChapters');

    area=dwr.util.byId('insertChapterArea');
    area.innerHTML='';

    area=dwr.util.byId('voteArea');
    area.innerHTML='';

    area=dwr.util.byId('editChapterArea');
    area.innerHTML='';
}

</script>

</head>
<body onload="showUsersOnline();">
<div id="maincontainer">

<div id="topsection">
<div class="innertube">
<h1>Book Authoring</h1>
<h4>Welcome <i><%= (String) session.getAttribute("username") %></i></h4>
</div>
</div>

<div id="contentwrapper">
<div id="contentcolumn">
<div id="bookContent" class="innertube">
<div id="feedbackArea"></div>
<div id="bookTitleContentArea"></div>
<table cellpadding="0" cellspacing="0">
    <tbody id="bookChapters">
    </tbody>
</table>

<div id="insertChapterArea"></div>
<div id="voteArea"></div>

<div id="editChapterArea"></div>

</div>
</div>
</div>

<div id="leftcolumn">
<div class="innertube">
<table cellpadding="0" cellspacing="0">
    <thead>
        <tr>
            <td><b>Books in process</b></td>
        </tr>
    </thead>
    <tbody id="booksInProgress">
    </tbody>
    <tfoot>
        <tr>
            <td><a href="#" onclick="startNewBook();">Start new book</a></td>
        </tr>
    </tfoot>
</table>

<table cellpadding="0" cellspacing="0">
    <thead>
        <tr>
            <td><b>Published books</b></td>
        </tr>
    </thead>
    <tbody id="publishedBooks">
    </tbody>
</table>

<table cellpadding="0" cellspacing="0">
    <thead>
        <tr>
            <td><b>Authors online</b></td>
        </tr>
    </thead>
    <tbody id="authorsOnline">
    </tbody>
</table>

<input id="logoutButton" type="button" value="Logout"
onclick="logout();return false;"></div>

</div>
<div id="footer">Stylesheet by <a
href="http://www.dynamicdrive.com/style/">Dynamic Drive CSS
Library</a></div>

</div>
<script type="text/javascript">
getBookLists();
</script>
</body>
</html>
```

由于所有客户端逻辑都包含在此 HTML 页面中的 JavaScript 函数中，因此这是一个相当大的页面。让我们浏览一下页面源代码。

在页面开始处，在声明 DWR 函数之前，有一段 Java 代码，其目的是检查页面请求是否有效。它是通过检查用户会话来完成的，如果从会话中找到用户名，则请求有效，如果从会话中找不到用户名，则向用户显示一个空屏幕。这是因为我们不希望用户直接访问主页面（在进一步开发时，可以在此处显示登录页面，而不是空页面）。

在检查用户会话后，我们加载 DWR 脚本。此示例应用程序中只有两个 Java 类，即 Login 和 BookDatabase。

在 script 元素开始处，我们为我们的使用启用了反向 AJAX（Comet）。前两个函数执行注销，并将用户重定向到登录屏幕，而第三个函数 showUsersOnline() 在屏幕左侧显示已登录用户。正如我们稍后会看到的，反向 AJAX 用于在新用户登录系统时调用每个用户的页面上的此函数。该函数还使用 DWR 实用程序函数用用户名填充表格。

getBookLists() 函数调用 BookDatabase Java 类并检索正在进行中的书籍列表和已发布的书籍列表。它在 HTML 页面的左侧导航中显示列表。对于正在进行的书籍，会创建一个打开章节页面的链接，对于已发布的书籍，会创建一个打开书籍页面的链接。

getChapters() 函数调用 getChaptersForTitle() 函数，后者调用 BookDatabase 类并获取所点击书籍的章节标题。该函数还为作者插入投票按钮，并显示投票的当前情况，并显示已投票作者的数量占总数的比例。

showBook() 函数从 BookDatabase 中检索渲染后的书籍并在 HTML 页面中显示它。

voteForPublish() 函数接收用户的投票并更新指定书籍的投票状态。refreshChapterIfVisible() 函数在书籍内容发生变化时更新用户的屏幕，而 clearChapterViewIfVisible() 函数在用户正在查看书籍且书籍被投票发布时清除书籍内容。

notifyBookPublished() 函数通知用户书籍已发布，并在书籍可见时清除书籍内容。

getChapterContent() 函数在可编辑文本区域中显示章节内容，用户可以在其中编辑章节。如果其他人当前正在编辑该章节，则向用户显示只读文本区域，并且该区域的内容是作者保存的最新内容。

cancelChapterContentEdit()、closeChapterContentEdit() 和 saveChapterContent() 函数对应于章节编辑页面中同名按钮。保存函数还调用 setFeedback() 函数以向用户显示保存事件的时间。

startNewBook() 和 addChapter() 函数用于添加书籍或书籍中的章节。refreshBookLists() 函数只是调用 getBookLists() 函数以获取用户界面中的当前书籍。最后一个函数 clearContentAreas() 清除书籍内容。

页面的其余部分是 HTML 代码，内容包括标题和占位符以及 JavaScript 函数使用的 div 元素。

## 配置 Web 应用程序

在进入 Java 代码之前，我们需要为我们的 Web 应用程序设置 DWR。我们将 dwr.jar 文件复制到 WEB-INF/lib 目录并按如下所示设置 web.xml 文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_
```


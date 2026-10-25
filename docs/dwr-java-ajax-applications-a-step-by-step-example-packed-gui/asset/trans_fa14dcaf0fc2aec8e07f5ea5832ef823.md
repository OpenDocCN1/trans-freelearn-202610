
```javascript
var contentFunctions='';
var scriptToBeEvaled='';
var contentHtml='';
for(var i=0;i<htmlArray.length;i++)
{
    var html=htmlArray[i];
    if(html.toLowerCase().indexOf('<script>')>-1)
    {
        if(html.indexOf('TO BE EVALED')>-1)
        {
            scriptToBeEvaled=html.substring(html.indexOf('>')+1,
                                            html.indexOf('</'));
        }
        else
        {
            eval(html.substring(html.indexOf('>')+1,html.indexOf('</')));
            contentFunctions+=html;
        }
    }
    else
    {
        contentHtml+=html;
    }
}

contentScriptArea=dwr.util.byId("contentAreaFunctions");
contentScriptArea.innerHTML=contentFunctions;
contentArea=dwr.util.byId("contentArea");
contentArea.innerHTML=contentHtml;
if(scriptToBeEvaled!='')
{
    eval(scriptToBeEvaled);
}
}
</script>
</head>
<body onload="loadMenuItems()">
<h1>DWR Easy Java Ajax Applications</h1>
<ul class="basictab" id="dwrMenu">
</ul>
<div id="contentAreaFunctions">
</div>
<div id="contentArea">
</div>
</body>
</html>
```

这段 JSP 是我们的用户界面。HTML 只是普通的 HTML，包含 head 元素和 body 元素。head 中引用了样式表以及 DWR JavaScript 文件，engine.js、util.js 和我们自己的 HorizontalMenu.js。util.js 文件是可选的，但由于它包含非常有用的函数，可以将其包含在所有使用 util.js 中函数的网页中。

body 元素在菜单下方有一个 contentArea 占位符，用于内容页面。它还包含特定内容的 JavaScript 函数的内容区域。body 元素的 onload 事件在页面加载时执行 loadMenuItems() 函数。loadMenuItems() 函数调用 HorizontalMenu Java 类的远程方法。HorizontalMenu.getMenuItems() JavaScript 函数的参数是回调函数，当 Java 方法执行完毕并返回菜单项时，DWR 会调用该回调。

setMenuItems() 函数是 loadMenuItems() 函数的回调，如上一段所述。在加载菜单项时，HorizontalMenu.getMenuItems() 远程方法以字符串列表的形式将菜单项作为参数返回给 setMenuItems() 函数。菜单项使用 menuItemFormatter() 辅助函数进行格式化。

menuItemFormatter() 函数创建菜单文本的 li 元素。菜单被格式化为链接，(a href)，并且它们具有 onclick 事件，该事件调用 getContent 函数，而 getContent 又调用 AppContent.getContent() 函数。

AppContent 是一个远程 Java 类，我们尚未实现它，其目的是根据用户点击的菜单项从文件中读取 HTML。AppContent 和内容页面的实现在下一节中描述。

我们的动态用户界面如下所示：

![](img/5b721cfaa97eb697efc92e3914f62fca_78_0.png)

注意浏览器屏幕底部的 Firebug 窗口。截图中的 Firebug 控制台显示了一次对我们的 HorizontalMenu.getMenuItems() 方法的 POST 请求。Firebug 的其他功能在开发工作中非常有用，我们发现 Firebug 在整个开发工作中都已启用，这很有用。

## 回调函数

我们第一次看到回调函数的参数是在 HorizontalMenu.getMenuItems(setMenuItems) 函数中，由于回调是 DWR 中的重要概念，在我们看到其首次使用后，现在进一步讨论它们会很有好处。

回调用于对从远程方法返回的数据进行操作。由于 DWR 和 AJAX 是异步的，RPC（远程过程调用）中典型的返回值，如 Java 调用中的返回值，并不适用。DWR 隐藏了调用回调函数的细节，并在我们从远程 Java 方法返回一个值到接收返回值的回调函数的整个过程中内部处理一切。

使用回调函数时推荐两种方法。

我们已经在 `HorizontalMenu.getMenuItems(setMenuItems)` 函数调用中看到了第一种方法。请记住，`getMenuItems()` Java 方法没有参数，但在 JavaScript 调用中，我们在参数列表末尾添加了回调函数的名称。如果 Java 方法有参数，则 JavaScript 调用类似于 `CountryDB.getCountries(selectedLetters,setCountryRows)`，其中 selectedLetters 是 Java 方法的输入参数，setCountryRows 是回调函数的名称（我们稍后会看到其实现）。

使用回调的第二种方法是远程 JavaScript 调用中的元数据对象。示例（完整实现将在本章后面显示）如下所示：

```
CountryDB.saveCountryNotes(ccode,newNotes, {
    callback:function(newNotes)
    {
        //function body here
    }
});
```

这里，函数是匿名的，其实现包含在对远程 Java 方法的 JavaScript 调用中。一个优点是代码易于阅读，并且在我们从 Java 方法获得返回值后立即执行代码。另一个优点是我们可以在调用中添加额外的选项。

额外的选项包括超时和错误处理程序，如以下示例所示：

```
CountryDB.saveCountryNotes(ccode,newNotes, {
    callback:function(newNotes)
    {
        //function body here
    },
    timeout:10000,
    errorHandler:function(errorMsg) { alert(errorMsg); }
});
```

也可以为那些不返回值的方法添加回调函数。将回调添加到没有返回值的 methods 在远程调用完成后获取通知时很有用。

## 后记

我们的第一个示例已准备就绪，它也是后续示例的基础。我们还查看了如何在 Eclipse 环境中测试应用程序。

使用 DWR，我们可以将浏览器上的 JavaScript 代码和服务器上的 Java 代码视为一体。可能需要一段时间才能习惯，但它将改变我们开发 Web 应用程序的方式。逻辑上，不再有客户端和服务器，而只是一个物理上分离的单一运行时平台。但在实践中，当然，使用 DWR、客户端 JavaScript 和服务器端 Java 的应用程序仍在使用典型的客户端-服务器交互。在逻辑上单一的运行时平台中编写应用程序时应记住这一点。

## 实现表格和列表

第一个实际示例在应用程序中非常常见：表格和列表。在此示例中，表格使用 DWR 实用函数和远程 Java 类进行填充。示例代码还展示了如何使用 DWR 进行内联表格编辑。当双击表格单元格时，会打开一个编辑框，用于保存新的单元格数据。

示例将具有国家数据的 CSV 文件：国家名称、正式名称、两位字母代码、首都和用户定义的备注。表格示例的用户界面如下所示：

![](img/5b721cfaa97eb697efc92e3914f62fca_80_0.png)

### 表格和列表的服务器代码

首先要做的是获取国家数据。国家数据位于 CSV 文件中（名为 `countries.csv`，位于 samples Java 包中）。以下是 CSV 文件内容的摘录（数据来自 `http://www.state.gov`）。

```
Short-form name,Long-form name,FIPS Code,Capital

Afghanistan,Islamic Republic of Afghanistan,AF,Kabul

Albania,Republic of Albania,AL,Tirana

Algeria,People's Democratic Republic of Algeria,AG,Algiers

Andorra,Principality of Andorra,AN,Andorra la Vella

Angola,Republic of Angola,AO,Luanda

Antigua and Barbuda,(no long-form name),AC,Saint John's

Argentina,Argentine Republic,AR,Buenos Aires

Armenia,Republic of Armenia,AM,Yerevan

...
```

每次客户端请求国家数据时都会读取 CSV 文件。虽然这不是非常高效，但在本文中已经足够好。其他替代方案包括内存缓存或真实数据库，如 Apache Derby 或 IBM DB2。作为示例，我们创建了一个 CountryDB 类，用于读取和写入国家 CSV。我们还有另一个类 DBUtils，它包含一些辅助方法。DBUtils 代码如下：

```
package samples;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
```

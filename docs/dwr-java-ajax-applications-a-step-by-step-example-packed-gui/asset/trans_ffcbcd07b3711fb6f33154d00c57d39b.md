

```
Collection<ScriptSession> sessions =
        serverContext.getScriptSessionsByPage
                (contextPath + "/mainpage.jsp");
ScriptProxy proxy = new ScriptProxy(sessions);
return proxy;
}
return null;
}

public void postNewMessage(String newMessage) {
    ScriptProxy proxy = getScriptProxyForSessions();
    if (proxy != null) {
        proxy.addFunctionCall("<newMessage>",newMessage);
    }
}
}
```

聊天室代码出人意料地简单。聊天内容存储在一个 `Vector` 的 `Strings` 中。`getChatContent()` 方法只是将聊天内容 `Vector` 返回给浏览器。

当用户发送新的聊天消息时，会调用 `postMessage()` 方法。该方法验证用户是否已登录，并将当前时间和用户名添加到聊天消息中，然后将消息附加到聊天内容中。

该方法还调用 `postNewMessage()` 方法，该方法用于向所有已登录用户显示新的聊天内容。注意 `postMessage()` 方法不返回任何值。我们让 DWR 和反向 AJAX 功能将聊天消息显示给所有用户，包括发送消息的用户。

`getScriptProxyForSessions()` 和 `postNewMessage()` 方法使用反向 AJAX 用新消息更新所有已登录用户的聊天区域。

就这样！聊天室示例非常直接，基本功能已经就位，应用程序已准备好进行进一步开发。

## 测试聊天

Smith 和 Brown 登录系统并交换了一些消息。两者在登录并开始聊天时看到空的聊天室。

![](img/5b721cfaa97eb697efc92e3914f62fca_219_0.png)

发送消息输入字段上方保留的空区域用于聊天内容。Smith 和 Brown 交换了一些消息，如以下屏幕截图所示：

![](img/5b721cfaa97eb697efc92e3914f62fca_219_1.png)

第三个用户 Jones 加入聊天并在聊天室中看到所有之前的消息。Jones 然后与 Smith 和 Brown 交换了一些消息。Smith 和 Brown 从系统中注销，留下 Jones 单独在聊天室中，直到她也注销为止。这在以下屏幕截图中可见：

![](img/5b721cfaa97eb697efc92e3914f62fca_220_0.png)

## 后记

本示例应用程序展示了如何在聊天室应用程序中使用 DWR。其功能与之前的图书作者应用程序类似。

## 摘要

本章中介绍的两个示例应用程序，协作式图书创作和聊天室，很明显 DWR 使开发此类协作应用程序变得非常容易。

DWR 本身甚至在应用程序中没有发挥很大的作用。DWR 只是应用程序的一个透明功能。因此，开发人员专注于实际项目和诸如数据持久性和整洁用户界面之类的方面，而不是 AJAX 的低级细节。

## 索引

### A

- **AJAX**
  - about 7
  - components 9
  - history 7
  - principle 8
  - uses 8
- **Asynchronous JavaScript XML.** *See* **AJAX**

### B

- **backend OrderSystem, messaging system**
  - testing with 163-165

### C

- **callback function, dynamic user interface**
  - advantages 70
  - methods 70
  - using 69
- **Chatroom**
  - about 202
  - chat, testing 209-211
  - Java code, developing 207-209
  - project, starting with 202
  - user interface, developing 203-207
- **client code, field completion**
  - JavaScript functions 92-94
- **client code, for tables and list**
  - HTML samples, using 79, 80
  - JavaScript function 80-83
- **Collaborative Book Authoring**
  - about 167, 168
  - Java class, developing 184
  - project starting with 168
  - testing 197-201
  - user interface, developing 168, 169
  - web application, configuring 182, 184
- **creator attributes, dwr.xml**
  - jsf 33
  - new 33
  - none 33
  - pageflow 33
  - scripted 33
  - spring 33
  - struts 33

### D

- **database, DWR**
  - configuring, in Geronimo 132-138
  - CountryDerbyDB java class, creating 138-142
  - integrating with 131
  - integration, testing 142, 143
- **deployment**
  - deployer tool, using 50
  - Geronimo Administration Console, using 47-49
  - hot deployment, using 51
  - steps 46
- **development environment, setting up**
  - DWR, using 37
  - Eclipse IDE, using 37-41
  - Firefox, using 37
- **Direct Web Remoting.** *See* **DWR**
- **DWR**
  - about 7-12
  - AJAX, for Java developers 9-11
  - applications, debugging 42
  - applications, testing 41-42
  - database, integrating with 131
  - error, handling 42, 43
  - mailing lists 11
  - messaging system, integrating with 153
  - supported browsers 30
  - user application positioning 10
  - web services, integrating with 143
- **dwr.xml, DWR configuration**
  - annotations, working with 35, 36
  - auth element, using 34
  - convertor element, using 34, 35
  - creator attributes 33
  - creators 33
  - exclude element, using 34
  - fluent configuration, using 36
  - include element, using 34
  - javascript attributes 33
  - param element, using 33
  - scope element, using 33
  - structure 32
  - using 32
- **dynamic user interface**
  - callback function 69
  - callback function, using 69
  - creating 53
- **dynamic user interface, creating**
  - sequence diagram 54
  - structure 54
  - web application, configuring 57-59
  - web application, developing 59, 60
  - web project, creating 55, 56

### E

- **Eclipse IDE, using**
  - Apache Geronimo 41
- **error, handling**
  - errorHandler used 42
  - exceptionHandler used 43
  - textHtmlHandler used 43
  - warningHandler used 42

### F

- **field completion**
  - feature 95
  - implementing 87
- **field completion, implementing**
  - client code 89-92
  - server code 88, 89
- **files, DWR configuration**
  - dwr.xml 30
  - web.xml 30
- **forms**
  - about 97
  - viewing 98
- **forms, creating**
  - FormHandler class, implementing 101-103
  - testing 103
  - user interface, developing 98-101
- **forms, testing**
  - form filling procedure 104-106

### G

- **Geronimo Administration Console, deployment** 47-49

### H

- **hot deployment, deployment** 51

### J

- **Java class, Collaborative Book Authoring**
  - Book.java class 188-190
  - BookDatabase.java class 190-196
  - BookDatabase.java class, static map variables 196
  - developing 184
  - Login.java class 184-186
  - UserDatabase class 186, 187
  - util.java class 187, 188

### M

- **map scrolling**
  - DWR, using 115, 116
  - Java class, creating 124, 125
  - map scroller, testing 126-128
  - map square, examples 116
  - user interface, developing 117
- **messaging application, messaging system**
  - JavaScript function, developing 162
  - reverse Ajax, enabling 163
- **messaging system, DWR**
  - backend OrderSystem, testing with 163-165
  - Comet mechanism used 154-158
  - integrating with 153
  - messaging application, setting up 161
  - OrderSystem Java class, developing 158-161
  - queues, setting up in Geronimo 154-158
  - STOMP (Streaming Text Oriented Messaging Protocol) 154
  - using 165

### N

- **navigation tree**
  - about 106
  - building 106
- **navigation tree, building**
  - functions 110-113
  - NavigationTree class, creating 108-110
  - part 2 user interface, developing 110-113
  - testing 113, 114
  - user interface, developing 106, 107

### O

- **OrderSystem Java class, messaging system**
  - developing 158-161
  - reverse Ajax functionality used 161

### P

- **packaging**
  - about 44
  - steps 45, 46
- **project, Chatroom**
  - dwr.xml file's source code 202
  - starting with 202
  - web.xml's source code 203

### R

- **reverse AJAX, messaging application**
  - enabling 163

### S

- **sample applications**
  - about 167
  - Chatroom 167, 202
  - Collaborative Book Authoring 167
- **supported environments** 30

### T

- **tables and list**
  - implementing 71
  - testing 85, 86
- **tables and list, implementing**
  - client code 79
  - server code 72-79

### U

- **user interface, Collaborative Book Authoring**
  - developing 168, 169
  - index.jsp page 170, 171
  - loginFailed.html page 171
  - mainpage.jsp page 171-181
  - mainpage.jsp page, DWR scripts 181, 182
- **user interface, forms**
  - developing 98, 99
  - JavaScript functions used 99-101
- **user interface, map scrolling**
  - developing 117-124
  - HTML file, using 117-124
  - JavaScript functions, using 123

### W

- **web.xml, DWR configuration**
  - about 30
  - load protection parameters 32
  - security parameters 31
- **web application, dynamic user interface**
  - developing 59, 60
  - testing 60-64
  - web pages, developing 64-69
- **web project, dynamic user interface**
  - creating 55, 56
- **web services, DWR**
  - client-side code, developing 144-151
  - integrating with 143
  - integration, testing 152, 153
  - web service call, implementing 152


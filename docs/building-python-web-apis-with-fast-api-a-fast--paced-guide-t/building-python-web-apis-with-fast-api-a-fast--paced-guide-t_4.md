## 第 2 部分：
构建和
保护 FastAPI
应用程序

完成本部分后，你将能够使用 FastAPI 构建一个功能齐全且安全的应用程序。本部分使用前一部分的知识，并将其强化到构建比第二章中构建的应用程序更复杂、功能更强的应用程序。你还将能够集成并连接到 SQL 和 NoSQL（MongoDB）数据库，并在本部分结束时能够保护 FastAPI 应用程序。

本部分包含以下章节：

+   - 第 5 章，FastAPI 应用程序结构
+   - 第 6 章，连接到数据库
+   - 第 7 章，保护 FastAPI 应用程序





### 5
结构化 FastAPI 应用

在过去的四章中，我们研究了理解 FastAPI 并创建 FastAPI 应用涉及的基本步骤。到目前为止我们构建的应用是一个单文件 todo 应用，展示了 FastAPI 的灵活性和强大功能。前几章的关键收获是使用 FastAPI 构建应用是多么容易。然而，随着复杂性和功能性的增加，应用需要进行适当的 structuring。

Structuring 指的是以有组织的格式安排应用组件，可以是模块化的，以提高应用代码和内容的 readability。具有适当 structuring 的应用能够实现更快的开发、更快的调试以及整体生产力的提升。

在本章结束时，你将具备关于 structuring 是什么以及如何 structuring 你的 API 的知识。本章将涵盖以下主题：

- Structuring application routes and models
- Implementing models for a planner API

### 技术要求

本章使用的代码可在 https://github.com/PacktPublishing/Building-Python-Web-APIs-with-FastAPI/tree/main/ch05/planner 获取。

### Structuring in FastAPI applications

在本章中，我们将构建一个活动策划应用。让我们设计应用结构如下：

```
planner/
  main.py
  database/
    __init__.py
    connection.py
  routes/
    __init__.py
    events.py
    users.py
  models/
    __init__.py
    events.py
    users.py
```

第一步是为应用创建一个新文件夹。它将被命名为 planner：

```
$ mkdir planner && cd planner
```

在新创建的 planner 文件夹中，创建一个入口文件 main.py，以及三个子文件夹 – database、routes 和 models：

```
$ touch main.py
$ mkdir database routes models
```

接下来，在每个文件夹中创建 __init__.py：

```
$ touch {database,routes,models}/__init__.py
```

在 database 文件夹中，让我们创建一个空白文件 database.py，它将处理我们在下一章中使用的数据库抽象和配置：

```
$ touch database/connection.py
```

在 routes 和 models 两个文件夹中，我们将创建两个文件 events.py 和 users.py：

```
$ touch {routes,models}/{events,users}.py
```

每个文件都有其功能，如下所述：

- Files in the routes folder:
  - events.py: This file will handle routing operations such as creating, updating, and deleting events.
  - users.py: This file will handle routing operations such as the registration and signing-in of users.
- Files in the models folder:
  - events.py: This file will contain the model definition for events operations.
  - users.py: This file will contain the model definition for user operations.

现在我们已经成功 structuring 了我们的 API，并根据其功能将相似的文件分组到组件中，让我们在下一节开始实现应用。

### Building an event planner application

在本节中，我们将构建一个活动策划应用。在此应用中，已注册用户将能够创建、更新和删除活动。创建的活动可以通过应用自动创建的活动页面进行查看。

每个已注册用户和活动都将拥有唯一的 ID。这是为了防止在管理具有相同 ID 的用户和活动时发生冲突。在本节中，我们不会优先考虑 authentication 或数据库管理，因为这将在第 6 章《Connecting to a Database》和第 7 章《Securing FastAPI Applications》中深入讨论。

为了启动开发，让我们在项目目录中创建一个虚拟环境并激活它：

```
$ python3 -m venv venv
$ source venv/bin/activate
```

接下来，让我们安装应用依赖项：

```
(venv)$ pip install fastapi uvicorn "pydantic[email]"
```

最后，将 requirements 保存到 requirements.txt 中：

```
(venv)$ pip freeze > requirements.txt
```

现在我们已经成功安装了依赖项并设置了开发环境，让我们接下来实现应用的 models。

### Implementing the models

让我们来看一下实现模型步骤：

1. 构建应用的第一步是定义事件和用户的模型。模型描述了数据将在我们的应用中如何存储、输入和表示。以下图表显示了用户和事件的建模以及它们之间的关系：

![](img/85e42522e1d8d05fd8b981490d6d9056_90_0.png)

Figure 5.1 – The model flow for the user and event

如上图所示的模型图，每个用户将拥有一个 Events 字段，即他们拥有的事件列表。

2. 让我们在 models/events.py 中定义 Event 模型：

```
from pydantic import BaseModel
from typing import List

class Event(BaseModel):
    id: int
    title: str
    image: str
    description: str
    tags: List[str]
    location: str
```

3. 让我们在 Event 类下定义一个 Config 子类，以展示当我们访问文档时模型数据将是什么样子的示例：

```
class Config:
    schema_extra = {
        "example": {
            "title": "FastAPI Book Launch",
            "image": "https://linktomyimage.com/image.png",
            "description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
            "tags": ["python", "fastapi", "book", "launch"]
            "location": "Google Meet"
        }
    }
```

我们在第一个代码块中的 event 模型包含五个字段：

- The event title
- A link to the event image banner
- The description of the event
- Event tags for grouping
- The location of the event

在第二个代码块中，我们定义了示例事件数据。这旨在指导我们在 API 中创建新事件时。

4. 现在我们已经定义了 event 模型，让我们定义 User 模型：

```
from pydantic import BaseModel, EmailStr
from typing import Optional, List
from models.events import Event

class User(BaseModel):
    email: EmailStr
    password: str
    events: Optional[List[Event]]
```

我们之前定义的 User 模型包含以下字段：

- The user’s email
- The user’s password
- A list of events created by the user, which is empty by default

5. 现在我们已经定义了 User 模型，让我们创建一个示例来指示用户数据如何存储和设置：

```
class Config:
    schema_extra = {
        "example": {
            "email": fastapi@packt.com,
            "username": "strong!!!",
            "events": [],
        }
    }
```

6. 接下来，我们将创建一个新的模型 NewUser，它继承自 User 模型；此新模型将用作注册新用户时的数据类型。User 模型将用作响应模型，在我们不希望与密码交互时使用，从而减少需要完成的工作量。

7. 最后，让我们实现一个用于用户登录的模型：

```
class UserSignIn(BaseModel):
    email: EmailStr
    password: str

    class Config:
        schema_extra = {
            "example": {
                "email": fastapi@packt.com,
                "password": "strong!!!",
                "events": [],
            }
        }
```

现在我们已经成功实现了我们的 models，让我们在下一节实现 routes。

### Implementing routes

构建应用的下一步是设置 API 的路由系统。我们将为事件和用户设计路由系统。用户路由将包括 sign-in、sign-out 和 sign-up 路由。经过身份验证的用户将可以访问创建、更新和删除事件的路由，而公众可以在事件创建后查看事件。以下图表显示了两个路由之间的关系：

![](img/85e42522e1d8d05fd8b981490d6d9056_94_0.png)

Figure 5.2 – The routes for user and event operations

让我们接下来详细查看这两个路由。

### User routes

现在我们已经从 Figure 5.2 中清楚地了解了要实现的路由，我们将开始在 users.py 中定义用户路由。让我们来看一下步骤：

1. 首先定义一个基本的 sign-up 路由：

```
from fastapi import APIRouter
```





```
status_code=status.HTTP_409_CONFLICT,
    detail="User with supplied username
    exists"
)
users[data.email] = data
return {
    "message": "User successfully registered!"
}
```

在之前定义的注册路由中，我们使用了应用内数据库。（我们将在第 6 章《连接到数据库》中介绍数据库。）

该路由会在添加新用户之前检查数据库中是否已存在具有相似电子邮件地址的用户。

- 2. 让我们实现登录路由：

```
@user_router.post("/signin")
async def sign_user_in(user: UserSignIn) -> dict:
    if users[user.email] not in users:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="User does not exist"
        )
    if users[user.email].password != user.password:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Wrong credentials passed"
        )

    return {
        "message": "User signed in successfully"
    }
```

在此路由中，第一步是检查数据库中是否存在该用户，如果用户不存在，则会引发异常。如果用户存在，应用程序将继续检查密码是否匹配，然后返回成功消息或异常。

在我们的路由中，我们以明文形式存储密码且未进行任何加密。这仅用于演示目的，通常在软件工程中这是错误的做法。适当的存储机制例如加密将在第 6 章《连接到数据库》中讨论，在那里我们的应用程序将从应用内数据库迁移到真实数据库。

3. 现在我们已经定义了用户操作的路由，让我们在 main.py 中注册它们并启动应用程序。让我们先导入库和用户路由定义：

```
from fastapi import FastAPI
from routes.user import user_router

import uvicorn
```

4. 接下来，让我们创建一个 FastAPI 实例并注册路由和应用程序：

```
app = FastAPI()

#### Register routes

app.include_router(user_router, prefix="/user")

if __name__ == "__main__":
    uvicorn.run("main:app", host="0.0.0.0", port=8080, reload=True)
```

在这段代码中，我们创建了 FastAPI 的一个实例并注册了路由。

5. 接下来，我们使用 uvicorn.run() 方法在 8080 端口启动应用程序并将热重载设置为 True。在终端中启动应用程序：

```
(venv)$ python main.py
INFO:     Will watch for changes in these directories:
['/Users/youngestdev/Work/Building-Web-APIs-with-FastAPI-and-Python/ch05/planner']
INFO:     Uvicorn running on http://0.0.0.0:8080 (Press CTRL+C to quit)
INFO:     Started reloader process [6547] using statreloader
```

```
INFO:     Started server process [6550]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

6. 现在我们的应用程序已成功启动，让我们测试我们实现的用户路由。我们将先注册一个用户：

```
(venv)$ curl -X 'POST' \
  'http://0.0.0.0:8080/user/signup' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "fastapi@packt.com",
  "password": "Stro0ng!",
  "username": "FastPackt"
}'
```

前述请求返回的响应为：

```
{
  "message": "User successfully registered!"
}
```

7. 前述响应表明所执行的操作成功。让我们测试登录路由：

```
(venv)$ curl -X 'POST' \
  'http://0.0.0.0:8080/user/signin' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "fastapi@packt.com",
  "password": "Stro0ng!"
}'
```

对该请求的前述响应如下：

```
{
  "message": "User signed in successfully"
}
```

8. 如果我们传入错误的密码，应用程序应返回不同的消息：

```
(venv)$ curl -X 'POST' \
'http://0.0.0.0:8080/user/signin' \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-d '{
"email": "fastapi@packt.com",
"password": "password!"
}'
```

前述请求的响应如下：

```
{
  "detail": "Wrong credential passed"
}
```

我们还可以通过 FastAPI 提供的交互式文档查看我们的路由，该文档由 Swagger 提供支持。让我们在浏览器中访问 http://0.0.0.0:8080/docs 以访问交互式文档：

![](img/85e42522e1d8d05fd8b981490d6d9056_98_0.png)

图 5.3 – 在交互式文档页面上查看的 planner 应用程序

现在我们已成功实现用户路由，让我们在下一节中实现事件操作的路由。

### 事件路由

在用户路由就位后，下一步是实现事件操作的路由。让我们看一下步骤：

- 1. 首先导入依赖项并定义事件路由器：

```
from fastapi import APIRouter, Body, HTTPException,
status
from models.events import Event
from typing import List

event_router = APIRouter(
    tags=["Events"]
)

events = []
```

- 2. 下一步是定义用于检索数据库中所有事件以及与所提供 ID 匹配的事件的路由：

```
@event_router.get("/", response_model=List[Event])
async def retrieve_all_events() -> List[Event]:
    return events

@event_router.get("/{id}", response_model=Event)
async def retrieve_event(id: int) -> Event:
    for event in events:
        if event.id == id:
            return event
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail="Event with supplied ID does not exist"
    )
```

在第二个路由中，当所提供 ID 的事件不存在时，我们会引发 HTTP_404_NOT_FOUND 异常。

3. 让我们实现创建事件、删除单个事件以及删除数据库中所有事件的路由：

```
@event_router.post("/new")
async def create_event(body: Event = Body(...)) -> dict:
    events.append(body)
    return {
        "message": "Event created successfully"
    }

@event_router.delete("/{id}")
async def delete_event(id: int) -> dict:
    for event in events:
        if event.id == id:
            events.remove(event)
            return { "message": "Event deleted successfully" }
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail="Event with supplied ID does not exist"
    )
@event_router.delete("/")
async def delete_all_events() -> dict:
    events.clear()
    return {
        "message": "Events deleted successfully"
    }
```

我们已成功实现了事件的路由。UPDATE 路由将在第 6 章《连接到数据库》中实现，在那里我们将把应用程序移植到使用实际数据库。

4. 现在我们已经实现了路由，让我们更新 main.py 中的路由配置以包含事件路由：

```
from fastapi import FastAPI
from routes.user import user_router
from routes.events import event_router
```

```
import uvicorn
app = FastAPI()

#### Register routes

app.include_router(user_router, prefix="/user")
app.include_router(event_router, prefix="/event")

if __name__ == "__main__":
    uvicorn.run("main:app", host="0.0.0.0", port=8080,
        reload=True)
```

应用程序在每次更改时都会自动重新加载。让我们测试路由：

- - GET 路由 – 以下操作返回一个空数组，告诉我们没有数据存在：
- - POST 路由 – 在终端中执行以下命令：

```
(venv)$ curl -X 'GET' \
  'http://0.0.0.0:8080/event/' \
  -H 'accept: application/json'
[]
```

让我们接下来向数组中添加数据。

```
(venv)$ curl -X 'POST' \
  'http://0.0.0.0:8080/event/new' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
"id": 1,
"title": "FastAPI Book Launch",
"image": "https://linktomyimage.com/image.png",
"description": "We will be discussing the contents of the FastAPI book in this event.Ensure to come with your own copy to win gifts!",
"tags": [
  "python",
```





```
"fastapi",
"book",
"launch"
],
"location": "Google Meet"
}'
```

以下是响应：

```
{
  "message": "Event created successfully"
}
```

从收到的响应来看，此操作成功。现在，让我们尝试检索刚刚创建的特定事件：

- GET 路由：

```
(venv)$ curl -X 'GET' \
'http://0.0.0.0:8080/event/1' \
-H 'accept: application/json'
```

以下是响应：

```
{
  "id": 1,
  "title": "FastAPI BookLaunch",
  "image": "https://linktomyimage.com/image.png",
  "description": "We will be discussing the contents of the FastAPI book in this event.Ensure to come with your own copy to win gifts!",
  "tags": [
    "python",
    "fastapi",
    "book",
    "launch"
  ],
  "location": "Google Meet"
}
```

最后，让我们删除该事件以确认事件路由是否正常工作：

- DELETE 路由 – 在终端中执行以下命令：

```
(venv)$ curl -X 'DELETE' \
  'http://0.0.0.0:8080/event/1' \
  -H 'accept: application/json'
```

以下是响应：

```
{
  "message": "Event deleted successfully"
}
```

如果我重试同一命令，我会得到以下响应：

```
(venv)$ {
  "detail": "Event with supplied ID does not exist"
}
```

我们已成功为我们的规划应用程序实现了路由和模型。我们还对其进行了测试以评估其工作状态。

### 摘要

在本章中，我们学习了如何构建 FastAPI 应用程序，并为活动规划应用程序实现路由和模型。我们使用了路由的基础知识以及在前一章中学习到的路由和建模知识。

在下一章中，您将学习如何将应用程序连接到 SQL 和 NoSQL 数据库。您将通过改进现有应用程序并添加更多功能来继续构建活动规划应用程序。在此之前，您将了解数据库是什么、不同类型以及如何在 FastAPI 应用程序中使用 SQL 和 NoSQL。

### 6
连接到
数据库

在上一章中，我们研究了如何构建 FastAPI 应用程序。我们成功为应用程序实现了一些路由和模型并测试了端点。然而，应用程序仍然使用应用内数据库来存储事件。在本章中，我们将迁移应用程序以使用适当的数据库。

数据库可以简单地称为数据的存储库。在此上下文中，数据库使我们能够永久存储数据，而与应用内数据库不同，应用内数据库会在任何应用程序重启或崩溃时被清除。数据库是一个包含列，即字段，和行，即记录的表。

到本章结束时，您将具备将 FastAPI 应用程序连接到数据库的知识。本章将解释如何使用 SQLModel 连接到 SQL 数据库，以及通过 Beanie 连接到 MongoDB 数据库。 （不过，应用程序将在后面的章节中主要使用 MongoDB 作为其主要数据库。）在本章中，您将涵盖以下主题：

- 设置 SQLModel
- 使用 SQLModel 对 SQL 数据库进行 CRUD 操作
- 设置 MongoDB
- 使用 Beanie 对 MongoDB 进行 CRUD 操作

### 技术要求

要跟上进度，需要 MongoDB 数据库组件。您操作系统的安装程序可以在其官方文档中找到。本章使用的代码可以在 https://github.com/PacktPublishing/Building-Python-Web-APIs-with-FastAPI/tree/main/ch06/planner 找到。

### 设置 SQLModel

将 SQL 数据库集成到我们的规划应用程序中的第一步是安装 SQLModel 库。SQLModel 库由 FastAPI 的创建者构建，并由 Pydantic 和 SQLAlchemy 提供支持。Pydantic 的支持将使我们能够轻松定义模型，正如我们在第 3 章《响应模型和错误处理》中所学到的。

由于我们将同时实现 SQL 和 NoSQL 数据库，因此我们将为本节创建一个新的 GitHub 分支。在终端中，导航到项目目录，初始化 GitHub 仓库并提交现有文件：

```
$ git init
$ git add database models routes main.py
$ git commit -m "Committing bare application without a database"
```

接下来，创建一个新分支：

```
$ git checkout -b planner-sql
```

现在，我们准备在应用程序中设置 SQLModel。从终端激活虚拟环境并安装 SQLModel 库：

```
$ source venv/bin/activate
(venv)$ pip install sqlmodel
```

在深入将数据库添加到我们的规划应用程序之前，让我们先看看本章中我们将使用的 SQLModel 中的一些方法。

### 表

表本质上是一个包含存储在数据库中的数据的对象 – 例如，事件数据将存储在事件表中。表将由列和行组成，数据最终将存储在其中。

要使用 SQLModel 创建表，首先定义一个表模型类。与 Pydantic 模型一样，表是定义的，但这次作为 SQLModel 类的子类。类定义还采用另一个配置变量 table，以指示此类是 SQLModel 表。

类中定义变量将默认表示列，除非被指定为字段。让我们看看事件表将如何定义：

```
class Event(SQLModel, table=True):
    id: Optional[int] = Field(default=None,
    primary_key=True)
    title: str
    image: str
    description: str
    location: str
    tags: List[str]
```

在此表类中，所有定义的变量都是列，除了 id，它被定义为字段。字段使用 SQLModel 库中的 Field 对象表示。id 字段也是数据库表中的主键。

> **什么是主键？**
> 主键是数据库表中记录的唯一标识符。

现在我们已经了解了表是什么以及如何创建它们，让我们在下一节中看看行。

### 行

发送到数据库表的数据存储在指定的列下的行中。要将数据插入行并存储它们，首先创建表的实例并用所需输入填充变量。例如，要将事件数据插入事件表，我们将首先创建模型的实例：

```
new_event = Event(title="Book Launch",
                image="src/fastapi.png",
                description="The book launch event will
                be held at Packt HQ, Packt city",
```

```
location="Google Meet",
tags=["packt", "book"]
```

接下来，我们使用 Session 类创建一个数据库事务：

```
with Session(engine) as session:
    session.add(new_event)
    session.commit()
```

前面的操作可能对您来说很陌生。让我们看看 Session 类是什么以及它做什么。

### 会话

会话对象处理从代码到数据库的交互。它主要充当执行操作的中间人。Session 类接受一个参数，即 SQL 引擎的实例。

现在我们已经了解了如何创建表和行，我们将看看如何创建数据库。本章中我们将使用的会话类的一些方法包括以下内容：

- add(): 此方法负责将数据库对象添加到内存中，等待进一步操作。在之前的代码块中，new_event 对象被添加到会话的内存中，等待由 commit() 方法提交到数据库。
- commit(): 此方法负责刷新会话中存在的事务。
- get(): 此方法接受两个参数 – 模型和请求的文档的 ID。此方法用于从数据库中检索单行。

现在我们知道如何创建表、行和列，以及如何使用 Session 类插入数据，让我们继续在下一节中创建数据库并执行 CRUD 操作。

### 创建数据库

在 SQLModel 中，连接到数据库是通过 SQLAlchemy 引擎完成的。引擎由 create_engine() 方法创建，从 SQLModel 库导入。





`create_engine()` 方法以数据库 URL 作为参数。数据库 URL 的形式为 `sqlite:///database.db` 或 `sqlite:///database.sqlite`。它还接受一个可选参数 `echo`，当设置为 True 时会在执行操作时打印出执行的 SQL 命令。

然而，仅靠 `create_engine()` 方法不足以创建数据库文件。要创建数据库文件，需要调用 `SQLModel.metadata.create_all(engine)` 方法，其参数为 `create_engine()` 方法的实例，例如如下所示：

```
database_file = "database.db"
engine = create_engine(database_file, echo=True)
SQLModel.metadata.create_all(engine)
```

`create_all()` 方法会创建数据库以及已定义的表。需要注意的是，包含表的 文件会被导入到建立数据库连接的文件中。

在我们的 planner 应用中，我们对事件执行 CRUD 操作。在 database 文件夹中，创建以下文件：

connection.py

在此文件中，我们将配置数据库所需的数据：

```
(venv)$ touch database/connection.py
```

现在我们已经创建了数据库连接文件，让我们创建连接应用程序到数据库所需的函数：

- 1. 我们将首先更新 `models/events.py` 中定义的 events 模型类，使其成为 SQLModel 表模型类：

```
from sqlmodel import JSON, SQLModel, Field, Column
from typing import Optional, List

class Event(SQLModel, table=True):
    id: int = Field(default=None, primary_key=True)
    title: str
    image: str
    description: str
    tags: List[str] = Field(sa_column=Column(JSON))
    location: str

class Config:
    arbitrary_types_allowed = True
    schema_extra = {
        "example": {
            "title": "FastAPI Book Launch",
            "image": "https://linktomyimage.com/image.png",
            "description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
            "tags": ["python", "fastapi", "book", "launch"],
            "location": "Google Meet"
        }
    }
```

在此代码块中，我们已将原始模型类修改为 SQL 表类。

2. 让我们再添加一个 SQLModel 类，该类将在 UPDATE 操作期间用作 body 类型：

```
class EventUpdate(SQLModel):
    title: Optional[str]
    image: Optional[str]
    description: Optional[str]
    tags: Optional[List[str]]
    location: Optional[str]

class Config:
    schema_extra = {
        "example": {
            "title": "FastAPI Book Launch",
            "image": "https://linktomyimage.com/image.png",
            "description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
            "tags": ["python", "fastapi", "book", "launch"],
            "location": "Google Meet"
        }
    }
```

3. 接下来，让我们在 `connection.py` 中定义创建数据库和表的配置：

```
from sqlalchemy import SQLModel, Session, create_engine
from models.events import Event

database_file = "planner.db"
database_connection_string = f"sqlite:///{database_file}"
connect_args = {"check_same_thread": False}
engine_url = create_engine(database_connection_string,
echo=True, connect_args=connect_args)

def conn():
    SQLModel.metadata.create_all(engine_url)

def get_session():
    with Session(engine_url) as session:
        yield session
```

在此代码块中，我们首先定义依赖项并导入表模型类。接下来，我们创建保存数据库文件位置的变量（如果不存在则会创建）、连接字符串以及 SQL 数据库实例。在 `conn()` 函数中，我们指示 SQLModel 创建数据库以及文件中存在的表 Events，并为在应用程序中持久化会话，定义了 `get_session()`。

4. 接下来，让我们指示应用程序在启动时创建数据库。使用以下代码更新 `main.py`：

```
from fastapi import FastAPI
from fastapi.responses import RedirectResponse
from database.connection import conn

from routes.users import user_router
from routes.events import event_router

import uvicorn

app = FastAPI()

#### Register routes

app.include_router(user_router, prefix="/user")
app.include_router(event_router, prefix="/event")

@app.on_event("startup")
def on_startup():
    conn()

@app.get("/")
async def home():
    return RedirectResponse(url="/event/")

if __name__ == '__main__':
    uvicorn.run("main:app", host="0.0.0.0", port=8080, reload=True)
```

数据库将在应用程序启动时创建。在启动事件中，我们调用了负责创建数据库的 `conn()` 函数。在终端启动应用程序，你应该在控制台看到输出，表明数据库以及表已成功创建：

![](img/85e42522e1d8d05fd8b981490d6d9056_113_0.png)

图 6.1 – planner 数据库和 event 表创建成功

终端中显示的 SQL 命令是因为在创建数据库引擎时将 `echo` 设置为 True。现在我们已成功创建数据库，让我们更新事件的 CRUD 操作路由以使用数据库。

### 创建事件

让我们看一下步骤：

- 1. 在 `routes/events.py` 中，更新导入以包含 Event 表模型类以及 `get_session()` 函数。导入 `get_session()` 函数是为了让路由能够访问创建的 session 对象：

```
from fastapi import APIRouter, Depends, HTTPException,
Request, status

from database.connection import get_session

from models.events import Event, EventUpdate
```

#### 什么是 Depends？

`Depends` 类负责在 FastAPI 应用程序中执行依赖注入。`Depends` 类接受一个真值源例如函数作为参数，并作为路由的函数参数传递，要求在执行任何操作之前必须满足依赖条件。

2. 接下来，让我们更新负责创建新事件的 POST 路由函数 `create_event()`：

```
@event_router.post("/new")
async def create_event(new_event: Event,
    session=Depends(get_session)) -> dict:
    session.add(new_event)
    session.commit()
    session.refresh(new_event)

    return {
        "message": "Event created successfully"
    }
```

在此代码块中，我们已指示执行数据库事务所需的 session 对象依赖于我们之前创建的 `get_session()` 函数。

在函数体中，数据被添加到 session 中，然后提交到数据库，之后数据库会被刷新。

3. 让我们测试路由以预览更改：

```
(venv)$ curl -X 'POST' \
  'http://0.0.0.0:8080/event/new' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
"title": "FastAPI Book Launch",
"image": "fastapi-book.jpeg",
"description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
"tags": [
    "python",
    "fastapi",
    "book",
    "launch"
],
"location": "Google Meet"
}'
```

返回成功的响应：

```
{
  "message": "Event created successfully"
}
```

如果操作执行失败，库将抛出异常。

### 读取事件

让我们更新获取事件列表的 GET 路由，使其从数据库中提取数据：

```
@event_router.get("/", response_model=List[Event])
async def retrieve_all_events(session=Depends(get_session)) -> List[Event]:
    statement = select(Event)
    events = session.exec(statement).all()
    return events
```

同样，通过 ID 检索时显示事件数据的路由也已更新：

```
@event_router.get("/{id}", response_model=Event)
async def retrieve_event(id: int, session=Depends(get_session)) -> Event:
    event = session.get(Event, id)
    if event:
        return event
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail="Event with supplied ID does not exist"
```



)

raise HTTPException(
    status_code=status.HTTP_404_NOT_FOUND,
    detail="Event with supplied ID does not exist"
)
```

两个路由的响应模型都已设置为模型类。让我们先通过发送 GET 请求来检索事件列表来测试这两个路由：

```
(venv)$ curl -X 'GET' \
'http://0.0.0.0:8080/event/' \
-H 'accept: application/json'
```

我们得到响应：

```
[
  {
    "id": 1,
    "title": "FastAPI Book Launch",
    "image": "fastapi-book.jpeg",
    "description": "We will be discussing the contents of the FastAPI book in this event.Ensure to come with your own copy to win gifts!",
    "tags": [
      "python",
      "fastapi",
      "book",
      "launch"
    ],
    "location": "Google Meet"
  }
]
```

接下来，让我们通过 ID 检索事件：

```
(venv)$ curl -X 'GET' \
'http://0.0.0.0:8080/event/1' \
-H 'accept: application/json'
```

```
}
{
  "id": 1,
  "title": "FastAPI Book Launch",
  "image": "fastapi-book.jpeg",
  "description": "The launch of the FastAPI book will hold on xyz.",
  "tags": [
    "python",
    " fastapi"
  ],
  "location": "virtual"
}
```

READ 操作成功实现后，让我们为应用程序添加编辑功能。

### 更新事件

让我们在 routes/events.py 中添加 UPDATE 路由：

```
@event_router.put("/edit/{id}", response_model=Event)
async def update_event(id: int, new_data: EventUpdate, session=Depends(get_session)) -> Event:
```

在函数体中，添加以下代码块以检索现有事件并处理事件更改：

```
event = session.get(Event, id)
if event:
    event_data = new_data.dict(exclude_unset=True)
    for key, value in event_data.items():
        setattr(event, key, value)
    session.add(event)
    session.commit()
    session.refresh(event)

    return event
raise HTTPException(
```

```
status_code=status.HTTP_404_NOT_FOUND,
    detail="Event with supplied ID does not exist"
)
```

在前面的代码块中，我们在继续更新事件数据之前检查事件是否存在。事件更新后，将返回更新后的数据。让我们更新现有文章的标题：

```
(venv)$ curl -X 'PUT' \
  'http://0.0.0.0:8080/event/edit/1' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "title": "Packt\'s FastAPI book launch II"
}'
{
  "id": 1,
  "title": "Packt's FastAPI book launch II",
  "image": "fastapi-book.jpeg",
  "description": "The launch of the FastAPI book will hold on xyz.",
  "tags": ["python", "fastapi"],
  "location": "virtual" }
```

现在我们已经添加了更新功能，让我们在下一节中快速添加删除操作。

### 删除事件

在 events.py 中，更新之前定义的删除路由：

```
@event_router.delete("/delete/{id}")
async def delete_event(id: int, session=Depends(get_session)) -> dict:
    event = session.get(Events, id)
    if event:
        session.delete(event)
```

```
session.commit()

return {
    "message": "Event deleted successfully"
}

raise HTTPException(
    status_code=status.HTTP_404_NOT_FOUND,
    detail="Event with supplied ID does not exist"
)
```

在此代码块中，函数检查是否提供了 ID 的事件存在，然后将其从数据库中删除。操作执行后，将返回成功消息，如果事件不存在则抛出异常。让我们从数据库中删除该事件：

```
(venv)$ curl -X 'DELETE' \
'http://0.0.0.0:8080/event/delete/1' \
-H 'accept: application/json'
```

请求返回成功响应：

```
{
    "message": "Event deleted successfully"
}
```

现在，如果我们检索事件列表，我们会得到一个空数组作为响应：

```
(venv)$ curl -X 'GET' \
'http://0.0.0.0:8080/event/' \
-H 'accept: application/json'
[]
```

我们已使用 SQLModel 成功将 SQL 数据库集成到应用程序中，并实现了 CRUD 操作。在学习如何在 MongoDB 中实现 CRUD 操作之前，让我们提交对应用程序所做的更改：

```
(venv)$ git add .
(venv)$ git commit -m "[Feature] Incorporate a SQL database and implement CRUD operations "
```

切换回 main 分支：

```
(venv)$ git checkout main
```

现在你已返回到应用程序的原始版本，让我们在下一节中将 MongoDB 作为数据库平台并实现 CRUD 操作。

### 设置 MongoDB

有许多库允许我们将 MongoDB 集成到 FastAPI 应用程序中。然而，我们将使用 Beanie，一个异步对象文档映射器 ODM 库，来从应用程序执行数据库操作。

让我们通过运行以下命令安装 beanie 库：

```
(venv)$ pip install beanie
```

在深入集成之前，让我们先查看 Beanie 库中的一些方法，以及本节中数据库表是如何创建的。

#### Document

在 SQL 中，存储在行和列中的数据包含在表中。在 NoSQL 数据库中，它被称为文档。文档表示数据将在数据库集合中如何存储。文档的定义方式与 Pydantic 模型定义相同，只是继承自 Beanie 库的 Document 类而不是。

一个示例文档定义如下：

```
from beanie import Document

class Event(Document):
    name: str
    location: str

    class Settings:
        name = "events"
```

Settings 子类用于告诉库在 MongoDB 数据库中创建传递的集合名称。

现在我们知道了如何创建文档，让我们来看一下用于执行 CRUD 操作的方法：

- `.insert()` 和 `.create()`：`.insert()` 和 `.create()` 方法由文档实例调用以在数据库中创建新记录。你也可以选择使用 `.insert_one()` 方法向数据库添加单个条目。

要将多个条目插入数据库，调用 `.insert_many()` 方法，该方法接受文档实例的列表，如下所示：

```
event = Event(name="Packt office launch",
location="Hybrid")
await event.create()
await Event.insert_one(event)
```

- `.find()` 和 `.get()`：`.find()` 方法用于查找与作为方法参数传递的搜索条件匹配的文档列表。`.get()` 方法用于检索与所提供 ID 匹配的单个文档。可以使用 `.find_one()` 方法找到与搜索条件匹配的单个文档，如下所示：

```
event = await Event.get("74478287284ff")
event = await Event.find(Event.location == "Hybrid").to_list() # 返回匹配项的列表
event = await.find_one(Event.location == "Hybrid") # 返回单个事件
```

- `.save()`、`.update()` 和 `.upsert()`：要更新文档，可以使用这些方法中的任何一种。`.update()` 方法接受更新查询，而 `.upsert()` 方法用于在文档不匹配搜索条件时使用。在本章中，我们将使用 `.update()` 方法。更新查询是 MongoDB 数据库遵循的指令，如下所示：

```
event = await Event.get("74478287284ff")
update_query = {"$set": {"location": "virtual"}}
await event.update(update_query)
```

在此代码块中，我们首先检索事件，然后创建更新查询以将事件集合中的 `location` 字段设置为 `virtual`。

- `.delete()`：此方法负责从数据库中删除文档记录，如下所示：

```
event = await Event.get("74478287284ff")
await event.delete()
```

现在我们已经了解了 Beanie 库中包含的方法的工作原理，让我们在事件规划应用程序中初始化数据库，定义我们的文档并实现 CRUD 操作。

#### 初始化数据库

让我们来看一下步骤：

- 1. 在 database 文件夹中，创建 connection.py 文件：

```
(venv)$ touch connection.py
```

Pydantic 使我们能够通过创建 BaseSettings 父类的子类的读取环境变量。在构建 Web API 时，将配置变量存储在环境文件中是标准做法。

- 2. 在 connection.py 中，添加以下内容：

```
from beanie import init_beanie
from motor.motor_asyncio import AsyncIOMotorClient
from typing import Optional
from pydantic import BaseSettings

class Settings(BaseSettings):
    DATABASE_URL: Optional[str] = None

async def initialize_database(self):
    client = AsyncIOMotorClient(self.DATABASE_URL)
    await init_beanie(
        database=client.get_default_database(),
                    document_models=[])

class Config:
    env_file = ".env"
```



在该代码块中，我们首先导入初始化数据库所需的依赖项。然后，我们定义 Settings 类，它具有 DATABASE_URL 值，该值从 Config 子类中定义的 env_file 环境中读取。我们还定义了 initialize_database 方法来初始化数据库。

init_beanie 方法接收数据库客户端，即在 SQLModel 部分中创建的引擎的 mongo 版本，以及文档列表。

3. 让我们更新 models 目录中的模型文件以包含 MongoDB 文档。在 models/events.py 中，用以下内容替换内容：

```
from beanie import Document
from typing import Optional, List

class Event(Document):
    title: str
    image: str
    description: str
    tags: List[str]
    location: str

class Config:
    schema_extra = {
        "example": {
            "title": "FastAPI Book Launch",
            "image": "https://linktomyimage.com/image.png",
            "description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
            "tags": ["python", "fastapi", "book", "launch"],
            "location": "Google Meet"
        }
    }
```

```
class Settings:
    name = "events"
```

4. 让我们为 UPDATE 操作创建一个 Pydantic 模型：

```
class EventUpdate(BaseModel):
    title: Optional[str]
    image: Optional[str]
    description: Optional[str]
    tags: Optional[List[str]]
    location: Optional[str]

class Config:
    schema_extra = {
        "example": {
            "title": "FastAPI Book Launch",
            "image": "https://linktomyimage.com/image.png",
            "description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
            "tags": ["python", "fastapi", "book", "launch"],
            "location": "Google Meet"
        }
    }
```

5. 在 model/users.py 中，用以下内容替换文件内容：

```
from typing import Optional, List
from beanie import Document, Link

from pydantic import BaseModel, EmailStr

from models.events import Event
```

```
class User(Document):
    email: EmailStr
    password: str
    events: Optional[List[Link[Event]]]

    class Settings:
        name = "users"

    class Config:
        schema_extra = {
            "example": {
                "email": "fastapi@packt.com",
                "password": "strong!!!",
                "events": [],
            }
        }

class UserSignIn(BaseModel):
    email: EmailStr
    password: str
```

6. 现在我们已经定义了文档，让我们更新 connection.py 中的 document_models 字段：

```
from models.users import User
from models.events import Event

async def initialize_database(self):
    client = AsyncIOMotorClient(self.DATABASE_URL)
    await init_beanie(
        database=client.get_default_database(),
        document_models=[Event, User])
```

7. 最后，让我们创建一个环境文件 .env，并添加数据库 URL 以完成数据库初始化阶段：

```
(venv)$ touch .env
(venv)$ echo DATABASE_URL=mongodb://localhost:27017/planner >> .env
```

现在我们已成功添加初始化数据库的代码块，让我们继续实现 CRUD 操作的方法。

### CRUD 操作

在 connection.py 中，创建一个新的 Database 类，该类在初始化期间接收模型作为参数：

```
from pydantic import BaseSettings, BaseModel
from typing import Any, List, Optional

class Database:
    def __init__(self, model):
        self.model = model
```

在初始化期间传递的模型是 Event 或 User 文档模型类。

#### Create

让我们在 Database 类下创建一个方法来向数据库集合添加记录：

```
async def save(self, document) -> None:
    await document.create()
    return
```

在此代码块中，我们定义了 save 方法以接收文档，该文档将在 Database 实例实例化时传递给 Database 实例的文档实例。

#### Read

让我们创建方法来检索数据库记录或数据库集合中存在的所有记录：

```
async def get(self, id: PydanticObjectId) -> Any:
    doc = await self.model.get(id)
    if doc:
        return doc
```

```
return False

async def get_all(self) -> List[Any]:
    docs = await self.model.find_all().to_list()
    return docs
```

第一个方法 get() 将 ID 作为方法参数并返回数据库中对应的记录，而 get_all() 方法不带参数并返回数据库中存在的所有记录的列表。

#### Update

让我们创建处理更新现有记录过程的方法：

```
async def update(self, id: PydanticObjectId, body: BaseModel) -> Any:
    doc_id = id
    des_body = body.dict()
    des_body = {k:v for k,v in des_body.items() if v is not None}
    update_query = {"$set": {
        field: value for field, value in des_body.items()
    }}

    doc = await self.get(doc_id)
    if not doc:
        return False
    await doc.update(update_query)
    return doc
```

在此代码块中，update 方法接收 ID 和负责的 Pydantic 模式，该模式将包含客户端通过 PUT 请求发送的更新字段。更新的请求体首先解析为字典，然后过滤以删除 None 值。完成此操作后，它将被插入到更新查询中，然后由 Beanie 的 update() 方法执行。

#### Delete

最后，让我们创建一个方法来从数据库中删除记录：

```
async def delete(self, id: PydanticObjectId) -> bool:
    doc = await self.get(id)
    if not doc:
        return False
    await doc.delete()
    return True
```

在此代码块中，该方法在从数据库中删除之前检查记录是否存在。

现在我们已在数据库文件中填充了执行 CRUD 操作所需的方法，让我们也更新路由。

### routes/events.py

让我们首先更新导入并创建数据库实例：

```
from beanie import PydanticObjectId
from fastapi import APIRouter, HTTPException, status
from database.connection import Database

from models.events import Event
from typing import List
event_database = Database(Event)
```

导入和数据库实例就位后，让我们更新所有路由。首先更新 GET 路由：

```
@event_router.get("/", response_model=List[Event])
async def retrieve_all_events() -> List[Event]:
    events = await event_database.get_all()
    return events

@event_router.get("/{id}", response_model=Event)
async def retrieve_event(id: PydanticObjectId) -> Event:
    event = await event_database.get(id)
    if not event:
```

```
raise HTTPException(
    status_code=status.HTTP_404_NOT_FOUND,
    detail="Event with supplied ID does not exist"
)
return event
```

在 GET 路由中，我们正在调用我们在数据库文件中之前定义的方法。让我们更新 POST 路由：

```
@event_router.post("/new")
async def create_event(body: Event) -> dict:
    await event_database.save(body)
    return {
        "message": "Event created successfully"
    }
```

让我们创建 UPDATE 路由：

```
@event_router.put("/{id}", response_model=Event)
async def update_event(id: PydanticObjectId, body: EventUpdate) -> Event:
    updated_event = await event_database.update(id, body)
    if not updated_event:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="Event with supplied ID does not exist"
        )
    return updated_event
```

最后，让我们更新 DELETE 路由：

```
@event_router.delete("/{id}")
async def delete_event(id: PydanticObjectId) -> dict:
    event = await event_database.delete(id)
    if not event:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="Event with supplied ID does not exist"
        )
```





```
return {
    "message": "Event deleted successfully."
}
```

现在我们已经为事件路由实现了 CRUD 操作，让我们来实现用户注册和用户登录的路由。

### routes/users.py

让我们先更新导入并创建数据库实例：

```
from fastapi import APIRouter, HTTPException, status
from database.connection import Database

from models.users import User, UserSignIn

user_router = APIRouter(
    tags=["User"],
)

user_database = Database(User)
```

接下来，更新用于注册新用户的 POST 路由：

```
@user_router.post("/signup")
async def sign_user_up(user: User) -> dict:
    user_exist = await User.find_one(User.email == user.email)
    if user_exist:
        raise HTTPException(
            status_code=status.HTTP_409_CONFLICT,
            detail="User with email provided exists already."
        )
    await user_database.save(user)
    return {
        "message": "User created successfully"
    }
```

在这个代码块中，我们在将用户添加到数据库之前检查是否存在使用传入电子邮件的用户。让我们添加用户登录的路由：

```
@user_router.post("/signin")
async def sign_user_in(user: UserSignIn) -> dict:
    user_exist = await User.find_one(User.email ==
    user.email)
    if not user_exist:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="User with email does not exist."
        )
    if user_exist.password == user.password:
        return {
            "message": "User signed in successfully."
        }
    raise HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Invalid details passed."
    )
```

在这个定义的路由中，我们首先检查用户是否存在，然后再检查其凭据的有效性。这里使用的身份验证方法很基础，不建议在生产环境中使用。我们将在下一章中了解正确的身份验证流程。

现在我们已经实现了路由，让我们启动 MongoDB 实例以及我们的应用程序。创建一个文件夹来存放我们的 MongoDB 数据库并启动 MongoDB 实例：

```
(venv) $ mkdir store
(venv) $ mongod --dbpath store
```

接下来，在另一个窗口中启动应用程序：

```
(venv)$ python main.py
INFO:     Uvicorn running on http://0.0.0.0:8080 (Press CTRL+C to quit)
INFO:     Started reloader process [3744] using statreload
INFO:     Started server process [3747]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

让我们测试事件路由：

- 1. 创建一个事件：

```
(venv)$ curl -X 'POST' \
  'http://0.0.0.0:8080/event/new' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
"title": "FastAPI Book Launch",
"image": "https://linktomyimage.com/image.png",
"description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
"tags": [
  "python",
  "fastapi",
  "book",
  "launch"
],
"location": "Google Meet"
}'
```

以下是前述操作的响应：

```
{
  "message": "Event created successfully"
}
```

2. 获取所有事件：

```
(venv)$ curl -X 'GET' \
  'http://0.0.0.0:8080/event/' \
  -H 'accept: application/json'
```

前述请求返回事件列表：

```
[
  {
    "_id": "624daab1585059e8a3fa77ac",
    "title": "FastAPI Book Launch",
    "image": "https://linktomyimage.com/image.png",
    "description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
    "tags": [
      "python",
      "fastapi",
      "book",
      "launch"
    ],
    "location": "Google Meet"
  }
]
```

3. 获取一个事件：

```
(venv)$ curl -X 'GET' \
  'http://0.0.0.0:8080/event/624daab1585059e8a3fa77ac' \
  -H 'accept: application/json'
```

此操作返回与提供 ID 匹配的事件：

```
{
  "_id": "624daab1585059e8a3fa77ac",
  "title": "FastAPI Book Launch",
  "image": "https://linktomyimage.com/image.png",
  "description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
```

```
"tags": [
    "python",
    "fastapi",
    "book",
    "launch"
],
"location": "Google Meet"
}
```

4. 让我们将事件位置更新为混合：

```
(venv)$ curl -X 'PUT' \
  'http://0.0.0.0:8080/event/624daab1585059e8a3fa77ac' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "location": "Hybrid"
}'
{
  "_id": "624daab1585059e8a3fa77ac",
  "title": "FastAPI Book Launch",
  "image": "https://linktomyimage.com/image.png",
  "description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
  "tags": [
    "python","fastapi",
    "book",
    "launch"
  ],
  "location": "Hybrid"
}
```

5. 最后，让我们删除该事件：

```
(venv)$ curl -X 'DELETE' \
  'http://0.0.0.0:8080/event/624daab1585059e8a3fa77ac' \
  -H 'accept: application/json'
```

以下是请求收到的响应：

```
{
  "message": "Event deleted successfully."
}
```

6. 现在我们已经测试了事件的路由，让我们创建一个新用户然后登录：

```
(venv)$ curl -X 'POST' \
  'http://0.0.0.0:8080/user/signup' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "fastapi@packt.com",
  "password": "strong!!!",
  "events": []
}'
```

请求返回响应：

```
{
  "message": "User created successfully"
}
```

再次运行请求会返回 HTTP 409 错误，表示冲突：

```
{
  "detail": "User with email provided exists already."
}
```

我们最初设计路由是为了检查现有用户以避免重复。

7. 现在，让我们发送 POST 请求以登录我们刚刚创建的用户：

```
(venv)$ curl -X 'POST' \
  'http://0.0.0.0:8080/user/signin' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "fastapi@packt.com",
  "password": "strong!!!"
}'
```

请求返回 HTTP 200 成功消息：

```
{
  "message": "User signed in successfully."
}
```

我们已使用 Beanie 库成功实现了 CRUD 操作。

### Summary

在本章中，我们学习了如何分别使用 SQLModel 和 Beanie 添加 SQL 和 NoSQL 数据库。我们使用了之前章节中的所有知识。我们还测试了路由以确保它们按计划工作。

在下一章中，您将了解如何保护您的应用程序。您将首先学习身份验证的基础知识以及 FastAPI 开发人员可用的各种身份验证方法。然后您将实现一个依赖于 JSON Web Token (JWT) 的身份验证系统并保护创建、更新和删除事件的路由。最后，您将修改创建事件的路由以允许将事件链接到用户。

### 7
### Securing FastAPI Applications

在上一章中，我们研究了如何将 FastAPI 应用程序连接到 SQL 和 NoSQL 数据库。我们成功实现了数据库方法并更新了现有路由以实现应用程序与数据库之间的交互。然而，规划应用程序仍然允许任何人添加事件，而不仅仅是经过身份验证的用户。在本章中，我们将使用 JSON Web Token (JWT) 保护应用程序并将某些事件操作限制为仅经过身份验证的用户。

保护应用程序涉及添加安全措施以限制未经授权的实体访问应用程序功能，以防止黑客攻击或对应用程序进行非法修改。身份验证是验证实体传递的凭据的过程，而授权 simply 意味着给予实体执行指定操作的权限。当凭据已被验证后，该实体将被授权执行各种操作。





在本章结束时，你将能够为 FastAPI 应用添加一层身份验证。本章将解释通过哈希保护密码、添加身份验证层以及保护未通过身份验证用户访问路由的过程。在本章中，我们将涵盖以下主题：

- FastAPI 中的身份验证方法
- 使用 OAuth2 和 JWT 保护应用程序
- 使用依赖注入保护路由
- 配置 CORS

### 技术要求

要跟上进度，需要 MongoDB 数据库组件。适用于你操作系统的安装程序可以在其官方文档中找到。本章使用的代码可以在 https://github.com/PacktPublishing/Building-Python-Web-APIs-with-FastAPI/tree/main/ch07/planner 找到。

### FastAPI 中的身份验证方法

FastAPI 中有多种身份验证方法可用。FastAPI 支持常见的身份验证方法，即基本 HTTP 身份验证、Cookie 和 Bearer 令牌身份验证。让我们简要了解一下每种方法的内容：

- **基本 HTTP 身份验证：** 在这种身份验证方法中，用户凭据，通常是用户名和密码，通过 Authorization HTTP 头发送。请求反过来返回一个包含 Basic 值和可选 realm 参数的 WWW-Authenticate 头，该参数表示进行身份验证请求的资源。
- **Cookies：** 当数据需要在客户端存储时，例如在 Web 浏览器中，就会使用 Cookie。FastAPI 应用也可以使用 Cookie 来存储用户数据，服务器可以检索这些数据用于身份验证目的。
- **Bearer 令牌身份验证：** 这种身份验证方法涉及使用称为 Bearer 令牌的安全令牌。这些令牌与 Bearer 关键字一起发送在 Authorization 头请求中。最常用的令牌是 JWT，它通常是一个包含用户 ID 和令牌过期时间的字典。

这里列出的每种身份验证方法都有其特定的用例以及优缺点。然而在本章中，我们将使用 Bearer 令牌身份验证。

身份验证方法作为依赖项注入到 FastAPI 应用中，并在运行时调用。这仅仅意味着当身份验证方法被定义时，它们处于休眠状态，直到被注入到使用位置。这种活动称为 **依赖注入。**

#### 依赖注入

依赖注入是一种模式，其中一个对象——在本例中为函数——接收实例变量以供函数进一步执行所需。

在 FastAPI 中，依赖项是通过在路径操作函数参数中声明它们来注入的。我们在前几章中使用了依赖注入。以下是上一章中的一个示例，我们从传递给函数的用户模型中检索 email 字段：

```
@user_router.post("/signup")
async def sign_user_up(user: User) -> dict:
    user_exist = await User.find_one(User.email == user.email)
```

在此代码块中，定义的依赖项是 User 模型类，它被注入到 sign_user_up() 函数中。通过将 User 模型注入到 user 函数参数中，我们可以轻松检索对象的属性。

#### 创建和使用依赖项

在 FastAPI 中，依赖项可以定义为函数或类。创建的依赖项使我们能够访问其底层值或方法，从而无需在继承它们的函数中创建这些对象。依赖注入有助于在某些情况下减少代码重复，例如强制执行身份验证和授权。

一个示例依赖项定义如下：

```
async def get_user(token: str):
    user = decode_token(token)
    return user
```

此依赖项是一个函数，它以 token 作为参数并从外部函数 decode_token 返回 user 参数。要使用此依赖项，依赖函数参数声明设置为具有 Depends 参数，例如：

```
from fastapi import Depends

@router.get("/user/me")
```

```
async get_user_details(user: User = Depends(get_user)):
    return user
```

这里的路由函数依赖于 get_user 函数，后者充当其依赖项。这意味着要访问前述路由，必须满足 get_user 依赖项。

Depends 类从 FastAPI 库导入，负责接收作为参数传递的函数，并在端点被调用时执行它，自动将传递给它的函数的返回值提供给端点。

现在你已经对如何创建依赖项以及如何使用它有了一些了解，让我们为活动规划应用构建身份验证依赖项。

### 使用 OAuth2 和 JWT 保护应用程序

在本节中，我们将为活动规划应用构建身份验证系统。我们将使用 OAuth2 密码流，这需要客户端以表单数据的形式发送用户名和密码。在我们的例子中，用户名是创建账户时使用的电子邮件。

当表单数据从客户端发送到服务器时，作为响应的访问令牌，即签名的 JWT，会被发送。通常，在创建令牌以允许进一步授权之前，会进行后台检查以验证发送到服务器的凭据。为了授权已通过身份验证的用户，JWT 在通过头发送时会被 Bearer 前缀，以授权服务器上的操作。

> 什么是 JWT 以及为什么要对其进行签名？
> JWT 是一个编码字符串，通常包含一个包含负载、签名及其算法的字典。JWT 使用只有服务器和客户端知道的唯一密钥进行签名，以避免编码字符串被外部主体篡改。

![](img/85e42522e1d8d05fd8b981490d6d9056_140_0.png)

图 7.1 – 身份验证流程

现在我们对身份验证流程的工作原理有了一些了解，让我们创建设置应用程序中身份验证系统所需的必要文件夹和文件：

1. 在项目文件夹中，首先创建 auth 文件夹：

```
(venv)$ mkdir auth
```

2. 接下来，在 auth 文件夹中创建以下文件：

```
(venv)$ cd auth && touch {__init__,jwt_handler,authenticate,hash_password}.py
```

前面的命令创建了四个文件：

- jwt_handler.py：此文件将包含用于编码和解码 JWT 字符串所需的函数。
- authenticate.py：此文件将包含 authenticate 依赖项，该依赖项将被注入到我们的路由中以强制执行身份验证和授权。
- hash_password.py：此文件将包含在注册期间用于加密用户密码以及在登录期间比较密码的函数。
- __init__.py：此文件表示文件夹的内容作为模块。

现在文件已经创建，让我们构建各个组件。我们将首先创建用于哈希用户密码的组件。

#### 哈希密码

在上一章中，我们以明文形式存储用户密码。这是在构建 API 时高度不安全且被禁止的做法。密码应使用适当的库进行加密或哈希。我们将使用 bcrypt 对用户密码进行加密。

让我们安装 passlib 库。该库包含我们将用于哈希用户密码的 bcrypt 哈希算法：

```
(venv)$ pip install passlib[bcrypt]
```

现在我们已经安装了该库，让我们在 hash_password.py 中创建用于哈希密码的函数：

```
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"],
```

```
deprecated="auto")

class HashPassword:
    def create_hash(self, password: str):
        return pwd_context.hash(password)

    def verify_hash(self, plain_password: str,
    hashed_password: str):
        return pwd_context.verify(plain_password,
            hashed_password)
```

在前面的代码块中，我们首先导入 CryptContext，它接受用于哈希传递给它的字符串的 bcrypt 方案。上下文存储在 pwd_context 变量中，使我们能够访问执行任务所需的<|message|>

```
user.email)

if user_exist:
    raise HTTPException(
        status_code=status.HTTP_409_CONFLICT,
        detail="User with email provided exists already."
    )
hashed_password = hash_password.create_hash(
user.password)
user.password = hashed_password
await user_database.save(user)
return {
    "message": "User created successfully"
}
```

现在我们已将用户注册路由更新为在保存前对密码进行哈希处理，让我们创建一个新用户来确认。在终端窗口中启动应用程序：

```
(venv)$ python main.py
INFO:     Uvicorn running on http://0.0.0.0:8080 (Press CTRL+C to quit)
INFO:     Started reloader process [8144] using statreload
INFO:     Started server process [8147]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

在另一个终端窗口中启动 MongoDB 实例：

```
$ mongod --dbpath database --port 27017
```

接下来，让我们创建一个新用户：

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/user/signup' \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-d '{
"email": "reader@packt.com",
"password": "exemplary"
}'
```

我们从上述请求中获得了成功响应：

```
{
    "message": "User created successfully"
}
```

现在我们已创建了一个用户，让我们验证发送到数据库的密码是否已进行哈希处理。为此，我们将创建一个交互式 MongoDB 会话，允许我们从数据库内部运行命令。

在新的终端窗口中，运行以下命令：

```
$ mongo --port 27017
```

启动了交互式 MongoDB 会话：

![](img/85e42522e1d8d05fd8b981490d6d9056_144_0.png)

图 7.2 – 一个交互式 MongoDB 会话

在交互式会话运行时，运行一系列命令以切换到 planner 数据库并检索所有用户记录：

```
> use planner
> db.users.find({})
```

![](img/85e42522e1d8d05fd8b981490d6d9056_145_0.png)

图 7.3 – find all users 查询的结果

前面的命令返回用户列表，我们现在可以确认用户的密码在存储到数据库之前已被哈希处理。现在我们已成功构建了安全存储用户密码的组件，让我们构建创建和验证 JWT 的组件。

### 创建和验证访问令牌

创建 JWT 使我们更接近于保护应用程序。令牌的负载将包含用户 ID 和编码前的过期时间，如下所示：

![](img/85e42522e1d8d05fd8b981490d6d9056_145_1.png)

图 7.4 – JWT 的结构

之前，我们了解到为什么 JWT 需要签名。JWT 使用只有发送方和接收方知道的密钥进行签名。让我们更新 database/database.py 中的 Settings 类以及环境文件 .env，以包含 SECRET_KEY 变量，该变量将用于签署 JWT：

#### database/database.py

```
class Settings(BaseSettings):
    SECRET_KEY: Optional[str] = None
```

#### .env

```
SECRET_KEY=HI5HL3V3L$3CR3T
```

设置好后，在 jwt_handler.py 中添加以下导入：

```
import time
from datetime import datetime

from fastapi import HTTPException, status
from jose import jwt, JWTError
from database.database import Settings
```

在前面的代码块中，我们导入了 time 模块、HTTPException 类以及来自 FastAPI 的 status。我们还导入了负责编码和解码 JWT 的 jose 库以及 Settings 类。

接下来，我们将创建一个 Settings 类的实例，以便检索 SECRET_KEY 变量并创建负责创建令牌的函数：

```
settings = Settings()

def create_access_token(user: str) -> str:
    payload = {
        "user": user,
        "expires": time.time() + 3600
    }

    token = jwt.encode(payload, settings.SECRET_KEY,
    algorithm="HS256")
    return token
```

在前面的代码块中，该函数接受一个字符串参数，该参数传递到 payload 字典中。payload 字典包含用户和过期时间，在 JWT 解码时返回。

expires 值设置为创建后一小时。payload 然后传递给 encode() 方法，该方法接受三个参数：

- **Payload**：包含要编码的值的字典。
- **Key**：用于签署 payload 的密钥。
- **Algorithm**：用于签署 payload 的算法。默认且最常见的是 **HS256** 算法。

接下来，让我们创建一个函数来验证发送到我们应用程序的令牌的真实性：

```
def verify_access_token(token: str) -> dict:
    try:
        data = jwt.decode(token, settings.SECRET_KEY,
        algorithms=["HS256"])

        expire = data.get("expires")

        if expire is None:
            raise HTTPException(
                status_code=status.HTTP_400_BAD_REQUEST,
                detail="No access token supplied"
            )
        if datetime.utcnow() >
        datetime.utcfromtimestamp(expire):
            raise HTTPException(
                status_code=status.HTTP_403_FORBIDDEN,
                detail="Token expired!"
            )
        return data

    except JWTError:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail="Invalid token"
        )
```

在前面的代码块中，该函数以令牌字符串作为参数，并在 try 块中运行多个检查。该函数首先检查令牌的过期时间。如果没有过期时间，则表示未提供令牌。第二个检查是令牌的有效性 – 抛出异常以通知用户令牌已过期。如果令牌有效，则返回解码后的负载。

在 except 块中，对于任何 JWT 错误都会抛出 bad request 异常。

现在我们已实现了用于创建和验证发送到应用程序的令牌的函数，让我们创建验证用户身份验证并作为依赖项的函数。

### 处理用户身份验证

我们已成功实现了哈希和比较密码的组件以及创建和解码 JWT 的组件。让我们实现将注入到事件路由中的依赖函数。该函数将作为检索活动会话用户的单一事实来源。

在 auth/authenticate.py 中，添加以下内容：

```
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer

from auth.jwt_handler import verify_access_token

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/user/signin")

async def authenticate(token: str = Depends(oauth2_scheme)) -> str:
    if not token:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Sign in for access"
        )

    decoded_token = verify_access_token(token)
    return decoded_token["user"]
```

在前面的代码块中，我们首先导入必要的依赖项：

- Depends：这将 oauth2_scheme 注入到函数中作为依赖项。
- OAuth2PasswordBearer：此类告诉应用程序存在安全方案。
- verify_access_token：此函数在创建和验证访问令牌部分中定义，将用于检查令牌的有效性。

我们然后定义 OAuth2 方案的令牌 URL 和 authenticate 函数。authenticate 函数以令牌作为参数。函数作为依赖项注入了 OAuth 方案。令牌被解码，如果令牌有效，则返回负载的用户字段，否则返回 verify_access_token 函数中定义的适当错误响应。

现在我们已成功创建了用于保护路由的依赖项，让我们更新路由中的身份验证流程，以及将 authenticate 函数注入到事件路由中。

### 更新应用程序

在本节中，我们将更新路由以使用新的身份验证模型。最后，我们将更新用于添加事件的 POST 路由以填充用户记录中的 events 字段。

#### 更新用户登录路由

在 routes/users.py 中，更新导入：

```
from fastapi import APIRouter, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordRequestForm
from auth.jwt_handler import create_access_token

from models.users import User
```

我们从 FastAPI 的 security 模块导入了 OAuth2PasswordRequestForm 类。这将被注入到登录路由中以检索发送的凭据：用户名和密码。让我们更新 sign_user_in() 路由函数：

```
async def sign_user_in(user: OAuth2PasswordRequestForm = Depends()) -> dict:
    user_exist = await User.find_one(User.email ==
```





```
user.username)
..
if hash_password.verify_hash(user.password,
user_exist.password):
    access_token = create_access_token(
    user_exist.email)
    return {
        "access_token": access_token,
        "token_type": "Bearer"
    }
```

在前面的代码块中，我们将 `OAuth2PasswordRequestForm` 类注入为此函数的依赖项，确保严格遵循 OAuth 规范。在函数体中，我们比较密码并返回访问令牌和令牌类型。在测试更新后的路由之前，让我们先在 `models/users.py` 中为登录路由创建一个响应模型，以替换不再使用的 `UserSignIn` 模型类：

```
class TokenResponse(BaseModel):
    access_token: str
    token_type: str
```

更新导入和登录路由的响应模型：

```
from models.users import User, TokenResponse

@user_router.post("/signin", response_model=TokenResponse)
```

让我们访问交互式文档以确认请求体是否符合 `http://0.0.0.0:8080/docs` 上的 OAuth2 规范：

![](img/85e42522e1d8d05fd8b981490d6d9056_151_0.png)

图 7.5 – 更新后登录路由的请求体

让我们登录以验证路由是否正常工作：

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/user/signin' \
-H 'accept: application/json' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=&username=reader%40packt.com&password=exemplary&scope=&client_id=&client_secret='
```

返回的响应是一个访问令牌和令牌类型：

```
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoicmhZG V yQHBhY2t0LmNvbSIsImV4cGlyZXMiOjE2NTA4M4jc0MjQuMDg2NDAxfQ.LY4i5EjIzlsKdfMyWki7XH7lLeDuVt3832hNfkQx8C8",
  "token_type": "Bearer"
}
```

现在我们已经确认路由按预期工作，让我们更新事件路由以仅允许授权用户进行 CREATE、UPDATE 和 DELETE 事件。

### 更新事件路由

现在我们已经实现了身份验证，让我们将身份验证依赖项注入到 POST、PUT 和 DELETE 路由函数中：

```
from auth.authenticate import authenticate

async def create_event(body: Event, user: str = Depends(authenticate)) -> dict:
  ...

async def update_event(id: PydanticObjectId, body: EventUpdate, user: str = Depends(authenticate)) -> Event:
  ...

async def delete_event(id: PydanticObjectId, user: str = Depends(authenticate)) -> dict:
  ...
```

注入依赖项后，交互式文档网站会自动更新以显示受保护的路由。如果我们登录到 `http://0.0.0.0:8080/docs`，我们可以在右上角看到 Authorize 按钮以及事件路由上的锁：

图 7.6 – 更新后的文档页面

如果我们点击 Authorize 按钮，会显示一个登录模态框。输入我们的凭据和密码后会返回以下屏幕：

![](img/85e42522e1d8d05fd8b981490d6d9056_153_0.png)

![](img/85e42522e1d8d05fd8b981490d6d9056_153_1.png)

图 7.7 – 已认证用户

现在我们已成功登录，我们可以创建一个事件：

![](img/85e42522e1d8d05fd8b981490d6d9056_154_0.png)

图 7.8 – 创建新事件

也可以从命令行执行相同的操作。首先，让我们获取访问令牌：

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/user/signin' \
-H 'accept: application/json' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=&username=reader%40packt.com&password=exemplary&scope=&client_id=&client_secret='
```

发送的请求返回访问令牌，即一个 JWT 字符串，以及令牌类型，即 Bearer 类型：

```
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlc2VyIjoi...",
  "token_type": "Bearer"
}
```

现在，让我们从命令行创建一个新事件：

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/event/new' \
-H 'accept: application/json' \
-H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXV CJ9.eyJlc2VyIjoicmVhZGVyQHBhY2t0LmNvbSIImV4cGlyZXMiOjE2NTA4Mjk xODMuNTgTg3NjAyfQ.MOXjI5GXnyzGNftdlxDGyM119_L1luPq8yCxBHepf04' \
-H 'Content-Type: application/json' \
-d '{
"title": "FastAPI Book Launch CLI",
"image": "https://linktomyimage.com/image.png",
"description": "We will be discussing the contents of the FastAPI book in this event.Ensure to come with your own copy to win gifts!",
"tags": [
"python",
"fastapi",
"book",
"launch"
],
"location": "Google Meet"
}'
```

在此发送的请求中，`Authorization: Bearer` 头部也被发送，以告知应用程序我们有权执行此操作。得到的响应如下：

```
{ "message": "Event created successfully" }
```

如果我们尝试在不传递带有有效令牌的授权头的情况下创建事件，将返回 HTTP 401 Unauthorized 错误：

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/event/new' \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-d '{
"title": "FastAPI BookLaunch",
```

```
"image": "https://linktomyimage.com/image.png",
"description": "We will be discussing the contents of the FastAPI book in this event.Ensure to come with your own copy to win gifts!",
"tags": [
    "python",
    "fastapi",
    "book",
    "launch"
],
"location": "Google Meet"
}'
```

以下是响应：

```
$ {
  "detail": "Not authenticated"
}
```

现在我们已成功保护了路由，让我们按如下方式更新受保护的路由：

- POST 路由：将创建的事件添加到用户拥有的事件列表中。
- UPDATE 路由：修改路由以确保只有创建事件的用户才能更新。
- DELETE 路由：修改路由以确保只有创建事件的用户才能删除。

在上一节中，我们成功地将身份验证依赖项注入到我们的路由操作中。为了便于识别事件并防止用户删除其他用户的事件，我们还将更新事件文档类以及路由。

### 更新事件文档类和路由

在 `models/events.py` 中向 Event 文档类添加 `creator` 字段：

```
class Event(Document):
    creator: Optional[str]
```

此字段将使我们能够将对事件执行的操作限制为仅用户本人。

接下来，让我们修改 `routes/events.py` 中的 POST 路由，在创建新事件时更新 `creator` 字段：

```
@event_router.post("/new")
async def create_event(body: Event, user: str = Depends(authenticate)) -> dict:
    body.creator = user
    await event_database.save(body)
    return {
        "message": "Event created successfully"
    }
```

在前面的代码块中，我们已更新 POST 路由以将当前用户的电子邮件添加为事件的创建者。如果您创建一个新事件，事件将存储创建者的电子邮件：

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/event/new' \
-H 'accept: application/json' \
-H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXV CJ9.eyJlc2VyIjoicmVhZGVyQHBhY2t0LmNvbSIImV4cGlyZXMiOjE2NTA4MzI5NjqQuMTU3MjQ4fQ.RxR1TYMx91JtVMNzYcT7718xXWX7skTCfWbnJxyf6fU' \
-H 'Content-Type: application/json' \
-d '{
"title": "FastAPI Book Launch",
"image": "https://linktomyimage.com/image.png",
"description": "We will be discussing the contents of the FastAPI book in this event.Ensure to come with your own copy to win gifts!",
"tags": [
    "python",
    "fastapi",
    "book",
    "launch"
],
"location": "Google Meet"
```

```
}'
```

上述请求返回的响应是：

```
{
  "message": "Event created successfully"
}
```

接下来，让我们检索数据库中存储的事件列表：

```
$ curl -X 'GET' \
'http://0.0.0.0:8080/event/' \
-H 'accept: application/json'
```

上述请求的响应是：

```
[
  {
    "_id": "6265a807e0c8daefb72261ea",
    "creator": "reader@packt.com",
    "title": "FastAPI BookLaunch",
    "image": "https://linktomyimage.com/image.png",
    "description": "We will be discussing the contents of the FastAPI book in this event.Ensure to come with your own copy to win gifts!",
    "tags": [
      "python",
      "fastapi",
      "book",
      "launch"
    ],
    "location": "Google Meet"
  },
]
```

接下来，让我们更新 UPDATE 路由：

```
@event_router.put("/{id}", response_model=Event)
async def update_event(id: PydanticObjectId, body: EventUpdate,
```



user: str = Depends(authenticate)) -> Event:
    event = await event_database.get(id)
    if event.creator != user:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail="Operation not allowed"
        )

在前面的代码块中，路由函数在继续执行前会检查当前用户是否可以编辑事件，否则会抛出 HTTP 400 Bad Request 异常。
下面是一个使用不同用户的示例：

```
$ curl -X 'PUT' \
  'http://0.0.0.0:8080/event/6265a83fc823a3c912830074' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiZmFzdGFwaUBwYWNRdC5jb20iLCJleHBpcmVzIjoxNjUwODM5NzYxJL2NzgzMX0.MMRT6pwEDBVHTU5C1a6MV8j9wCfWhqbza9NBpZz08xE' \
  -H 'Content-Type: application/json' \
  -d '{
"title": "FastAPI Book Launch"
}'
```

以下是响应：

```
{
  "detail": "Operation not allowed"
}
```

最后，让我们更新 DELETE 路由：

```
@event_router.delete("/{id}")
async def delete_event(id: PydanticObjectId, user: str = Depends(authenticate)):
    event = await event_database.get(id)
    if event.creator != user:
        raise HTTPException(
status_code=status.HTTP_404_NOT_FOUND,
    detail="Event not found"
)
```

在前面的代码块中，我们指示路由函数首先检查当前用户是否为创建者，否则抛出异常。让我们看一个另一个用户尝试删除其他用户事件的示例：

```
$ curl -X 'DELETE' \
  'http://0.0.0.0:8080/event/6265a83fc823a3c912830074' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiZmFzdGFwaUBwYWNRdC5jb20iLCJleHBpcmVzIjoxNjUwODM5NzYxLjI2NzgzMX0.MMR T6pwEDBVHTU5C1a6MV8j9wCfWhqbza9NBpZz08xE'
```

事件未找到作为响应返回：

```
{
  "detail": "Event not found"
}
```

然而，理想的所有者可以删除事件：

```
$ curl -X 'DELETE' \
  'http://0.0.0.0:8080/event/6265a83fc823a3c912830074' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoicmVhZGVyQHBhY2t0LmNvbSI sImV4cGlyZXMiOjE2NTA4MzQzOTUuMDkzMDI3fQ.IKYHWQ2YO3rQc-KR8kyfoy_54MsEVE75WbRqoVbdoW0'
```

以下是响应：

```
{
  "message": "Event deleted successfully."
}
```

我们已成功保护了应用程序及其路由。让我们通过在下一节中配置跨源资源共享（CORS）中间件来结束本章。

### Configuring CORS

跨源资源共享（CORS）充当一条规则，防止未注册的客户端访问资源。

当我们的 Web API 被前端应用程序使用时，浏览器将不允许跨源 HTTP 请求。这意味着资源只能从与 API 完全相同的源或 API 允许的源访问。

FastAPI 提供了一个 CORS 中间件，CORSMiddleware，允许我们注册可以访问我们 API 的域名。中间件接受一个将被允许访问服务器上资源的源数组。

> **What is a middleware?**
> A middleware is a function that acts as an intermediary between an operation. In web APIs, a middleware serves as an mediator in a request-response operation.

例如，要仅允许 Packt 访问我们的 API，我们在 origin 数组中定义 URL：

```
origins = [
    "http://packtpub.com",
    "https://packtpub.com"
]
```

要允许来自任何客户端的请求，origins 数组将只包含一个值，即星号（*）。星号是一个通配符，告诉我们的 API 允许来自任何地方的请求。

在 main.py 中，让我们配置我们的应用程序以接受来自任何地方的请求：

```
from fastapi.middleware.cors import CORSMiddleware

#### register origins

origins = ["*"]

app.add_middleware(
CORSMiddleware,
allow_origins=origins,
allow_credentials=True,
allow_methods=["*"],
allow_headers=["*"],
)
```

在上面的代码块中，我们首先从 FastAPI 导入 CORS Middleware 类。我们注册了 origins 数组，最后使用 add_middleware 方法将中间件注册到应用程序中。

> **More information**
> The FastAPI documentation has more details on CORS - https://fastapi.tiangolo.com/tutorial/cors/

我们已成功配置应用程序以允许来自世界各地任何源的请求。

### Summary

在本章中，我们学习了如何使用 OAuth 和 JWT 保护 FastAPI 应用程序。我们还学习了依赖注入是什么、它在 FastAPI 应用程序中的使用方式，以及如何保护路由免受未经授权的用户访问。我们还添加了一个 CORS 中间件以允许任何客户端访问我们的 API。我们使用了之前章节中的知识。

在下一章中，您将了解如何测试您的 FastAPI 应用程序。您将学习什么是测试应用程序、为什么应该测试应用程序以及如何测试 FastAPI 应用程序。

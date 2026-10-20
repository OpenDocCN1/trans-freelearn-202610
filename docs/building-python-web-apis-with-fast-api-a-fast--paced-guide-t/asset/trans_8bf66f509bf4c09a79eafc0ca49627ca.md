

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

# Register routes

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

## 事件路由

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

# Register routes

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


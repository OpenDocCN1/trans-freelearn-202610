

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

# Register routes

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

## 创建事件

让我们看一下步骤：

- 1. 在 `routes/events.py` 中，更新导入以包含 Event 表模型类以及 `get_session()` 函数。导入 `get_session()` 函数是为了让路由能够访问创建的 session 对象：

```
from fastapi import APIRouter, Depends, HTTPException,
Request, status

from database.connection import get_session

from models.events import Event, EventUpdate
```

### 什么是 Depends？

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

## 读取事件

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


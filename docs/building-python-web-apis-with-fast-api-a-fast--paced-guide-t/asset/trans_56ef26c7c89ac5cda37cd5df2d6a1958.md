

在运行此测试之前，让我们先暂时注释掉 conftest.py 中删除用户数据的那一行，因为它会导致经过身份验证的测试失败：

```
# await User.find_all().delete()
```

从你的终端启动 MongoDB 服务器并运行测试：

```
(venv)$ pytest tests/test_login.py
```

注册路由已成功测试：

![](img/85e42522e1d8d05fd8b981490d6d9056_174_0.png)

图 8.3 – 注册路由的成功测试运行

让我们继续编写登录路由的测试。在此期间，你可以快速调整测试响应，看看测试是否会失败！

## 测试登录路由

在注册路由的测试下方，让我们定义登录路由的测试。我们将先像第一个测试那样定义请求负载和请求头，然后再发起请求：

```
@pytest.mark.asyncio
async def test_sign_user_in(default_client: httpx.AsyncClient) -> None:
    payload = {
        "username": "testuser@packt.com",
        "password": "testpassword"
    }
```

```
}

headers = {
    "accept": "application/json",
    "Content-Type": "application/x-www-form-urlencoded"
}
```

接下来，我们将发起请求并测试响应：

```
response = await default_client.post("/user/signin",
    data=payload, headers=headers)

    assert response.status_code == 200
    assert response.json()["token_type"] == "Bearer"
```

让我们重新运行测试：

```
(venv)$ pytest tests/test_login.py
```

![](img/85e42522e1d8d05fd8b981490d6d9056_175_0.png)

图 8.4 – 两个路由的成功测试运行

让我们将用于登录的用户名改为错误的用户名，以确认测试会失败：

```
payload = {
    "username": "wronguser@packt.com",
    "password": "testpassword"
}
```

![](img/85e42522e1d8d05fd8b981490d6d9056_176_0.png)

图 8.5 – 因请求负载错误导致测试失败

我们已成功为注册和登录路由编写了测试。让我们继续测试活动规划 API 的 CRUD 路由。

## 测试 CRUD 端点

我们将首先创建一个名为 test_routes.py 的新文件：

```
(venv)$ touch test_routes.py
```

在新创建的文件中，添加以下代码：

```
import httpx
import pytest

from auth.jwt_handler import create_access_token
from models.events import Event
```

在前面的代码块中，我们导入了常规依赖项。我们还导入了 create_access_token（用户）函数和 Event 模型。由于某些路由受保护，我们将自行生成访问令牌。让我们创建一个在调用时返回访问令牌的新 fixture。该 fixture 的作用域为 module，这意味着它仅在测试文件执行时运行一次，而不会在每次函数调用时被调用。添加以下代码：

```
@pytest.fixture(scope="module")
async def access_token() -> str:
    return create_access_token("testuser@packt.com")
```

让我们创建一个向数据库添加事件的新 fixture。此操作是为了在测试 CRUD 端点之前运行初步测试。添加以下代码：

```
@pytest.fixture(scope="module")
async def mock_event() -> Event:
    new_event = Event(
        creator="testuser@packt.com",
        title="FastAPI Book Launch",
        image="https://linktomyimage.com/image.png",
        description="We will be discussing the contents of the FastAPI book in this event.Ensure to come with your own copy to win gifts!",
        tags=["python", "fastapi", "book", "launch"],
        location="Google Meet"
    )

    await Event.insert_one(new_event)

    yield new_event
```

## 测试 READ 端点

接下来，让我们编写一个测试函数来测试 /event 路由上的 GET HTTP 方法：

```
@pytest.mark.asyncio
async def test_get_events(default_client: httpx.AsyncClient, mock_event: Event) -> None:
    response = await default_client.get("/event/")
```

```
assert response.status_code == 200
assert response.json()[0]["_id"] == str(mock_event.id)
```

在前面的代码块中，我们正在测试事件路由路径，以检查在 mock_event fixture 中添加到数据库的事件是否存在。让我们运行测试：

```
(venv)$ pytest tests/test_routes.py
```

结果如下：

![](img/85e42522e1d8d05fd8b981490d6d9056_178_0.png)

图 8.6 – 成功测试运行

接下来，让我们编写 /event/{id} 端点的测试函数：

```
@pytest.mark.asyncio
async def test_get_event(default_client: httpx.AsyncClient,
mock_event: Event) -> None:
    url = f"/event/{str(mock_event.id)}"
    response = await default_client.get(url)

    assert response.status_code == 200
    assert response.json()["creator"] == mock_event.creator
    assert response.json()["_id"] == str(mock_event.id)
```

在前面的代码块中，我们正在测试检索单个事件的端点。传递的事件 ID 从 mock_event fixture 中获取，并将请求结果与 mock_event fixture 中存储的数据进行比较。让我们运行测试：

```
(venv)$ pytest tests/test_routes.py
```

结果如下：

![](img/85e42522e1d8d05fd8b981490d6d9056_179_0.png)

图 8.7 – 单个事件检索的成功测试运行

接下来，让我们编写创建新事件的测试函数。

## 测试 CREATE 端点

我们将首先定义函数并从之前创建的 fixture 中获取访问令牌。我们将创建将发送到服务器的请求负载、将包含内容类型以及授权头值的请求头。测试响应也将被定义，然后发起请求并比较响应。添加以下代码：

```
@pytest.mark.asyncio
async def test_post_event(default_client: httpx.AsyncClient,
access_token: str) -> None:
    payload = {
        "title": "FastAPI Book Launch",
        "image": "https://linktomyimage.com/image.png",
        "description": "We will be discussing the contents
of the FastAPI book in this event.Ensure to come
with your own copy to win gifts!",
        "tags": [
```

```
"python",
"fastapi",
"book",
"launch"
],
"location": "Google Meet",
}

headers = {
    "Content-Type": "application/json",
    "Authorization": f"Bearer {access_token}"
}

test_response = {
    "message": "Event created successfully"
}

response = await default_client.post("/event/new",
json=payload, headers=headers)

assert response.status_code == 200
assert response.json() == test_response
```

让我们重新运行测试文件：

```
(venv)$ pytest tests/test_routes.py
```

结果如下所示：

![](img/85e42522e1d8d05fd8b981490d6d9056_181_0.png)

图 8.8 – 成功 POST 请求测试运行

让我们编写一个测试来验证数据库中存储的事件数量（在本例中为 2）。添加以下内容：

```
@pytest.mark.asyncio
async def test_get_events_count(default_client: httpx.AsyncClient) -> None:
    response = await default_client.get("/event/")

    events = response.json()

    assert response.status_code == 200
    assert len(events) == 2
```

在前面的代码块中，我们将 JSON 响应存储在 events 变量中，其长度用于我们的测试比较。让我们重新运行测试文件：

```
(venv)$ pytest tests/test_routes.py
```

结果如下：

![](img/85e42522e1d8d05fd8b981490d6d9056_182_0.png)

图 8.9 – 确认事件数量的成功测试运行

我们已成功测试了 GET 端点 /event 和 /event/{id} 以及 POST 端点 /event/new。让我们接下来测试 /event/new 的 UPDATE 和 DELETE 端点。

## 测试 UPDATE 端点

让我们从 UPDATE 端点开始：

```
@pytest.mark.asyncio
async def test_update_event(default_client: httpx.AsyncClient, mock_event: Event, access_token: str) -> None:
    test_payload = {
        "title": "Updated FastAPI event"
    }

    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {access_token}"
    }
```


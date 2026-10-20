

```
url = f"/event/{str(mock_event.id)}"

response = await default_client.put(url,
json=test_payload, headers=headers)

assert response.status_code == 200
assert response.json()["title"] ==
test_payload["title"]
```

在前面的代码块中，我们通过从 `mock_event` fixture 中获取 ID 来修改数据库中存储的事件。然后我们定义请求负载和头部。在 `response` 变量中，请求被发起并获取的响应被比较。让我们确认测试是否正确运行：

```
(venv)$ pytest tests/test_routes.py
```

以下是结果：

![](img/85e42522e1d8d05fd8b981490d6d9056_183_0.png)

图 8.10 – UPDATE 请求的成功运行

> **提示**
> `mock_event` fixture 在作为 MongoDB 文档的 ID 时非常有用，因为每次向数据库添加文档时都会唯一生成 ID。

让我们更改预期响应以确认我们测试的有效性：

```
assert response.json()["title"] == "This test should fail"
```

重新运行测试：

```
(venv)$ pytest tests/test_routes.py
```

以下是结果：

![](img/85e42522e1d8d05fd8b981490d6d9056_184_0.png)

## 测试 DELETE 端点

最后，让我们为 DELETE 端点编写测试函数：

```
@pytest.mark.asyncio
async def test_delete_event(default_client: httpx.AsyncClient, mock_event: Event, access_token: str) -> None:
    test_response = {
        "message": "Event deleted successfully."
    }

    headers = {
        "Content-Type": "application/json",
```

```
"Authorization": f"Bearer {access_token}"
}

url = f"/event/{mock_event.id}"

response = await default_client.delete(url,
headers=headers)

assert response.status_code == 200
assert response.json() == test_response
```

与之前的测试一样，预期测试响应也被定义，以及头部。DELETE 路由被调用并比较响应。让我们运行测试：

```
(venv)$ pytest tests/test_routes.py
```

以下是结果：

![](img/85e42522e1d8d05fd8b981490d6d9056_185_0.png)

图 8.12 – 成功 DELETE 测试

为了确认文档确实已被删除，让我们添加一个最终测试：

```
@pytest.mark.asyncio
async def test_get_event_again(default_client: httpx.
AsyncClient, mock_event: Event) -> None:
```

```
url = f"/event/{str(mock_event.id)}"
response = await default_client.get(url)

assert response.status_code == 200
assert response.json()["creator"] == mock_event.creator
assert response.json()["_id"] == str(mock_event.id)
```

预期响应是失败。让我们试试：

```
(venv)$ pytest tests/test_routes.py
```

以下是结果：

![](img/85e42522e1d8d05fd8b981490d6d9056_186_0.png)

图 8.13 – 失败的测试响应

如前面的截图所示，该项目已无法在数据库中找到。现在你已成功为身份验证和事件路由实现了测试，请取消注释负责从数据库中清除用户数据的代码：

```
await User.find_all().delete()
```

更新最后一个测试：

```
assert response.status_code == 404
```

最后，让我们运行应用程序中存在的所有测试：

```
(venv) $ pytest
```

以下是结果：

![](img/85e42522e1d8d05fd8b981490d6d9056_187_0.png)

图 8.14 – 完整的测试在 0.57 秒内运行

现在我们已成功测试了 event-planner API 中包含的端点，让我们运行覆盖率测试以确定测试操作中涉及的代码百分比。

## 测试覆盖率

测试覆盖率报告有助于确定在测试过程中执行的代码百分比。让我们安装 coverage 模块，以便我们可以衡量我们的 API 是否得到充分测试：

```
(venv) $ pip install coverage
```

接下来，让我们通过运行以下命令生成覆盖率报告：

```
(venv) $ coverage run -m pytest
```

以下是结果：

![](img/85e42522e1d8d05fd8b981490d6d9056_188_0.png)

图 8.15 – 生成的覆盖率报告

接下来，让我们查看由 `coverage run -m pytest` 命令生成的报告。我们可以选择在终端或网页上查看报告，通过生成 HTML 报告。我们将两者都做。

让我们从终端查看报告：

```
(venv)$ coverage report
```

以下是结果：

![](img/85e42522e1d8d05fd8b981490d6d9056_188_1.png)

图 8.16 – 来自终端的覆盖率报告

从前面的报告中，百分比表示执行的和交互的代码量。让我们生成 HTML 报告，以便我们可以检查交互的代码块。

![](img/85e42522e1d8d05fd8b981490d6d9056_189_0.png)

图 8.17 – 生成 HTML 覆盖率报告

接下来，从浏览器打开 `htmlcov/index.html`。

![](img/85e42522e1d8d05fd8b981490d6d9056_189_1.png)

图 8.18 – 来自网页浏览器的覆盖率报告

让我们检查 `routes/events.py` 的覆盖率报告。点击它以显示。

![](img/85e42522e1d8d05fd8b981490d6d9056_190_0.png)

图 8.19 – 覆盖率报告显示已执行的代码为绿色，未触及的代码为红色

## 总结

在本章中，你已成功通过为身份验证路由和 CRUD 路由编写测试来测试 API。你学习了什么是测试以及如何使用 pytest 编写测试，pytest 是一个为 Python 应用程序构建的快速测试库。你还学习了 pytest fixtures 是什么，并在创建可重用的访问令牌和数据库对象以及在整个测试会话中保留应用程序实例时使用了它们。你能够断言 API HTTP 请求的响应并验证 API 的行为。最后，你学习了如何为测试生成覆盖率报告并区分测试会话期间运行的代码块。

现在你已具备测试 Web API 的知识，你已准备好通过部署渠道将应用程序发布到万维网。在下一章也是最后一章中，你将学习如何容器化你的应用程序并使用 Docker 和 docker-compose 在本地部署它。

## 9
### 部署 FastAPI 应用程序

在上一章中，你学习了如何为 FastAPI 应用程序中创建的 API 端点编写测试。我们首先学习了什么是测试，并使用 pytest 库介绍了单元测试的基础知识。我们还研究了如何消除重复并使用 fixtures 重用测试组件，然后继续设置我们的测试环境。我们在上一章的结尾通过为每个端点编写测试并在测试后检查测试覆盖率报告来测试它们。

在本章中，你将学习如何使用 Docker 和 docker-compose 在本地部署 FastAPI 应用程序。还添加了一个简短的部分，其中包含将应用程序部署到你选择的无服务器平台的外部资源。

在本章中，我们将涵盖以下主题：

- 部署准备
- 使用 Docker 部署
- 部署 Docker 镜像

## 技术要求

本章使用的代码可以在 https://github.com/PacktPublishing/Building-Python-Web-APIs-with-FastAPI/tree/main/ch09/planner 找到。

## 部署准备

部署通常标志着应用程序生命周期的结束。在部署我们的应用程序之前，我们必须确保为顺利部署所需的正确设置已到位。这些设置包括确保应用程序依赖项在 requirements.txt 文件中是最新的，配置环境变量等。

## 管理依赖项

在前几章中，我们安装了 beanie 和 pytest 等包。这些包在 requirements.txt 文件中缺失，该文件充当应用程序的依赖管理器。保持 requirements.txt 文件是最新的非常重要。

在 Python 中，开发环境中使用的包列表可以使用 pip freeze 命令检索。pip freeze 命令返回直接安装的所有包的列表以及每个已安装包的子依赖项。幸运的是，requirements.txt 文件可以手动维护，使我们能够仅列出主要包，从而使依赖管理变得更容易。

让我们在覆盖 requirements.txt 文件之前列出应用程序使用的依赖项：

```
(venv)$ pip freeze
anyio==3.5.0
asgi-lifespan==1.0.1
asgiref==3.5.0
attrs==21.4.0
bcrypt==3.2.2
cffi==1.15.0
python-multipart==0.0.5
...
```


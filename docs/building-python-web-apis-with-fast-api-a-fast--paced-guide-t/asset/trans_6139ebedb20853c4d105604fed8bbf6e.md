

在构建模板之前，让我们先在 FastAPI 应用中配置 Jinja：

+   1. 让我们修改 todo API 组件 todo.py 的 POST 路由：

```
from fastapi import APIRouter, Path, HTTPException,
status, Request, Depends
from fastapi.templating import Jinja2Templates
from model import Todo, TodoItem, TodoItems

todo_router = APIRouter()

todo_list = []

templates = Jinja2Templates(directory="templates/")

@todo_router.post("/todo")
async def add_todo(request: Request, todo: Todo =
Depends(Todo.as_form())):
    todo.id = len(todo_list) + 1
    todo_list.append(todo)
    return templates.TemplateResponse("todo.html",
    {
        "request": request,
        "todos": todo_list
    })
```

+   2. 接下来，更新 GET 路由：

```
@todo_router.get("/todo", response_model=TodoItems)
async def retrieve_todo(request: Request):
    return templates.TemplateResponse("todo.html", {
        "request": request,
        "todos": todo_list
    })

@todo_router.get("/todo/{todo_id}")
async def get_single_todo(request: Request, todo_id: int
= Path(..., title="The ID of the todo to retrieve.")):
```

```
for todo in todo_list:
    if todo.id == todo_id:
        return templates.TemplateResponse(
            "todo.html", {
                "request": request,
                "todo": todo
            })
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail="Todo with supplied ID doesn't exist",
    )
```

在前面的代码块中，我们已配置 Jinja 在 templates 目录中查找模板，以提供传递给 templates.TemplateResponse() 方法的模板。

用于添加 todo 的 POST 方法也已更新，以包含对输入的依赖。依赖将在第 6 章《连接到数据库》中详细介绍。

+   3. 在 model.py 中，在 Config 子类之前添加高亮代码：

```
from typing import List, Optional

class Todo(BaseModel):
    id: Optional[int]
    item: str

    @classmethod
    def as_form(
        cls,
        item: str = Form(...)
    ):
        return cls(item=item)
```

现在我们已更新 API 代码，让我们编写模板。我们将从下一步开始编写基础 home.html 模板。

4. 在 home.html 中，我们将首先声明文档类型：

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Packt Todo Application</title>
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/css/bootstrap.min.css" integrity="sha384-9gVQ4dYFwwWSjIDZ9bE..." crossorigin="anonymous">
        <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.10/css/all.css" integrity="sha384-+d0P83n9kaQMCwj8F4RJB66tzIwOKmrdb46+porD/OvrJ+37WqIM7UoBtwH06Nlg" crossorigin="anonymous">
    </head>
```

5. 下一步是为模板正文编写内容。在模板的正文中，我们将在 <header></header> 标签下包含应用程序的名称，并将指向子模板的 todo_container 的链接包裹在 block 标签中。子模板将在第 8 步中编写。

在 home.html 模板文件中，紧接在 </head> 标签后包含以下代码：

```
<body>
    <header>
        <nav class="navbar">
            <div class="container-fluid">
                <center>
```

```
<h1>Packt Todo
Application</h1>
</center>
</div>
</nav>
</header>
<div class="container-fluid">
{% block todo_container %}{% endblock %}
</div>
</body>
</html>
```

高亮代码告诉父模板 todo_container 块将由子模板定义。包含 todo_container 块并扩展父模板的子模板，其内容将在此处显示。

6. 要查看更改，请激活虚拟环境并启动应用程序：

```
$ source venv/bin/activate
(venv)$ uvicorn api:app --host=0.0.0.0 --port 8000 --reload
```

7. 打开 http://127.0.0.1:8000/todo 以预览更改：

![](img/85e42522e1d8d05fd8b981490d6d9056_79_0.png)

图 4.2 – Todo 应用程序主页

8. 接下来，让我们在 todo.html 中编写 todo 模板：

```
{% extends "home.html" %}

{% block todo_container %}
<main class="container">
    <hr>
    <section class="container-fluid">
        <form method="post">
            <div class="col-auto">
                <div class="input-group mb-3">
                    <input type="text" name="item"
                           value="{{ item }}" class="form-control" placeholder="Purchase Packt's Python workshop course"
                           aria-label="Add a todo"
                           aria-describedby="button-addon2" />
                    <button class="btn btn-outline-primary" type="submit" id="button-addon2" data-mdb-ripple-color="dark">
                        Add Todo
                    </button>
                </div>
            </div>
        </form>
    </section>
{% if todo %}
    <article class="card container-fluid">
        <br/>
        <h4>Todo ID: {{ todo.id }} </h4>
        <p>
            <strong>
                Item: {{ todo.item }}
            </strong>
```

```
</p>
</article>
{% else %}
    <section class="container-fluid">
        <h2 align="center">Todos</h2>
        <br>
        <div class="card">
            <ul class="list-group list-group-flush">
                {% for todo in todos %}
                    <li class="list-group-item">
                        {{ loop.index }}. <a href="/todo/{{loop.index}}"> {{
                        todo.item }} </a>
                    </li>
                {% endfor %}
            </ul>
        </div>
    {% endif %}
</section>
</main>
{% endblock %}
```

在前面的代码块中，todo 模板继承了主页模板。我们还定义了 todo_container 块，其内容将在父模板中显示。

todo 模板同时被用于检索所有 todos 和单个 todo 的路由。因此，模板根据使用的路由渲染不同的内容。

在模板中，Jinja 使用 {% if todo %} 块检查是否传递了 todo 变量。如果传递了 todo 变量，则渲染 todo 详情，否则渲染 {% else %} 块中的内容，即 todos 列表。

9. 刷新 Web 浏览器以查看最近的更改：

![](img/85e42522e1d8d05fd8b981490d6d9056_82_0.png)

图 4.3 – 更新 todo 主页

10. 让我们添加一个 todo 以验证主页按预期工作：

![](img/85e42522e1d8d05fd8b981490d6d9056_82_1.png)

图 4.4 – 显示的 todos 列表

11. todo 是可点击的。点击 todo，你应该会看到以下页面：

![](img/85e42522e1d8d05fd8b981490d6d9056_83_0.png)

图 4.5 – 单个 todo 页面

我们已成功向 FastAPI 应用程序添加了模板。

# 摘要

在本章中，我们学习了什么是模板化、Jinja 模板系统的基础知识，以及如何在 FastAPI 中使用它。我们利用本章第一部分学到的基础知识来决定渲染什么内容。我们还学习了什么是模板继承，以及它是如何工作的，使用主页和 todo 模板作为示例。

在下一章中，你将被介绍 FastAPI 中的应用程序结构。在本章中，你将使用本章和前几章的知识构建一个计划应用程序。你将首先被介绍应用程序是如何结构化的，然后再继续构建计划应用程序。

# 第 2 部分：
构建和
保护 FastAPI
应用程序

完成本部分后，你将能够使用 FastAPI 构建一个功能齐全且安全的应用程序。本部分使用前一部分的知识，并将其强化到构建比第二章中构建的应用程序更复杂、功能更强的应用程序。你还将能够集成并连接到 SQL 和 NoSQL（MongoDB）数据库，并在本部分结束时能够保护 FastAPI 应用程序。

本部分包含以下章节：

+   - 第 5 章，FastAPI 应用程序结构
+   - 第 6 章，连接到数据库
+   - 第 7 章，保护 FastAPI 应用程序


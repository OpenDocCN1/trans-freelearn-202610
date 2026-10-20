

在本章结束时，你将能够为 FastAPI 应用添加一层身份验证。本章将解释通过哈希保护密码、添加身份验证层以及保护未通过身份验证用户访问路由的过程。在本章中，我们将涵盖以下主题：

- FastAPI 中的身份验证方法
- 使用 OAuth2 和 JWT 保护应用程序
- 使用依赖注入保护路由
- 配置 CORS

## 技术要求

要跟上进度，需要 MongoDB 数据库组件。适用于你操作系统的安装程序可以在其官方文档中找到。本章使用的代码可以在 https://github.com/PacktPublishing/Building-Python-Web-APIs-with-FastAPI/tree/main/ch07/planner 找到。

## FastAPI 中的身份验证方法

FastAPI 中有多种身份验证方法可用。FastAPI 支持常见的身份验证方法，即基本 HTTP 身份验证、Cookie 和 Bearer 令牌身份验证。让我们简要了解一下每种方法的内容：

- **基本 HTTP 身份验证：** 在这种身份验证方法中，用户凭据，通常是用户名和密码，通过 Authorization HTTP 头发送。请求反过来返回一个包含 Basic 值和可选 realm 参数的 WWW-Authenticate 头，该参数表示进行身份验证请求的资源。
- **Cookies：** 当数据需要在客户端存储时，例如在 Web 浏览器中，就会使用 Cookie。FastAPI 应用也可以使用 Cookie 来存储用户数据，服务器可以检索这些数据用于身份验证目的。
- **Bearer 令牌身份验证：** 这种身份验证方法涉及使用称为 Bearer 令牌的安全令牌。这些令牌与 Bearer 关键字一起发送在 Authorization 头请求中。最常用的令牌是 JWT，它通常是一个包含用户 ID 和令牌过期时间的字典。

这里列出的每种身份验证方法都有其特定的用例以及优缺点。然而在本章中，我们将使用 Bearer 令牌身份验证。

身份验证方法作为依赖项注入到 FastAPI 应用中，并在运行时调用。这仅仅意味着当身份验证方法被定义时，它们处于休眠状态，直到被注入到使用位置。这种活动称为 **依赖注入。**

### 依赖注入

依赖注入是一种模式，其中一个对象——在本例中为函数——接收实例变量以供函数进一步执行所需。

在 FastAPI 中，依赖项是通过在路径操作函数参数中声明它们来注入的。我们在前几章中使用了依赖注入。以下是上一章中的一个示例，我们从传递给函数的用户模型中检索 email 字段：

```
@user_router.post("/signup")
async def sign_user_up(user: User) -> dict:
    user_exist = await User.find_one(User.email == user.email)
```

在此代码块中，定义的依赖项是 User 模型类，它被注入到 sign_user_up() 函数中。通过将 User 模型注入到 user 函数参数中，我们可以轻松检索对象的属性。

### 创建和使用依赖项

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

## 使用 OAuth2 和 JWT 保护应用程序

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

### 哈希密码

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
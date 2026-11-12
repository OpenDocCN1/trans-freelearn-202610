

好吧，现在可以进行身份验证了吗？让我们试一下。访问 http://localhost:8000/inventory，你将看到如图 9.3 所示的页面：

![](img/e6cf5550804ccae9d1859270da8688f5_242_0.png)

图 9.3 – 库存模块的登录页面

尝试用随机值填写字段并单击登录按钮。结果将是一个带有错误消息的窗口，如图 9.4 所示：

![](img/e6cf5550804ccae9d1859270da8688f5_242_1.png)

图 9.4 – 登录尝试后的错误消息

那么，问题出在哪里？如果你通过浏览器调试器检查 HTTP 响应，将看到以下错误消息：Class "Laminas\\Session\\Container" not found。如果你运行测试，也会看到这条消息：

```
1) InventoryTest\Controller\
IndexControllerTest::testLoginActionCanBeAccessed
Failed asserting response code "302", actual status code is
"500"

Exceptions raised:
Exception 'Error' with message 'Class "Laminas\\Session\
Container" not found' in /opt/lampp/htdocs/whatstore/vendor/
laminas/laminas-authentication/src/Storage/Session.php:62
```

此错误发生在 laminas-authentication 的一个类中。但是，等等！如果 laminas-authentication 需要这个 Container 类，为什么 Composer 没有安装它？这是因为 Laminas 组件试图在它们之间保持弱耦合。你不需要使用 Container 来存储身份验证数据。你可以在 AuthenticationService 构造函数中使用另一个类。可以将另一个存储类注入到 AuthenticationService 构造函数中。但是，由于我们不使用自定义存储类，laminas-authentication 需要一个，并默认要求在另一个组件 – laminas-session 中提供实现。

没问题。我们在下一章也将使用 laminas-session。添加并安装 laminas-session 组件，方法与本章使用 Composer 安装 laminas-authentication 组件类似。在 Eclipse 中打开 composer.json 文件，通过“依赖项”选项卡搜索 laminas/laminas-session 包并添加它。不要忘记保存文件并单击“更新依赖项”按钮。你是不是又忘记保存文件了，对吧？别担心，我们会重复必要的次数。

安装 laminas-session 后，你可以通过 Web 界面再次尝试登录。结果将是一个显示“Invalid data!”消息的窗口，如图 9.5 所示：

![](img/e6cf5550804ccae9d1859270da8688f5_243_0.png)

图 9.5 – 登录尝试后的警告消息

当然，此刻，无论你输入什么昵称和密码都是无效数据，因为没有注册的员工。为此，我们需要创建员工注册页面。这就是我们将在下一节中要做的事情。

## 创建员工注册

也许我们在上一节中做出了错误的承诺。事实上，想象你已经为员工实现了 CRUD 操作，类似于我们在第 7 章“请求控制和数据视图”中为产品所做的。还想象我们使用 laminas-form、laminas-filter 和 laminas-validation 实现了这个 CRUD 操作，类似于我们在第 8 章“创建表单并实现过滤器和验证器”中为产品折扣所做的。这并不难想象，因为其源代码实际上可在“技术要求”部分提到的仓库中找到。由于我们使用模式，CRUD 实现的结构是相同的。因此，我们假设 module/Inventory/src/Controller 文件夹中有以下类：

- EmployeeAPIController
- EmployeeAPIControllerFactory
- EmployeeController
- EmployeeControllerFactory

请注意，我们有两对类 – 一个 API 控制器及其工厂，以及一个 Web 界面控制器及其工厂。这与我们用于产品控制器层实现的结构完全相同。这些控制器类由 module/Inventory/test/Controller 文件夹中的以下类进行测试：

- EmployeeAPIControllerTest
- EmployeeControllerTest

我们还为折扣实现了一个视图层，这与我们为产品制作的非常相似。因此，我们假设 module/Inventory/view/inventory/employee 文件夹中有以下视图脚本：

- edit.js
- edit.phtml
- index.js
- index.phtml

在这里，我们有两对文件，生成两个网页 – 插入/更新页面和列表页面。.phtml 文件包含 HTML 和 PHP 代码的组合，而 .js 文件专用于 JavaScript 代码。

好，现在我们可以注册员工了。我们可以插入一个员工并检查他们是否可以登录和登出。我们将遵循以下步骤执行：

- 1. 首先，访问 http://localhost:8000/inventory/employee。此 URL 将带你到达如图 9.6 所示的页面：

![](img/e6cf5550804ccae9d1859270da8688f5_245_0.png)

图 9.6 – 员工列表页面

- 2. 单击“添加员工”选项。此操作将带你到达如图 9.7 所示的页面。在此示例中，我们正在添加一名名为 Steven Grant 的员工，其昵称为 Mark Spektor，密码为“moonknight”。

![](img/e6cf5550804ccae9d1859270da8688f5_245_1.png)

图 9.7 – 员工插入表单

- 3. 单击**保存**按钮后，你将看到一条确认插入的消息，如我们在*图 9.8*中看到的：

![](img/e6cf5550804ccae9d1859270da8688f5_246_0.png)

*图 9.8 – 员工插入的确认消息*

最后，你将在列表页面看到插入的员工。请注意，过滤器已将字母转换为大写，如*图 9.9*所示：

![](img/e6cf5550804ccae9d1859270da8688f5_246_1.png)

*图 9.9 – 包含插入员工的员工列表页面*

太棒了！现在我们有一个用于身份验证的用户了。让我们在下一节中使用此用户来测试我们的登录页面。

## 测试登录页面

根据示例，我们访问 http://localhost:8000/inventory 并使用插入员工的数据填写登录表单。

当你单击**登录**按钮后出现一条显示**“无效数据！”**的消息时，你可能会感到沮丧。“但哪里出了问题？”你可能会想。你一定正确输入了员工昵称和密码，对吧？我相信你。事实上，与密码有一个小细节 – 它在插入员工时会被加密。因此，我们必须将员工提供的加密密码与存储在 *employees* 表中的加密密码进行比较。

此比较需要对 Employee 类进行一些更改。你应该记得 Employee 类有一个私有 encrypt 方法。我们将把此方法修改为 public 和 static，如下所示：

```
public static function encrypt(String $text)
{
    $text = strrev(hash('sha256', $text));
    $subtext = substr($text,1,rand(0,strlen($text)-1));
    $text = substr($subtext . $text, 0, strlen($text));
    return hash('md5', $text);
}
```

由于 encrypt 方法被同一类的 exchangeArray 方法使用，我们还必须修改后者，将 $this 关键字更改为 self：

```
public function exchangeArray($data)
{
    $this->ID = ($data['ID'] ?? 0);
    $this->name = ($data['name'] ?? '');
    $this->nickname = ($data['nickname'] ?? '');
    $this->password = (isset($data['password']) ?
        self::encrypt($data['password']) : '');
}
```

好了，现在我们可以在 IndexController->loginAction 方法中使用 encrypt 方法。接下来，我们必须替换以下行：

```
$this->adapter->setCredential($password);
```

它需要替换为：

```
$this->adapter->setCredential(Employee::encrypt($password));
```

再次尝试登录。再次，你感到失望；你又看到了**无效数据**消息。哪里出了问题？嗯，如果你仔细查看 Employee::encrypt 方法，你会注意到它为相同的密码生成不同的哈希值，因为存在一个随机组件。随机组件非常适合向用户推荐密码，但当加密数据需要相同时，它们不适合用于比较。因此，当你尝试登录时，生成的哈希值与数据库中存储的哈希值不同。


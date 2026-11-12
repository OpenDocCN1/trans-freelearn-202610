

## 在 GAE 上部署 Spring Web MVC 应用程序

自 Spring Roo 1.1.5 起，Roo 生成的 Spring Web MVC 应用程序无法在 GAE 上运行。其原因与 GAE 和 Roo 生成的 Spring MVC 应用程序所使用的 JSTL 版本不匹配有关。此外，Roo 生成的 JPA 实体仅支持非拥有关系（有关拥有关系和非拥有关系的信息，请参阅 http://code.google.com/appengine/docs/java/datastore/jdo/relationships.html）。在本示例中，我们将探讨一个 Spring 3.0 Web MVC 应用程序（该应用使用可在 GAE 上运行的 JSTL 标签），该应用由 FlightDescription 和 Flight JPA 实体组成，并演示如何在 JPA 实体之间创建单向拥有一对多关系。FlightDescription 实体位于一对多关系的“一”端。

如果您使用的是 Spring Roo 1.2.x，则 Roo 生成的 Spring Web MVC 应用程序可以成功部署到 GAE 上。

## 准备工作

如果您仅想使用适用于 Java 的 App Engine SDK 在本地运行 Spring Web MVC 应用程序，则无需注册 Google App Engine 并创建应用程序标识符。如果您想将应用程序部署到 GAE，请按照前面示例中描述的步骤操作，并确保修改 WEB-INF/appengine-web.xml 文件中的应用程序标识符。建议您将应用程序部署到 GAE 服务器上，因为我们还将讨论管理控制台提供的某些功能。

将随本书附带的 ch06-gae-spring-mvc.zip 文件解压到 C:\roo-cookbook 目录。解压 ZIP 文件将创建一个名为 ch06-gae-spring-mvc 的目录，其中包含 flightapp-gae-spring-mvc Eclipse 项目。flightapp-gae-spring-mvc 项目使用 App Engine SDK 版本 1.4.2 和 Maven GAE Plugin 0.8.2。

## 操作步骤

要在 GAE 上部署 Spring Web MVC 应用程序，请按照以下步骤操作：

- 1. 打开命令提示符并进入 c:\roo-cookbook\ch06-gae-spring-mvc 目录。执行 mvn gae:run 命令，将 flightapp-gae-spring-mvc 项目部署到 App Engine 开发 Web 服务器：
    C:\roo-cookbook\ch06-gae-spring-mvc> mvn gae:run

- 2. 开发 Web 服务器成功启动后，打开 Web 浏览器并访问以下 URL：http://localhost:8080。如果看到以下页面，则表示您的 flightapp-gae-spring-mvc 项目已成功部署：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_355_0.png)

- 3. 给定的截图显示了我们的航班预订应用程序的主页，当前允许管理 Flight 和 FlightDescription 对象。选择 **管理航班描述** 超链接允许用户创建和查看 FlightDescription 实例。此外，它还允许用户添加与 FlightDescription 实例关联的子 Flight 实例。**管理航班** 选项允许用户创建和删除 Flight 实例。

- 4. 选择 **管理航班描述** 选项以查看应用程序中的 FlightDescription 实例，如下所示：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_355_1.png)

- 5. 由于我们尚未创建任何 FlightDescription 实例，表格显示 **未找到记录** 消息。

- 6. 选择 **创建航班描述** 选项以查看创建新 FlightDescription 实例的表单，如下所示：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_355_2.png)

- 7. 按给定截图所示输入 **FlightDescription** 详细信息，然后单击 **创建** 按钮。新创建的 **FlightDescription** 实例将显示在 **FlightDescription** 实例列表中，如下所示：

| 出发地 | 目的地 | 价格 | 操作 |
|---|---|---|---|
| NYC | DELHI | 1200 | 查看 删除 |

- 8. **FlightDescription** 记录对应的 **查看** 按钮显示 **FlightDescription** 详细信息以及与 **FlightDescription** 实例关联的 **Flight** 详细信息。**删除** 按钮将删除 **FlightDescription** 实例以及关联的 **Flight** 实例。

- 9. 单击 **主页** 链接返回应用程序的主页，并选择 **管理航班** 链接以查看 **Flight** 实例，如下所示：

| 航班名称 | 操作 |
|---|---|
| 未找到记录 | |

- 10. 选择 **创建航班** 链接以查看创建 **Flight** 实例的表单，如下所示：

| 字段 | 值 |
|---|---|
| 航班名称 | FLT-100 |
| 航班描述 | NYC - DELHI 仅需 1200 美元 |

- 11. 将 **FLT-100** 输入为 **航班名称** 字段的值，并将 **NYC – DELHI 仅需 1200 美元** 选择为 **航班描述** 字段的值。单击 **创建** 按钮以创建 **Flight** 实例（航班名称为 **FLT-100**），它是 **FlightDescription** 实例（出发地：NYC，目的地：DELHI，价格：1200）的子实例。同样，创建名为 **FLT-200**、**FLT-300** 和 **FLT-400** 的航班。现在，如果您转到 **FlightDescription** 列表页面并单击 **查看** 按钮，则将看到新创建的 **Flight** 实例作为 **NYC-DELHI-1200 FlightDescription** 实例的子实例，如下所示：

| 航班描述 | | |
|---|---|---|
| 出发地 | 目的地 | 价格 |
| NYC | DELHI | 1200 |
| 关联航班 | | |
| 航班名称 | | |
| FLT-100 | | |
| FLT-200 | | |
| FLT-300 | | |
| FLT-400 | | |

- 12. 给定的截图显示了与 FlightDescription 实例关联的 Flight 实例。

- 13. 如果您现在通过选择对应于 NYC-DELHI-1200 FlightDescription 实例的 Delete 按钮（如下一个截图所示）来删除 FlightDescription 实例，则它也将删除关联的 Flight 实例。

| 出发地 | 目的地 | 价格 | 操作 |
|---|---|---|---|
| NYC | DELHI | 1200 | 查看 删除 |

## 工作原理

GAE 数据存储是一个非关系型数据库，其中实体之间的关系被建模为拥有关系或非拥有关系。拥有关系基于父子关系的概念，其中子实例不能在没有父实例的情况下存在。在非拥有关系中，实例可以独立于与其他实体的关系而存在。

拥有关系最好可视化为树结构，其中树的根是一个实体实例，是所有实体的最终父实例。以下图示展示了一个拥有关系的示例，其中 FlightDescription 是根实体，Flight 实体实例为其子实例，Booking 实体实例是 Flight 实体的子实例。

![](img/b8c379a2ed921ead3aa2b01811dc29fa_358_0.png)

在给定的图中，FlightDescription (NYC-DELHI-1200) 位于树的根部。Flight 实例 (FLT-100, FLT-200, FLT-300, FLT-400) 是 FlightDescription (NYC-DELHI-1200) 实例的子实例。Booking (ashish, greg) 实例是 Flight (FLT-400) 实例的子实例。如果在 App Engine 数据存储中存储的实体定义了这种父子关系，则这些实体被称为属于同一实体组。您可以将实体组视为树，树顶部是一个根实体，该根实体没有任何父实例。在此图中，FlightDescription (NYC-DELHI-1200) 表示根实体。关于拥有关系的另一个重要点是，除了根实体外，实体组中的所有其他实体都不能在没有父实例的情况下创建。例如，Flight 实体实例不能在没有 FlightDescription 实例的情况下存在，Booking 实例不能在没有 Flight 实例的情况下存在。

在 GAE 中，创建拥有关系会限制您可以为子实体定义的主键类型。其原因是，子实体实例除了需要知道其自身实体 ID 外，还需要知道其在实体组中的父实例。拥有关系中子实体的主键类型为 com.google.appengine.api.datastore.Key 或 com.google.appengine.api.datastore.Key 的编码字符串形式。GAE 提供了一个 KeyFactory 类（将在本示例后面讨论），您可以使用它来创建 Key 实例，或将 Key 实例值转换为字符串（称为 Key 的编码形式）并反之亦然。


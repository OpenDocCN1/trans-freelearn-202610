
## 我们社区框架的企业级方法

我们需要为处理的每个部分创建组合逻辑，例如日志（Log）、缓存（Cache）、电子邮件（Email）、配置（Configuration）、重定向器（Redirector）等。因此，为了不在代码中到处重复组合逻辑，我们将创建一个静态类来集中组合逻辑。

```csharp
//Fisharoo/Common/MEFManager.cs
using System;
using System.ComponentModel.Composition;
using System.ComponentModel.Composition.Hosting;
using System.IO;

namespace Fisharoo.Common
{
    public static class MEFManager
    {
        public static void Compose(object o)
        {
            var container = new CompositionContainer(
                new DirectoryCatalog(Path.Combine(
                AppDomain.CurrentDomain.BaseDirectory, "bin")));

            var batch = new CompositionBatch();
            batch.AddPart(o);
            container.Compose(batch);
        }
    }
}
```

为了从目录读取程序集，使用了 **DirectoryCatalog()** 方法。一旦找到为 **IRedirector** 提供实现的程序集，它就会被程序拾取，并通过 **container.ComposeParts()** 组合到应用程序中。

我们从 **Application_Start()** 方法中调用此组合逻辑。

```csharp
void Application_Start(object sender, EventArgs e)
{
    // Code that runs on application startup
    MEFManager.Compose(this);
    log.EnsureInitialized();
}
```

### 数据访问层

为了实现上面领域驱动设计部分所讨论的领域模型，我们将使用实体框架（Entity Framework）创建数据访问层。为此，我们需要在解决方案中添加一个新项目。我们将其命名为 DataAccess。

在这个项目中，我们将创建两个类和一个 edmx 文件。第一个类是我们的连接对象（命名为 Connection.cs），第二个文件将命名为 PersonRepository.cs。创建好这些类文件后，右键单击 DataAccess 项目并选择“添加新项”来添加新的 ADO.NET 实体数据模型。在“数据”选项卡中选择“ADO.NET 实体数据模型”，如下面的截图所示：

![](img/15a62e134135dde036b0ac3cbb01dab6_74_0.png)

我们将其命名为 Fisharoo.edmx。之后，向导将引导你完成以下解释的各种步骤。由于我们已经创建了数据库中的表，并且希望模型基于该表，因此我们将选择第一个屏幕中的**从数据库生成**，然后点击**下一步**。选择适当的连接字符串设置，然后点击**下一步**。

在下一个屏幕中，按如下方式从 **Tables | Person** 中选择数据库对象，并将模型命名空间命名为 **FisharooModel**，然后点击**完成**。

这为一些演示提供了基础框架。现在，让我们深入研究我们创建的两个类文件——**Connection.cs** 文件和 **PersonRepository.cs** 文件。**Connection** 类为我们提供了一种封装 **DataContext** 创建的方式。它有一个方法返回 **FisharooDataContext** 对象。如果将来有更多的数据上下文需要使用，它们也可以放在这里，我们可以将当前的 **GetContext()** 方法重命名为更有意义的名字。

```csharp
//Fisharoo/DataAccess/Connection.cs
namespace Fisharoo.DataAccess
{
    public class Connection
    {
        public static FisharooDataContext GetContext()
        {
            FisharooDataContext fdc = new FisharooDataContext(ConfigurationManager.ConnectionStrings["FisharooConnectionString"].ToString());
            return fdc;
        }
    }
}
```

请注意，我们正在创建一个新的 **FisharooDataContext** 类实例，并引用之前放在 **web.config** 文件中的连接字符串。

现在，让我们转向 **PersonRepository** 类。我们之前创建的 Person 表仅供演示之用，这个 repository 也是如此。以下是该类。

```csharp
//Fisharoo/DataAccess/PersonRepository.cs
using System.Collections.Generic;
using System.ComponentModel.Composition;
using System.Linq;
using Fisharoo.Interfaces;

namespace Fisharoo.DataAccess
{
    [Export(typeof(IPersonRepository))]
    public class PersonRepository : IPersonRepository
    {
        public List<string> GetAllNames()
        {
            List<string> names = new List<string>();
            FisharooDataContext dc = Connection.GetContext();

            var people = from p in dc.People
                         select p;

            foreach (Person p in people)
            {
                names.Add(p.FirstName + " " + p.LastName);
            }

            return names;
        }
    }
}
```

请注意，它有一个 Export 属性，以便我们以后需要时可以替换实现。与所有 MEF 类一样（出于良好的设计原因），我们也继承自 IPersonRepository 接口，这清晰地定义了这个类。

现在我们的 repository 中只有一个方法。它负责简单地从 Person 表中选择所有人，并返回这些人的名字和姓氏列表。我们有一个字符串类型的 names 集合列表，将在方法末尾返回。然后我们获取 DataContext 用于查询。一旦定义了 DataContext，就可以执行查询。

我们使用 var 关键字声明一个局部变量。这允许我们声明动态类型的变量。在这种情况下，我们将选择 Person 类的集合。但我们并不确定！而且也不需要知道。

我们在代码中看到的查询是从 Person 表中选择所有人并返回这些人的集合。它非常像 SQL，但顺序相反，即 from 在前，select 在后。LINQ 查询就是这样编写的，正是由于这种顺序，Visual Studio 智能感知才能正常工作。

查询执行后，结果会被获取到 people 变量中，其类型为 ObjectSet<Person>。这帮助我们可以在循环中遍历它。当我们创建 edmx 时，实体框架会自动创建 People ObjectSet 作为 FisharooDataContext 类的一部分。

最后，在代码中有一个循环，我们将每个人的名字和姓氏连接起来，并将它们添加到 names 集合中。完成后，我们简单地将该集合返回给此方法的使用者。

这里给出的演示非常简单，因为我们不想让你感到困惑。你会了解 Repository 在此应用程序中的工作方式。我们还涉及了 LINQ 的基础知识。

此时，解决方案结构如下面的截图所示：

![](img/15a62e134135dde036b0ac3cbb01dab6_78_0.png)

### 摘要

在本章中，我们介绍了创建一个企业级框架来满足大多数 Web 应用程序的基本需求。我们讨论了架构分层、领域驱动设计、相关模式和最佳实践。我们还通过一些代码为将在应用程序中使用的基本框架类创建了组件。我们讨论了如何使用一些新技术和现有模式来处理数据访问。我们还了解了如何在表示层使用 MVP 模式。本章应为你提供坚实的基础，在此基础上我们现在可以开始构建应用程序。

## 用户账户

没有人，你的社区就不存在！

任何社区网站要被认为成功，首先必须拥有一群忠实的用户。尽管还有其他衡量标准，但在大多数情况下，社区的人口越多，就被认为越成功。因此，我们应该创建一种方式让用户来到我们的站点、创建账户，并成为社区的一部分。下面的截图显示了注册页面的外观：

![](img/15a62e134135dde036b0ac3cbb01dab6_79_0.png)


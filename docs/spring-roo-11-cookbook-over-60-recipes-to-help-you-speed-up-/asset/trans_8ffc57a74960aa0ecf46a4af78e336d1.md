

privileged aspect Passenger_Roo_JavaBean
{
    public String Passenger.getFirstName()
    {
        return this.firstName;
    }

    public void Passenger.setFirstName(String firstName)
    {
        this.firstName = firstName;
    }

    ...
}
```

给定的代码显示，Passenger_Roo_JavaBean.aj 已被 Spring Roo 更新，以便为添加到 Passenger 类的每个字段引入 getter 和 setter 方法。之所以能实现此功能，是因为 Passenger 类中存在 @RooJavaBean 注解。

以下代码显示 Passenger_Roo_ToString.aj AspectJ ITD，当向 Passenger 类添加字段时，它也被 Spring Roo 修改了：

```
privileged aspect Passenger_Roo_ToString
{
    public String Passenger.toString()
    {
        StringBuilder sb = new StringBuilder();
        sb.append("FirstName: ").append(getFirstName()).append(", ");
        sb.append("LastName: ").append(getLastName()).append(", ");
        sb.append("Age: ").append(getAge()).append(", ");
        sb.append("Address: ").append(getAddress());
        return sb.toString();
    }
}
```

如给定代码所示，它为 Passenger 类引入了一个 toString 方法，该方法返回一个包含每个属性值的拼接字符串。之所以能实现此功能，是因为 Passenger 类使用了 @RooToString 注解。

### 如果使用 IDE 添加属性怎么办？

Spring Roo 会主动监视带有 Roo 注解的类的变化，任何类的变化都会触发 Spring Roo 更新相应的 AspectJ ITD 文件。因此，无论你是使用 Roo shell 还是 IDE 向 Java 类添加属性，都无关紧要。

以下图显示了 Spring Roo 如何管理 AspectJ ITD 文件：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_52_0.png)

给定的图显示，当你从某个目录启动 Spring Roo shell 时，它会主动监视文件系统中带有 Roo 注解（例如 @RooToString、@RooJavaBean 等）的 Java 类。当这些 Java 类中的任何一个使用 IDE 或其他编辑器修改时，Spring Roo 会检查与这些 Java 类对应的 AspectJ ITD 文件（遵循 *_Roo_*.aj 命名约定，如前所述）是否与 Java 类保持同步。如果不同步，它会相应地更新 AspectJ ITD 文件。Spring Roo 使用插件来对 AspectJ ITD 文件进行修改。

## 还有更多内容...

本示例表明，如果你希望 Roo 自动为所有属性生成 `toString` 方法以及 getter 和 setter 方法，则请使用 `@RooToString` 和 `@RooJavaBean` 注解对类进行注解。

> **如果 Spring Roo 未运行时添加了属性怎么办？**
> 
> 当你启动 Roo shell 时，它会检查 AspectJ ITD 是否与相应的 Java 类保持同步。如果存在差异，Roo 会更新 AspectJ ITD 文件以反映 Java 类的当前状态。此时，如果 Roo 发现某个 ITD 文件不再需要，它甚至可能会删除该 ITD 文件。例如，如果你从 `Passenger` 类中移除所有属性，那么相应的 `Passenger_Roo_JavaBean.aj` 和 `Passenger_Roo_ToString.aj` 文件会被 Roo 自动删除。

Spring Roo 不提供用于移除或修改属性的命令。因此，如果你想要移除或修改 Java 类中现有的属性，可以使用 IDE 来完成。Spring Roo 会负责在相应的 AspectJ ITD 文件中移除或修改该属性。

## @RooJavaBean—控制 getter 和 setter 方法的生成

我们看到，使用 @RooJavaBean 注解会为类中的所有字段引入 getter 和 setter 方法。在某些情况下，你可能希望控制这些 getter 和 setter 方法的生成。@RooJavaBean 允许你使用 gettersByDefault 和 settersByDefault 属性来实现这一点。这些属性指定是否应该默认生成 getter 和 setter 方法。这些属性的默认值为 true，这意味着 @RooJavaBean 注解会在相应的 *_Roo_JavaBean.aj ITD 为类中定义的所有字段创建 getter 和 setter 方法。

如果将 gettersByDefault 和 settersByDefault 元素的值都指定为 false，则 Spring Roo 会删除相应的 AspectJ ITD 文件。

### 另请参阅

- 创建 Java 接口示例展示了如何使用 Spring Roo 创建 Java 接口
- 第 2 章“使用 JPA 持久化对象”中的“向持久化实体添加字段”示例展示了 field 命令可用的其他参数

## 创建 Java 接口

你可以使用 Spring Roo 的 interface 命令或 IDE 来创建 Java 接口。在本示例中，我们将看到如何创建名为 FlightServiceIntf 的接口。

### 准备工作

从 C:\roo-cookbook\ch01-recipe 目录启动 Roo shell，该目录包含 flight-app Roo 项目。

### 操作步骤...

Spring Roo 提供了 interface 命令来创建 Java 接口，如下所示：

```
roo> interface --class sample.roo.flightapp.service.FlightServiceIntf
```

```
Created SRC_MAIN_JAVA\sample\roo\flightapp\service\FlightServiceIntf.java
```

```
sample.roo.flightapp.service.FlightServiceIntf roo>
```

### 工作原理...

以下表格描述了 interface 命令接受的参数：

| 参数 | 用途 |
| --- | --- |
| class | 这是必需参数，用于指定 Java 接口的完全限定名。 |
| path | 这是可选参数，用于标识创建接口的目录，默认为 SRC_MAIN_JAVA。 |
| permitReservedWords | 这是可选参数，指示 Spring Roo 允许在 Java 接口名称中使用保留字。 |

### 还有更多内容...

使用 Spring Roo，你可以在 Java 接口中添加常量或声明方法。要添加常量或方法，需要使用 IDE。你可能已经注意到，interface 命令中不提供 rooAnnotations 参数；因此，你可以安全地假设，当你对 Java 接口进行修改时，Spring Roo 不会生成任何相应的代码。

### 另请参阅

- 创建 Java 类示例展示了如何使用 Spring Roo 创建 Java 类

## 从 Roo shell 引用类型

在某些场景中，你可能希望将命令的焦点设置到特定的 Java 类型上。例如，你可能希望 Roo shell 在特定的 Java 类型上执行 field 命令，这样你就不需要在 field 命令中指定 class 参数了。

### 准备工作

从 C:\roo-cookbook\ch01-recipe 目录启动 Roo shell，该目录包含 flight-app Roo 项目。

### 操作步骤...

Spring Roo 提供了 focus 命令，允许你将命令的目标更改为不同的 Java 类型。以下步骤序列展示了如何使用 focus 命令在不同类型之间切换：

- 1. 执行以下 `focus` 命令以指定你要处理 `flight-app` 项目。这将更改 Roo 提示符以反映 Roo 项目的顶级包名：
  
  ```
  roo> focus --class ~
  ```
  
- 2. 使用 `class` 命令创建 `FlightDesc` 类。使用 ~ 表示你正在相对于项目顶级包名指定类的包名。这将更改 Roo 提示符以引用 `FlightDesc` 类型，如下所示：
  
  ```
  sample.roo.Flightapp roo> class --class ~.domain.FlightDesc
  --rooAnnotations
  
  Created SRC_MAIN_JAVA\sample\roo\flightapp\domain\FlightDesc.java
  ```
  
- 3. 使用 `class` 命令创建 `Flight` 类。这将更改 Roo 提示符，从引用 `FlightDesc` 类型改为引用新创建的 `Flight` 类型，如下所示：
  
  ```
  ~.domain.FlightDesc roo> class --class ~.domain.Flight
  --rooAnnotations
  
  Created SRC_MAIN_JAVA\sample\roo\flightapp\domain\Flight.java
  ```
  
- 4. 使用 `focus` 命令切换到 `FlightDesc` 类型。这将更改 Roo 提示符，从引用 `Flight` 类型改为引用 `FlightDesc` 类型，如下所示：
  
  ```
  ~.domain.Flight roo> focus --class ~.domain.FlightDesc
  ```
  
- 5. 如下所示，向 `FlightDesc` 类添加 `from_city` 和 `to_city` 属性。由于 Roo 提示符当前引用的类型是 `FlightDesc`，因此你不需要指定 `class` 参数：
  
  ```
  ~.domain.FlightDesc roo> field string from_city
  ~.domain.FlightDesc roo> field string to_city
  ```


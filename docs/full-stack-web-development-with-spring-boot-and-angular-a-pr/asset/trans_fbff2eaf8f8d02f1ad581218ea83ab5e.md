

Figure 5.7 – Project structure after the creation of packages

我们的项目结构现在应该与前面的截图中展示的一样。首先创建我们的实体；右键单击我们创建的实体包，然后单击 New | JPA | Entity。这将打开一个用于创建新实体的模型。我们将把我们的实体命名为 AntiHeroEntity 并单击 OK 按钮。该过程在以下截图中进行了说明：

Figure 5.8 – Creating an entity

我们将在实体包下看到一个新生成的类，其代码如下：

```
package com.example.springboot.superheroes.superheroes.antiHero.entity;

import javax.persistence.Entity;
import javax.persistence.Table;

@Entity
@Table(name = "anti_hero_entity")
public class AntiHeroEntity {
}
```

实体已自动生成，并带有 @Entity 和 @Table 注解，这些注解将用于将此模型标识为数据库中的对象。当前代码将存在一些问题，指出实体没有主键（PK）；在这种情况下，我们将向我们的模型添加以下内容：

- @Data：Lombok 注解，将设置 getter 和 setter 方法、toString() 方法以及 @RequiredArgsConstructor。
- @AllArgsConstructor：用于生成带有所有字段作为参数的构造函数的 Lombok 注解。
- @NoArgsConstructor：用于生成无参数构造函数的 Lombok 注解。
- @Id：在 javax.persistence.* 下找到，这将确定模型的 PK。
- @GeneratedValue：用于在 PK 上确定将使用哪种生成类型。
- @NotNull：在 javax.validation.constraints 下找到，并验证特定字段不能为空。

依赖项应通过向 pom.xml 添加以下代码来安装：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

在向 pom.xml 添加依赖项后，右键单击您的项目并选择 Maven | Reload project 以安装新的依赖项。

在成功向我们的模型添加 Lombok 注解、PK、验证和字段后，我们将拥有以下代码：

```
package com.example.springbootsuperheroes.superheroes.antiHero.entity;
...

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.UUID;
import javax.persistence.*;
import javax.validation.constraints.NotNull;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
...
```

在添加所有前述包之后，我们现在可以开始编写我们的类和注解，如下所示：

```
@Data
@Entity
@Table
@AllArgsConstructor
@NoArgsConstructor
public class AntiHeroEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO,
                    generator = "UUID")
    @Column(nullable = false, updatable = false)
    private UUID id;

    @NotNull(message = "First Name is required")
    private String firstName;

    private String lastName;
    private String house;
```

```
private String knownAs;
private String createdAt =
    new SimpleDateFormat("dd-MM-yyyy HH:mm:ss z")
        .format(new Date());
}
```

在前面的代码块中，我们可以看到我们添加了一个 UUID 类型的 PK；当我们向数据库插入新的反英雄数据时，它将被自动生成。该类型使用 @GeneratedValue 注解定义，其中我们还指明策略将使用自动生成器。我们还添加了多个字段，用于存储反英雄信息。

我们已成功创建了我们的实体；现在，我们将为反英雄实体创建一个 DTO。右键单击 dto 包，选择 New | Java class，我们将把新类命名为 AntiHeroDto。成功创建 DTO 后，我们将把以下代码放入其中：

```
package com.example.superheroes.antiHero.dto;

import java.util.UUID;
import javax.validation.constraints.NotNull;
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class AntiHeroDto {

    private UUID id;

    @NotNull(message = "First Name is required")
    private String firstName;

    private String lastName;
    private String house;
    private String knownAs;
}
```

在前面的代码示例中，我们添加了字段，这些字段是我们希望在向客户端发送反英雄数据时仅显示的；在这种情况下，我们在 DTO 中移除了 createdAt 字段。我们还添加了 @Getter 和 @Setter 注解以生成 DTO 的 getter 和 setter 方法，最后，我们还添加了 @NotNull 验证以使我们的约束与实体匹配。

我们已成功在应用程序中使用 Lombok 创建了我们的 DTO 和模型。在下一节中，我们将使我们的服务负责 Spring Boot 应用程序的 CRUD 功能。

## Writing services

在本节中，我们现在将为我们的应用程序编写服务，但首先，让我们讨论一下服务在 Spring Boot 中的主要目的。服务是放置所有业务逻辑的类；在这里，我们将借助 JPA 仓库编写我们的 CRUD 功能。在这种情况下，我们不仅会创建我们的服务类，还会创建我们的 JPA 仓库。

### JPA repository

如第 4 章《Setting Up the Database and Spring Data JPA》中所述，JPA 仓库是用于在 Java 应用程序中管理关系数据的广泛使用的规范。它通过不实现读写操作来帮助 Spring 开发减少样板代码。

在 Spring Boot 中创建 JPA 仓库非常简单；话虽如此，JPA 库提供了诸如 CrudRepository 等类，我们可以用来扩展我们的类。让我们在我们的 Spring Boot 应用程序示例中创建一个。按以下步骤操作：

- 1. 右键单击我们的 antiHero 包，选择 New，然后单击 Package 选项。
- 2. 创建一个名为 repository 的新包。
- 3. 创建新包后，右键单击 repository 包，选择 New，然后单击 Class 选项。
- 4. 当小模态弹出时，切换到 Interface 选项并将接口命名为 AntiHeroRepository。以下代码将被生成：

```
package com.example.springboot.superheroes.superheroes.antiHero.repository;

import com.example.springboot.superheroes.superheroes.antiHero.entity.AntiHeroEntity;
import org.springframework.data.repository.CrudRepository;

import java.util.UUID;

public interface AntiHeroRepository {
}
```

我们已创建了我们的接口，但这只是一个简单的接口。我们将通过扩展 CrudRepository 类来扩展我们的接口，将其转换为 JPA 仓库，如下所示：

```
public interface AntiHeroRepository extends
CrudRepository<AntiHeroEntity, UUID> {
}
```

AntiHeroRepository，如我们已经用 CrudRepository<Type, ID> 扩展的那样，将具有以下功能的方法：

- 持久化、更新和删除其中一个博客实体
- 通过其 PK 查找一个或多个博客
- 统计所有博客
- 验证单个博客是否存在

我们已成功创建了我们的 JPA 仓库；下一步是进行我们的服务。我们的目标是创建一个服务，该服务将具有获取整个列表、通过 PK 获取单个数据库、插入新数据、更新所选数据和删除数据的能力。我们可以通过 JPA 仓库提供的以下方法实现这一点：

- findAll()：获取特定实体中所有现有数据
- findById(Id)：通过 PK 查找特定数据库
- save()：在表中插入新数据
- save(data)：在表中更新现有数据
- deleteById(id)：通过 PK 删除表中的特定数据

现在我们已成功确定了服务所需的<|message|>

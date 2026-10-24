## 在 Spring Boot 中编写测试

在上一章中，你已经了解了日志记录器的重要性、相关概念，以及它们如何帮助开发人员调试和维护应用程序。你已经学习了 Log4j2，这是一个用于 Spring Boot 的第三方框架，提供了如 **Appenders**、**Filters** 和 **Markers** 等功能，可以帮助开发人员对日志事件进行分类和格式化。我们还讨论了 SLF4J，它是日志框架的抽象，允许我们在运行时或部署时在不同框架之间切换。最后，我们使用 XML 配置和 Lombok 实现并配置了日志框架。

本章现在将重点讨论为 Spring Boot 应用程序编写单元测试；我们将讨论 Java、JUnit 和 AssertJ 中最常用的测试框架，并将它们应用到我们的应用程序中。我们还将把 Mockito 集成到单元测试中，用于模拟对象和服务。

在本章中，我们将涵盖以下主题：

-   - 理解 JUnit 和 AssertJ
-   - 编写测试
-   - 使用 Mockito 在服务中编写测试

### 技术要求

本章完成版本的链接在这里：https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-09。

### 理解 JUnit 和 AssertJ

在应用程序的每次开发之后，测试永远是下一步，这是在我们交付或部署应用程序到生产环境之前最重要的任务之一。测试阶段对公司来说至关重要，因为这确保了产品的质量和有效性。





由于这是必不可少的过程之一，测试中几乎不允许出现错误，而手动测试是不够的，因为这容易出现人为错误，并且更有可能遗漏应用程序中已存在的问题。这就是单元测试发挥作用的地方——单元测试是一种自动化测试，允许开发人员为单个类或实体编写测试。

它是一种**回归测试**形式，会运行所有测试以验证在对应用程序代码应用多次更改或更新后，代码是否仍然通过测试用例。单元测试有助于维护我们应用程序的质量，因为它们带来了以下好处：

- **速度：** 与手动测试相比，单元测试所需的时间更少，因为它是可编程的，并且会在短时间内提供结果。
- **成本降低：** 单元测试是自动化的，这意味着测试应用程序所需的测试人员更少。
- **更少错误：** 单元测试会显著减少错误数量，因为测试不是由人工手动完成。
- **可编程：** 单元测试可以生成复杂的测试来检测应用程序中的隐藏信息。

由于其优势和测试，单元测试现在在前端和后端开发中都广泛使用，尤其是在 Java 中。Java 中已经有多个测试框架可用，但我们将讨论第一个也是最常用的框架，**JUnit**。

## JUnit 框架

JUnit 是一种回归测试框架，主要用于为 Java 应用程序中的单个类编写测试和断言；它提倡*先测试后编码*的理念，即在实现之前，我们需要为要测试的代码片段创建测试数据。JUnit 也是一个开源框架，这使其更可靠。

有一个庞大的社区支持该框架，它使用断言来测试预期结果并使用注解来标识用于测试的方法，并且可以高效地利用并与 Maven 和 Gradle 项目集成。

让我们讨论我们将用于编写测试的 JUnit 功能：

- **Fixtures：** 这些是我们可视为常量或运行测试的基线的对象。Fixtures 的主要用途是确保在整个测试过程中具有相同值的变量会被使用。有两种类型的 fixtures，分别是：
  - `setUp()`：此方法在每次测试被调用*之前*执行。
  - tearDown()：此方法在每次测试被调用*之后*执行：

```
public class JavaTest extends TestCase {
    protected int value1, value2;
    // will run before testSubtract and testMultiply
    protected void setUp(){
        value1 = 23;
        value2 = 10;
    }
    public void testSubtract(){
        double result = value1 - value2;
        assertTrue(result == 13);
    }
    public void testMultiply(){
        double result = value1 * value2;
        assertTrue(result == 230);
    }
}
```

在前面的代码示例中，我们可以看到定义了两个测试方法，分别是 testSubtract() 和 testMultiply()，在每个方法被调用之前。setUp() fixture 将首先被调用以分配 value1 和 value2 变量的值。

- **Test suites：** 将多个单元测试用例分组并一起执行，使用 @RunWith 和 @Suite 注解来运行测试。让我们看一下以下示例：

```
//JUnit Suite Test
@RunWith(Suite.class)
@Suite.SuiteClasses({
    TestOne.class, TestTwo.class
});
public class JunitTestSuite {
}
public class TestOne {
    int x = 1;
    int y = 2;
    @Test
    public void TestOne() {
        assertEquals(x + y, 3);
    }
}
```

```
}
public class TestTwo {
    int x = 1;
    int y = 2;
    @Test
    public void TestTwo() {
        assertEquals(y - x, 1);
    }
}
```

在前面的代码示例中，我们可以看到我们有两个定义的类，每个类都有一个带有 @Test 注解的方法；测试方法将被一起执行，因为我们已使用 @Suite.SuiteClasses 方法将它们捆绑在一起。

- **Test runners：** 主要用于运行测试用例，我们使用 runClasses() 方法在特定类中运行测试用例。让我们在这里看一个基本示例：

```
public class JUnitTestRunner {
    public static void main(String[] args) {
        Result result =
            JUnitCore.runClasses(TestJunit.class);

        for (Failure failure : result.getFailures()) {
            System.out.println(failure.toString());
        }

        System.out.println(result.wasSuccessful());
    }
}
```

- **Classes：** JUnit 类主要用于为我们的应用程序编写测试；这些包括以下内容：
  - **Assert：** 包含一组断言方法
  - **Test case：** 包含用于运行多个测试的 fixtures 的测试用例
  - **Test result：** 包含从已执行的测试用例收集所有结果的方法

## JUnit 中的断言

**Assertions** 是通过检查所写代码的结果来验证我们的测试是否有效的方式。在 JUnit 中，所有断言都在 `Assert` 类下，一些来自 Assert 的基本方法如下：

- `void assertTrue(boolean condition)`：验证条件是否为真
- `void assertFalse(boolean condition)`：验证条件是否为假
- `void assertNotNull(Object obj)`：检查对象是否不为 null
- `void assertNull(Object obj)`：检查对象是否为 null
- `void assertEquals(Object obj1, Object obj2)`：检查两个对象或原始类型是否相等
- `void assertArrayEquals(Array array1, Array array2)`：验证两个数组是否彼此相等

## 注解

**Annotations** 是我们添加到方法和类的元标签；这为 JUnit 提供额外信息，说明哪些方法应在测试方法之前和之后运行，以及哪些将在测试执行期间被忽略。

以下是我们可以在 JUnit 中使用的注解：

- `@Test`：此注解用于 `public void` 方法，以表示该方法是可执行的测试用例。
- `@Ignore`：此注解用于忽略不执行的测试用例。
- `@Before`：此注解用于 `public void` 方法，在每个测试用例方法之前运行该方法。这通常用于如果我们想声明所有测试用例使用的相似对象。
- `@After`：此注解用于 `public void` 方法，在每个测试用例方法之后运行该方法；这通常用于如果我们想在运行新测试用例之前释放或清理多个资源。
- `@BeforeClass`：此注解允许 `public static void` 方法在所有测试用例执行之前运行一次。
- `@AfterClass`：此注解允许 `public static void` 方法在所有测试用例执行后运行一次。

让我们看一个带有注解及其执行顺序的示例测试：

```
public class JunitAnnotationSequence {

    //execute once before all test
    @BeforeClass
    public static void beforeClass() {
        System.out.println("beforeClass()");
    }
    //execute once after all test
    @AfterClass
    public static void afterClass() {
        System.out.println("afterClass()");
    }

    //execute before each test
    @Before
    public void before() {
        System.out.println("before()");
    }
    //execute after each test
    @After
    public void after() {
        System.out.println("after()");
    }
    @Test
    public void testMethod1() {
        System.out.println("testMethod1()");
    }
    @Test
    public void testMethod2() {
        System.out.println("testMethod2()");
    }
}
```

在前面的代码示例中，我们有一个 `JunitAnnotationSequence` 类，其中包含多个带注解的方法。当我们执行测试时，我们将得到以下输出：

```
beforeClass()
before()
testMethod1()
after()
before()
testMethod2()
after()
afterClass()
```

我们可以看到在前面的示例中，带有 `@BeforeClass` 和 `@AfterClass` 注解的方法只被调用一次，并且它们在测试执行开始和结束时被调用。另一方面，带有 `@Before` 和 `@After` 注解的方法在每个测试方法开始和结束时被调用。

我们已经了解了单元测试中 JUnit 的基础知识；现在，让我们讨论 AssertJ 的概念。

## 使用 AssertJ

我们在上一部分刚刚探索了 JUnit 的概念和功能，并且我们了解到仅使用 JUnit，我们可以使用 Assert 类应用断言，但我们可以通过使用 AssertJ 使我们的断言更流畅和灵活。AssertJ 是一个主要用于编写断言的库；其主要目标是提高测试代码的可读性并使测试的维护更简单。

让我们比较一下在 JUnit 和 AssertJ 中编写断言的方法：

- JUnit 检查条件是否返回 `true`：
  
  ```java
  Assert.assertTrue(condition)
  ```
  
- AssertJ 检查条件是否返回 `true`：
  
  ```java
  Assertions.assertThat(condition).isTrue()
  ```

我们可以看到在前面的示例中，在 AssertJ 中，我们将始终将要比较的对象传递给 `assertThat()` 方法，然后我们将调用下一个方法，即实际的断言。让我们看一下在 AssertJ 中我们可以使用不同种类的断言。





## 布尔断言

布尔断言用于检查条件是否返回 true 或 false。断言方法如下：

- isTrue(): 检查条件是否为真：
  ```
  Assertions.assertThat(4 > 3).isTrue()
  ```

- isFalse(): 检查条件是否为假：
  ```
  Assertions.assertThat(11 > 100).isFalse()
  ```

## 字符断言

字符断言用于将对象与字符进行比较或检查字符是否在 Unicode 表中；断言方法如下：

- isLowerCase(): 检查给定字符是否为小写：
  ```
  Assertions.assertThat('a').isLowerCase();
  ```

- isUpperCase(): 检查字符是否为大写：
  ```
  Assertions.assertThat('a').isUpperCase();
  ```

- isEqualTo(): 检查两个给定字符是否相等：
  ```
  Assertions.assertThat('a').isEqualTo('a');
  ```

- isNotEqualTo(): 检查两个给定字符是否不相等：
  ```
  Assertions.assertThat('a').isNotEqualTo('b');
  ```

- inUnicode(): 检查字符是否包含在 Unicode 表中：
  ```
  Assertions.assertThat('a').inUnicode();
  ```

这些只是 AbstractCharacterAssert 下可用的一些断言。有关完整文档，您可以前往 https://joel-costigliola.github.io/assertj/core-8/api/org/assertj/core/api/AbstractCharacterAssert.html。

## 类断言

**类断言**用于检查特定类中的字段、类型、访问修饰符和注解。以下是一些类断言方法：

- isNotInterface(): 验证该类不是接口：
  ```
  interface Hero {}
  class Thor implements Hero {}
  Assertions.assertThat(Thor.class).isNotInterface()
  ```

- isInterface(): 验证该类是接口：
  ```
  interface Hero {}
  class Thor implements Hero {}
  Assertions.assertThat(Hero.class).isInterface()
  ```

- isPublic(): 验证该类是公共的：
  ```
  public class Hero {}
  protected class AntiHero {}
  Assertions.assertThat(Hero.class).isPublic()
  ```

- isNotPublic(): 验证该类不是公共的：
  ```
  public class Hero {}
  protected class AntiHero {}
  Assertions.assertThat(AntiHero.class).isNotPublic()
  ```

这些只是 AbstractClassAssert 下可用的一些断言。有关完整文档，您可以前往 https://joel-costigliola.github.io/assertj/core-8/api/org/assertj/core/api/AbstractClassAssert.html。

## 可迭代断言

**可迭代断言**用于根据其长度和内容验证可迭代对象或数组对象。以下是一些可迭代断言方法：

- contains(): 证明可迭代对象具有给定值：
  ```
  List test = List.asList("Thor", "Hulk", "Dr. Strange");
  assertThat(test).contains("Thor");
  ```

- isEmpty(): 验证给定可迭代对象的长度是否等于 0：
  ```
  List test = new List();
  assertThat(test).isEmpty();
  ```

- isNotEmpty(): 验证给定可迭代对象的长度是否大于 0：
  ```
  List test = List.asList("Thor", "Hulk",
      "Dr. Strange");
  assertThat(test).isNotEmpty();
  ```

- hasSize(): 验证可迭代对象的长度是否等于给定值：
  ```
  List test = List.asList("Thor", "Hulk",
      "Dr. Strange");
  assertThat(test).hasSize(3);
  ```

这些只是 AbstractIterableAssert 下可用的一些断言。有关完整文档，您可以前往此处提供的链接：https://joel-costigliola.github.io/assertj/core-8/api/org/assertj/core/api/AbstractIterableAssert.html。

## 文件断言

文件断言用于验证文件是否存在、可写或可读，并验证其内容。以下是一些文件断言方法：

- exists(): 证明文件或目录存在：
  ```
  File file = File.createTempFile("test", "txt");
  assertThat(file).exists();
  ```

- isFile(): 验证给定对象是否为文件（提供目录将导致测试失败）：
  ```
  File file = File.createTempFile("test", "txt");
  assertThat(file).isFile();
  ```

- canRead(): 验证给定文件是否可由应用程序读取：
  ```
  File file = File.createTempFile("test", "txt");
  assertThat(file).canRead();
  ```

- canWrite(): 验证给定文件是否可由应用程序修改：
  ```
  File file = File.createTempFile("test", "txt");
  assertThat(file).canWrite();
  ```

这些只是 AbstractFileAssert 下可用的一些断言。有关完整文档，您可以前往此处提供的链接：https://joel-costigliola.github.io/assertj/core-8/api/org/assertj/core/api/AbstractFileAssert.html。

## Map 断言

**Map 断言**用于根据其条目、键和大小检查 map。以下是一些 map 断言方法：

- contains(): 验证 map 是否包含给定的条目：
  ```
  Map<name, Hero> heroes = new HashMap<>();
  heroes.put(stark, iron_man);
  heroes.put(rogers, captain_america);
  heroes.put(parker, spider_man);
  assertThat(heroes).contains(entry(stark, iron_man), entry(rogers, captain_america));
  ```

- containsAnyOf(): 验证 map 是否包含至少一个条目：
  ```
  Map<name, Hero> heroes = new HashMap<>();
  heroes.put(stark, iron_man);
  heroes.put(rogers, captain_america);
  heroes.put(parker, spider_man);
  assertThat(heroes).contains(entry(stark, iron_man), entry(odinson, thor));
  ```

- hasSize(): 验证 map 的大小是否等于给定值：
  ```
  Map<name, Hero> heroes = new HashMap<>();
  heroes.put(stark, iron_man);
  heroes.put(rogers, captain_america);
  heroes.put(parker, spider_man);
  assertThat(heroes).hasSize(3);
  ```

- isEmpty(): 验证给定的 map 是否为空：
  ```
  Map<name, Hero> heroes = new HashMap<>();
  assertThat(heroes).isEmpty();
  ```

- isNotEmpty(): 验证给定的 map 是否不为空：
  ```
  Map<name, Hero> heroes = new HashMap<>();
  heroes.put(stark, iron_man);
  heroes.put(rogers, captain_america);
  heroes.put(parker, spider_man);
  assertThat(heroes).isNotEmpty();
  ```

这些只是 AbstractMapAssert 下可用的一些断言。有关完整文档，您可以前往此处提供的链接：https://joel-costigliola.github.io/assertj/core-8/api/org/assertj/core/api/AbstractMapAssert.html。

我们已经学习了使用 AssertJ 的不同断言方法；现在，我们将在 Spring Boot 应用程序中实现并编写我们的单元测试。

## 编写测试

在本节中，我们现在将在 Spring Boot 应用程序中开始编写我们的单元测试。当我们回到我们的应用程序时，服务和存储库是我们应用程序中需要实现单元测试的重要部分，因为服务包含业务逻辑，并且在添加新功能时经常会被修改。存储库包括用于 CRUD 和其他操作的方法。

我们将在编写单元测试时实现两种方法。第一种方法是使用内存数据库如 H2 在运行单元测试时存储我们创建的数据。第二种方法是使用 Mockito 框架模拟我们的对象和存储库。

## 使用 H2 数据库进行测试

我们将要实现的第一个编写测试的方法是使用 JUnit 和 AssertJ 与 H2 数据库。H2 数据库是一个内存数据库，允许我们在系统内存中存储数据。一旦应用程序关闭，它将删除所有存储的数据。H2 通常用于概念验证或单元测试。

我们在第 4 章《设置数据库和 Spring Data JPA》中已经添加了 H2 数据库，但如果您错过了这一部分，为了添加 H2 依赖，我们将在 pom.xml 文件中添加以下内容：

```
<dependency>
<groupId>com.h2database</groupId> <artifactId>h2</artifactId>
```


<scope>runtime</scope>
</dependency>

成功添加依赖后，我们将在 test/java 文件夹下添加 h2 配置。我们将添加一个新的资源包并创建一个新的应用程序来实现此目的。单元测试将使用属性文件，我们将放置以下配置：

```
spring.datasource.url=jdbc:h2://mem:testdb;DB_CLOSE_DELAY=-1
spring.datasource.username={username}
spring.datasource.password={password}
spring.datasource.driver-class-name=org.h2.Driver
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.properties.hibernate.format_sql=true
```

在前面的示例配置中，首先，我们已指定希望使用 spring.datasource.url 属性将数据存储在 test.mv.db 文件中。我们还可以使用 spring.datasource.username 和 spring.datasource.password 属性覆盖 H2 控制台的用户名和密码，并且我们还指定表将在应用程序启动时创建，并在应用程序停止时删除。

## Testing a service

现在，我们将在 test/java 文件夹下创建一个包。这里我们将编写测试。我们将从主文件夹创建一个类似的包。在这种情况下，我们将创建 com.example.springboot superheroes.superheroes.antiHero.h2.service。在新创建的包下，我们将创建一个名为 AntiHeroH2ServiceTest 的新类，在这里我们将开始为 AntiHeroService 编写测试。

我们需要采取的第一步是使用 @DataJpaTest 注解对我们的类进行注解。该注解允许服务仅关注 JPA 组件，通过禁用完整自动配置并仅应用与测试相关的配置。下一步是添加 AntiHeroService 的依赖项，即 AntiHeroRepository。我们将声明一个新的 AntiHeroRepository 并使用 @Autowired 注解注入依赖项，我们还将声明 AntiHeroService，因为这是我们需要测试的服务。我们将有以下代码：

```
@DataJpaTest
public class AntiHeroH2ServiceTest {
    @Autowired
private AntiHeroRepository repo;
private AntiHeroService service;
}
```

在注入依赖项并对类进行注解之后，接下来我们需要考虑在运行每个测试之前希望拥有的可能属性是什么；在这种情况下，我们希望在运行测试用例之前创建一个 AntiHeroService 实例。为此，我们将创建一个用 @BeforeEach 注解的方法，并使用 AutoWired AntiHeroRepository 作为参数创建 AntiHeroService 的新实例：

```
@BeforeEach
public void setup() {
    service = new AntiHeroService(repo);
}
```

现在，我们可以为我们的服务编写一个测试用例；我们的目标是为 AntiHeroService 拥有的每个方法编写一个测试。

让我们列出 AntiHeroService 的方法：

- Iterable<AntiHeroEntity> findAllAntiHeroes: 应返回反英雄列表。
- AntiHeroEntity addAntiHero(AntiHeroEntity antiHero): 应添加新的反英雄实体。
- void updateAntiHero(UUID id, AntiHeroEntity antiHero): 应根据给定的 ID 更新反英雄。
- AntiHeroEntity findAntiHeroById(UUID id): 应返回具有给定 ID 的反英雄；如果未找到，它将返回 NotFoundException。
- void removeAntiHeroById(UUID id): 应根据给定的 ID 从数据库中删除反英雄。

让我们首先为 findAllAntiHeroes() 方法编写一个测试。该方法的可能测试用例是检查该方法是否成功从数据库中检索所有反英雄。为了测试此场景，我们希望首先向我们的 H2 数据库添加一个实体或一组测试反英雄实体。我们可以调用 findAllAntiHeroes() 方法来检索数据库中新添加的实体。让我们看一下这里的示例单元测试：

```
@Test
public void shouldFindAllAntiHero() {
AntiHeroEntity antiHero = new AntiHeroEntity();
antiHero.setFirstName("Eddie");
antiHero.setLastName("Brock");
antiHero.setHouse("MCU");

service.addAntiHero(antiHero);

Iterable<AntiHeroEntity> antiHeroList =
    service.findAllAntiHeroes();
AntiHeroEntity savedAntiHero =
    antiHeroList.iterator().next();

assertThat(savedAntiHero).isNotNull();
}
```

在前面的代码示例中，我们可以看到我们首先创建了一个新的反英雄实例作为数据库内存中的示例数据。我们使用 addAntiHero() 方法将数据添加到数据库中。成功插入数据后，我们可以检查或断言是否可以使用 findAllAntiHeroes() 方法检索新创建的反英雄。在此场景中，我们检索了反英雄列表中的第一个数据。我们使用 assertThat(savedAntiHero).isNotNull() 来验证列表的第一个元素不为 null。

现在，让我们为 addAntiHero() 方法编写一个测试。我们将为以下方法创建的测试大多与我们为 findAllAntiHeroes() 方法创建的测试相似。以下方法的可能测试用例是检查实体是否成功添加到我们的数据库中。

让我们看一下以下示例单元测试：

```
@Test
public void shouldAddAntiHero() {
    AntiHeroEntity antiHero = new AntiHeroEntity();
    antiHero.setFirstName("Eddie");
    antiHero.setLastName("Brock");
    antiHero.setHouse("MCU");
    service.addAntiHero(antiHero);

    Iterable<AntiHeroEntity> antiHeroList =
        service.findAllAntiHeroes();
```
```java
AntiHeroEntity savedAntiHero =
    antiHeroList.iterator().next();

assertThat(antiHero).isEqualTo(savedAntiHero);
}
```
```

我们在前面的代码示例中创建了一个新的反英雄实体并使用 addAntiHero() 方法将其插入到数据库中。添加最新数据后，我们可以检索列表并验证我们的数据是否在数据库中。在给定的场景中，我们检索了反英雄列表中的第一条数据，我们使用 assertThat(antiHero).isEqualTo(savedAntiHero); 来检查我们检索到的数据是否等于我们实例化的数据。

接下来，让我们现在编写 updateAntiHeroMethod(); 的测试。以下方法的可能测试用例是检查该方法是否成功修改我们数据库中特定实体的信息。

让我们看一下满足此处测试用例的示例单元测试：

```
```java
@Test
public void shouldUpdateAntiHero() {
    AntiHeroEntity antiHero = new AntiHeroEntity();
    antiHero.setFirstName("Eddie");
    antiHero.setLastName("Brock");
    antiHero.setHouse("MCU");

    AntiHeroEntity savedAntiHero =
        service.addAntiHero(antiHero);

    savedAntiHero.setHouse("San Francisco");
    service.updateAntiHero(savedAntiHero.getId(),
                           savedAntiHero);
    AntiHeroEntity foundAntiHero =
        service.findAntiHeroById(savedAntiHero.getId());

    assertThat(foundAntiHero.getHouse()).isEqualTo(
        "San Francisco");
}
```
```

我们在前面的代码示例中创建了一个新的反英雄实体并使用 addAntiHero() 方法将其插入到数据库中。添加实体后，我们将添加的反英雄的房屋信息更新为“San Francisco”并使用 updateAntiHeroMethod() 将其保存到数据库中。最后，我们使用其 ID 检索了修改后的反英雄并通过添加 assertThat (foundAntiHero.getHouse()).isEqualTo("San Francisco"); 断言验证了房屋信息已修改。

接下来，我们现在将为 removeAntiHeroById() 方法创建一个单元测试。该方法的可能测试用例是验证具有相应 ID 的实体是否已成功从数据库中删除。

让我们看一下满足测试用例的示例单元测试：

```
@Test
public void shouldDeleteAntiHero() {
    assertThrows(NotFoundException.class, new Executable() {
        @Override
        public void execute() throws Throwable {
            AntiHeroEntity savedAntiHero =
                service.addAntiHero(antiHero);

            service.removeAntiHeroById(
                savedAntiHero.getId());
            AntiHeroEntity foundAntiHero =
                service.findAntiHeroById(
                    savedAntiHero.getId());

            assertThat(foundAntiHero).isNull();
        }
    });
}
```

在前面的示例中，我们可以看到我们在编写单元测试时添加了一些额外元素；我们创建了一个新的 Executable() 实例，我们在其中放置了主要代码。我们使用 NotFoundException.class 断言了我们的 Executable()。这样做的主要原因是我们期望 findAntiHeroByID() 将返回 NotFoundException 错误，因为我们已从数据库中删除了实体。

请记住，在断言错误时，我们应该使用 assertThrows()。

我们已成功为我们的服务编写了测试，现在，我们将在存储库级别实现单元测试。





## 测试仓库

为应用程序的仓库编写测试与我们在服务层编写测试的方式大致相同；我们也将其视为服务，并在仓库中添加额外方法时对其进行测试。

我们将以为 UserRepository 编写单元测试为例。让我们回顾一下 UserRepository 拥有的 方法：

- Boolean selectExistsEmail(String email)：当用户使用给定邮箱存在时返回 true
- UserEntity findByEmail(String email)：当给定邮箱存在于数据库中时返回用户

开始编写测试时，首先，我们将在 com.example.springboot.superheroes 包下创建一个名为 user.repository 的新包，并创建一个名为 UserRepositoryTest 的新类。成功创建仓库后，我们将使用 @DataJpaTest 注解该类，以便它仅关注 JPA 组件，并使用 @Autowired 注解注入 UserRepository。

我们的类现在将如下所示：

```
@DataJpaTest
class UserRepositoryTest {

    @Autowired
    private UserRepository underTest;

}
```

现在，我们可以在成功注入仓库后编写测试。首先，我们想为 selectExistsEmail() 方法编写一个测试。该方法的可能测试用例是：如果邮箱存在于我们的数据库中，则应返回 true。

让我们看一下以下示例代码：

```
@Test
void itShouldCheckWhenUserEmailExists() {
    // give
    String email = "seiji@gmail.com";
    UserEntity user = new UserEntity(email, "21398732478");
underTest.save(user);

// when
boolean expected = underTest.selectExistsEmail(email);

// then
assertThat(expected).isTrue();
}
```

我们在示例单元测试中向数据库添加了一个示例用户实体。selectExistsEmail() 方法预计返回 true。这应该检索使用给定邮箱添加的用户。

下一个测试是针对 findByEmail() 方法的；这与我们为 selectExistsEmail() 方法创建的测试几乎相似。我们唯一需要修改的是断言，因为我们期望返回 User 类型的返回值。

让我们看一下以下示例代码：

```
@Test
void itShouldFindUserWhenEmailExists() {
    // give
    String email = "dennis@gmail.com";
    UserEntity user = new UserEntity(email, "21398732478");

    underTest.save(user);

    // when
    UserEntity expected = underTest.findByEmail(email);

    // then
    assertThat(expected).isEqualTo(user);
}
```

我们已使用 JUnit、AssertJ 和 H2 数据库成功为我们的服务和仓库编写了测试。在下一节中，我们将使用第二种实现，使用 JUnit 和 AssertJ 与 Mockito 编写单元测试。

## 使用 Mockito 在服务中编写测试

在上一节中，我们使用 H2 数据库创建了单元测试；在这种方法中，我们将完全省略数据库的使用，并利用模拟的概念在单元测试中创建示例数据。我们将通过使用 Mockito 实现这一点。Mockito 是 Java 中的一个模拟框架，允许我们在隔离状态下测试类；它不需要任何数据库。

它使我们能够从模拟对象或服务返回虚拟数据。Mockito 非常有用，因为这使得单元测试变得不那么复杂，尤其是在大型应用程序中，因为我们不想同时测试服务和依赖项。以下是使用 Mockito 的其他好处：

- **支持返回值**：支持模拟返回值。
- **支持异常**：可以在单元测试中处理异常。
- **支持注解**：可以使用注解创建模拟。
- **安全重构**：重命名方法名称或更改参数顺序不会影响测试，因为模拟是在运行时创建的。

让我们探索 Mockito 的不同功能以编写单元测试。

## 添加行为

Mockito 包含 when() 方法，我们可以在其中模拟对象返回值。这是 Mockito 最有价值的功能之一，因为我们可以定义服务或仓库的虚拟返回值。

让我们看一下以下代码示例：

```
public class HeroTester {
    // injects the created Mock
    @InjectMocks
    HeroApp heroApp = new HeroApp();
    // Creates the mock
    @Mock
    HeroService heroService;
    @Test
    public void getHeroHouseTest(){
        when(heroService.getHouse()).thenReturn(
            "San Francisco ");
        assertThat(heroApp.getHouse()).isEqualTo(
            "San Francisco");
    }
}
```

在前面的代码示例中，我们可以看到我们在测试中模拟了 HeroService。我们这样做是为了隔离类，而不测试 HeroService 本身的功能；我们想要测试的只是 HeroApp 的功能。我们已通过指定模拟返回 thenReturn() 方法为 heroService.getHouse() 方法添加了行为。在这种情况下，我们期望 getHouse() 方法将返回值为 "San Francisco"。

## 验证行为

我们可以在单元测试中使用的下一个 Mockito 功能是行为验证。这允许我们验证模拟方法是否被调用并使用参数执行。这可以通过 verify() 方法实现。

让我们使用相同的类示例：

```
public class HeroTester {
    // injects the created Mock
    @InjectMocks
    HeroApp heroApp = new HeroApp();
    // Creates the mock
    @Mock
    HeroService heroService;
    @Test
    public void getHeroHouseTest() {
        when(heroService.getHouse()).thenReturn(
            "San Francisco ");
        assertThat(heroApp.getHouse()).isEqualTo(
            "San Francisco");
        verify(heroService).getHouse();
    }
}
```

在前面的代码示例中，我们可以看到我们已将 verify(heroService).getHouse() 添加到我们的代码中。这验证了我们是否已调用 getHouse() 方法。我们还可以验证方法是否使用某些给定参数被调用。

## 期望调用

期望调用是行为验证的扩展功能；我们还可以检查模拟方法被调用的次数。我们可以通过使用 times(n) 方法做到这一点。同时，我们也可以使用 never() 方法验证它是否已被调用。

让我们看一下以下示例代码：

```
public class HeroTester {
    // injects the created Mock
    @InjectMocks
    HeroApp heroApp = new HeroApp();
    // Creates the mock
    @Mock
    HeroService heroService;
    @Test
    public void getHeroHouseTest() {
        // gets the values of the house
        when(heroService.getHouse()).thenReturn(
            "San Francisco ");
        // gets the value of the name
        when(heroService.getName()).thenReturn("Stark");
        // called one time
        assertThat(heroApp.getHouse()).isEqualTo(
            "San Francisco");
        // called two times
        assertThat(heroApp.getName()).isEqualTo("Stark");
        assertThat(heroApp.getName()).isEqualTo("Stark");

        verify(heroService, never()).getPowers();
        verify(heroService, times(2)).getName();
    }
}
```

在前面的代码示例中，我们可以看到我们使用了 times(2) 方法来验证 heroService 的 getName() 方法是否被调用了两次。我们还使用了 never() 方法，它检查 getPowers() 方法是否未被调用。

Mockito 除 times() 和 never() 之外，还提供了额外的验证预期调用次数的方法，这些方法如下：

- atLeast (int min)：验证方法是否至少被调用 n 次
- atLeastOnce ()：验证方法是否至少被调用一次
- atMost (int max)：验证方法是否最多被调用 n 次

## 异常处理

Mockito 还在单元测试中提供异常处理；它允许我们在模拟上抛出异常以测试应用程序中的错误。

让我们看一下以下示例代码：

```
public class HeroTester {
    // injects the created Mock
    @InjectMocks
    HeroApp heroApp = new HeroApp();
    // Creates the mock
    @Mock
    HeroService heroService;
    @Test
    public void getHeroHouseTest() {
        doThrow(new RuntimeException("Add operation not implemented")).when(heroService.getHouse())
        .thenReturn("San Francisco ")
        assertThat(heroApp.getHouse()).isEqualTo(
            "San Francisco");
    }
}
```

在前面的示例中，我们配置了 heroService.getHouse()，一旦被调用，就会抛出 RunTimeException。这将允许我们测试并覆盖应用程序中的错误块。

我们已了解 Mockito 中可用的不同功能。现在，让我们继续在 Spring Boot 应用程序中编写测试。

## Spring Boot 中的 Mockito

在本节中，我们现在将实现 Mockito 用于在 Spring Boot 应用程序中编写单元测试。我们将再次为服务编写测试，并将在我们的 test/java 文件夹下创建另一个包，将用于使用 Mockito 的单元测试；我们将创建 com.example.springboot.superheroes.antiHero.service。在新创建的包下，我们将创建一个名为 AntiHeroServiceTest 的新类，在其中开始为 AntiHeroService 编写测试。





成功创建类后，我们需要使用 @ExtendWith(MockitoExtension.class) 为类添加注解，以便能够使用 Mockito 的方法和功能。下一步是模拟我们的 AntiHeroRepository 并将其注入到我们的 AntiHeroRepositoryService 中。为此，我们将使用 @Mock 注解声明的存储库，并使用 @InjectMocks 注解声明的服务，我们的类现在将如下所示：

```
@ExtendWith(MockitoExtension.class)
class AntiHeroServiceTest {

    @Mock
    private AntiHeroRepository antiHeroRepository;

    @InjectMocks
    private AntiHeroService underTest;
}
```

在前面的示例中，我们成功模拟了存储库并将其注入到服务中。现在我们可以在测试中开始模拟存储库的返回值和行为。

让我们在 AntiHeroService 中有一些示例测试；在示例场景中，我们将为 addAntiHero() 方法编写一个测试。此方法的可能测试用例是验证存储库的 save() 方法是否被调用以及反英雄是否成功添加。

让我们看一下这里的示例代码：

```
@Test
void canAddAntiHero() {
    // given
    AntiHeroEntity antiHero = new AntiHeroEntity(
        UUID.randomUUID(),
        "Venom",
        "Lakandula",
        "Tondo",
        "Datu of Tondo",
        new SimpleDateFormat(
            "dd-MM-yyyy HH:mm:ss z").format(new Date())
    );
```

```
// when
underTest.addAntiHero(antiHero);

// then
ArgumentCaptor<AntiHeroEntity>
antiHeroDtoArgumentCaptor =
    ArgumentCaptor.forClass(
        AntiHeroEntity.class
    );
verify(antiHeroRepository).save(
    antiHeroDtoArgumentCaptor.capture());
AntiHeroEntity capturedAntiHero =
    antiHeroDtoArgumentCaptor.getValue();

assertThat(capturedAntiHero).isEqualTo(antiHero);
}
```

在前面的示例中，第一步始终是创建一个示例实体，我们可以用它作为添加新反英雄的参数；在调用我们正在测试的 addAntiHero() 方法之后，我们使用 verify() 方法验证 AntiHeroRepository 的 save() 方法是否已被调用。

我们还使用了 ArgumentCaptor 来捕获我们以前使用的方式中使用的参数值，这将用于进一步的断言。在这种情况下，我们断言捕获的反英雄等于我们创建的反英雄实例。

## Summary

至此，我们已到达本章的结尾。让我们回顾一下您学到的宝贵内容；您已了解 JUnit 的概念，JUnit 是一个测试框架，提供诸如 fixtures、测试套件和类等功能来测试我们应用程序中的方法。您还了解了 AssertJ 与 JUnit 的应用，它提供了一种更灵活的方式来断言我们单元测试中的对象；最后，您还了解了 Mockito 的重要性，它使我们能够模拟对象和服务。

在下一章中，我们将使用 Angular 开发前端应用程序。我们将讨论如何组织我们的功能和模块、在 Angular 文件结构中构建我们的组件，以及向用户界面添加 Angular Material。

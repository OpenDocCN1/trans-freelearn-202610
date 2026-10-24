

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

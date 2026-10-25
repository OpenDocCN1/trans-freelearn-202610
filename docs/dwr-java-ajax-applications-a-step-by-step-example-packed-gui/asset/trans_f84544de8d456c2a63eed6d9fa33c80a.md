

```java
public String[] getCountryData(String ccode) {
    List<String> countryData = new Vector<String>();
    Connection con = getConnection();
    try {
        PreparedStatement pstmt = con
            .prepareStatement("select short_name,long_name,code,capital,notes from country_data where code=?");
        pstmt.setString(1, ccode);
        ResultSet rs = pstmt.executeQuery();
        if (rs.next()) {
            for (int i = 1; i <= 5; i++) {
                if (i == 5) {
                    String notes = rs.getString(i);
                    if (notes.length() == 0) {
                        notes = "&nbsp;";
                    }
                    countryData.add(notes);
                } else {
                    countryData.add(rs.getString(i));
                }
            }
        }
    } catch (SQLException e) {
        e.printStackTrace();
    }
    closeConnection(con);
    String[] cdata = new String[5];
    return countryData.toArray(cdata);
}

public String[] saveCountryNotes(String ccode, String notes) {
    Connection con = getConnection();
    try {
        PreparedStatement pstmt = con
            .prepareStatement("update country_data set notes=? where code=?");
        pstmt.setString(1, notes);
        pstmt.setString(2, ccode);
        pstmt.executeUpdate();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    closeConnection(con);
    String[] rv = new String[2];
    rv[0] = ccode;
    rv[1] = notes;
    return rv;
}
}
```

该类具有与使用 CSV 文件作为数据源的类相同的 方法。请注意，最好为类似的类定义一个接口，然后使用例如工厂类在运行时选择正确的实现。

`getConnection()` 方法使用指定的数据源名称从 Geronimo 运行时获取连接，而 `closeConnection()` 方法关闭连接（或将其返回到 Geronimo 池）。

该类中检索或更新数据库中国家的其他方法都是使用简单直接的 JDBC 调用和 SQL 脚本的。

为了在我们的应用程序中使用此类，我们需要将其配置为访问 Geronimo 数据库池。为此，我们在 web.xml 和 geronimo-web.xml 中添加数据源条目。在 geronimo-web.xml 文件中，我们将之前创建的数据库池映射到我们在 web.xml 文件中指定的数据源。

以下条目在 geronimo-web.xml 文件中将我们的数据库池配置为应用程序的依赖项：

```xml
<sys:dependencies>
  ...
  <sys:dependency>
    <sys:groupId>console.dbpool</sys:groupId>
    <sys:artifactId>EasyAjaxDBPool</sys:artifactId>
    <sys:version>1.0</sys:version>
    <sys:type>rar</sys:type>
  </sys:dependency>
  ...
</sys:dependencies>
```

此外，在 geronimo-web.xml 中，有一个将数据源名称映射到数据库池的条目。以下条目位于 XML 文件中 context-root 元素之后。

```xml
<resource-ref>
  <ref-name>jdbc/CountryDataSource</ref-name>
  <resource-link>EasyAjaxDBPool</resource-link>
</resource-ref>
```

下一个配置是 web.xml 文件中的配置，我们在其中设置资源引用，以便我们的类可以使用它。以下条目位于 web.xml 文件的末尾。

```xml
<resource-ref>
  <res-ref-name>jdbc/CountryDataSource</res-ref-name>
  <res-type>javax.sql.DataSource</res-type>
  <res-auth>Container</res-auth>
  <res-sharing-scope>Shareable</res-sharing-scope>
</resource-ref>
```

最后，我们在这里做的最后一个配置是更改 dwr.xml 文件中的远程类为新的 CountryDerbyDB 类。在 DWR 配置中，我们更改 CountryDB 的 class 参数。

```xml
<create creator="new" javascript="CountryDB">
  <param name="class" value="samples.CountryDerbyDB" />
  <include method="getCountries" />
  <include method="saveCountryNotes" />
  <include method="getCountryData" />
</create>
```

## 测试数据库集成

我们通过更改某个国家的注释然后检查这些注释是否已保存到数据库来验证它是否有效。在此示例中，我们更改了巴哈马的注释，然后使用 Geronimo 控制台检查巴哈马的注释是否确实已存储到数据库中，如以下屏幕截图所示：

| ID | SHORT_NAME | LONG_NAME | CODE | CAPITAL | NOTES |
|---|---|---|---|---|---|
| 1 | Afghanistan | Islamic Republic of Afghanistan | AF | Kabul |  |
| 2 | Albania | Republic of Albania | AL | Tirana |  |
| 3 | Algeria | People's Democratic Republic of Algeria | AG | Algiers |  |
| 4 | Andorra | Principality of Andorra | AN | Andorra la Vella |  |
| 5 | Angola | Republic of Angola | AO | Luanda |  |
| 6 | Antigua and Barbuda | (no long-form name) | AC | Saint John's |  |
| 7 | Argentina | Argentine Republic | AR | Buenos Aires |  |
| 8 | Armenia | Republic of Armenia | AM | Yerevan |  |
| 9 | Australia | Commonwealth of Australia | AS | Canberra |  |
| 10 | Austria | Republic of Austria | AU | Vienna |  |
| 11 | Azerbaijan | Republic of Azerbaijan | AJ | Baku |  |
| 12 | Bahamas | The Commonwealth of The Bahamas | BF | Nassau | In the Caribbean |
| 13 | Bahrain | Kingdom of Bahrain | BA | Manama |  |
| 14 | Bangladesh | People's Republic of Bangladesh | BG | Dhaka |  |
| 15 | Barbados | (no long-form name) | BB | Bridgetown |  |
| 16 | Belarus | Republic of Belarus | BO | Minsk |  |
| 17 | Belgium | Kingdom of Belgium | BE | Brussels |  |

## 后记

数据库集成非常容易实现，实际上不会以任何方式影响 DWR 功能。访问数据库只需使用另一个 Java 类，如果我们将其远程化，就可以使用 DWR 从浏览器访问它。

## 集成 Web 服务

在本章中，我们使用第三方 Web 服务进行信用卡验证。此信用卡验证服务是在互联网上（经过快速搜索）找到的，并且适合我们的示例。Web 服务由 Hypercom 公司托管（http://www.hypercom.com）。

### 开发 Web 服务客户端

该服务是一个标准的 Web 服务，具有 WSDL 描述，这使得我们很容易将其集成到我们的应用程序中。WSDL 位于 http://www.tpisoft.com/smartpayments/validate.asmx?WSDL。正如 URL 所显示的那样，该服务不是基于 Java 的。在 Web 服务世界中，只要接口是标准的，实现方式并不重要。

我们使用 Eclipse 的内置工具为信用卡验证 Web 服务生成客户端代码。

- 1. Web 服务是使用 Eclipse IDE 中的向导创建的。选择项目 DWREasyAjax，在 File | New | Other 菜单中，有一个创建 Web Service Client 的向导。

  ![](img/5b721cfaa97eb697efc92e3914f62fca_153_0.png)

- 3. 服务定义具有 WSDL URL。我们可以保持其他设置不变；按下 **Finish** 将为 **Web Service Client** 生成类。

片刻之后，Eclipse 为 **CreditCardValidator** Web 服务创建了客户端类。默认位置是项目的 **source** 目录。以下屏幕截图显示了生成的类：

![](img/5b721cfaa97eb697efc92e3914f62fca_155_0.png)

我们只需要几个类：CreditCardValidatorSoap，它实际上是一个接口并被间接使用，以及 CreditCardValidatorSoapProxy，它负责实例化 CreditCardValidatorSoap 接口的实现类并充当实现类的代理。以下是 CreditCardValidatorSoap 接口的源代码。此接口是从 WSDL 自动生成的，并且由于 WSDL 文件包含 Web 服务操作的文档，Eclipse 工具也提供了生成的方法的文档，如下所示：

```java
/**
 * CreditCardValidatorSoap.java
 *
 * This file was auto-generated from WSDL
 * by the Apache Axis 1.4 Apr 22, 2006 (06:55:48 PDT) WSDL2Java
 * emitter.
 */
```

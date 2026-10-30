
Oracle 数据库中的期刊目录。通过绑定托管 bean 创建面板网格的步骤如下。

- 1. 创建一个 Java Bean 类来表示期刊目录中的不同字段。
- 2. 从 Java Bean 类创建托管 bean。
- 3. 将托管 bean 绑定到 JSF 页面中的面板网格。

删除上一节中创建的 JSFPanelGrid 应用程序，并在 Applications-Navigator 中为面板网格创建类似的应用程序和项目。从 JavaBean 类生成托管 bean。托管 bean 实现了一个资源，即示例应用程序中的 JavaBean 类。JavaBean 类具有属性以及对应于示例数据库表中不同列的属性的 getter/setter 方法。使用 File | New 向项目添加一个 Java 类。在 New Gallery 窗口中，在 Categories 中选择 General，在 Items 中选择 Java Class。在 Create Java Class 窗口中指定类名 PanelGridBean 和包名 example.panelgrid。将以下列表复制到 PanelGridBean。

```java
package example.panelgrid;
public class PanelGridBean
{
    private int catalogId;
    private String journal;
    private String publisher;
    private String edition;
    private String title;
    private String author;
    private String href;
    public PanelGridBean() {
    }
    public int getCatalogId() {
        return this.catalogId;
    }
    public void setCatalogId(int catalogId) {
        this.catalogId=catalogId;
    }
    public String getJournal() {
        return this.journal;
    }
    public void setJournal(String journal) {
        this.journal=journal;
    }
    public String getPublisher() {
        return this.publisher;
    }
    public void setPublisher(String publisher) {
        this.publisher=publisher;
    }
    public String getEdition() {
return this.edition;
}
public void setEdition(String edition){
    this.edition=edition;
}
public String getTitle(){
    return this.title;
}
public void setTitle(String title){
    this.title=title;
}
public String getAuthor(){
    return this.author;
}
public void setAuthor(String author){
    this.author=author;
}
public String getHref(){
    return this.href;
}
public void setHref(String href){
    this.href=href;
}
}
```

接下来，使用 File | New 向项目添加一个 JSF JSP 页面 catalog.jsp。在 New Gallery 窗口中，在 Categories 中选择 Web Tier | JSF，并在 Items 中选择 JSF JSP。单击 OK。接下来，我们将从 Java Bean 类创建托管 bean。在 Applications Navigator 中打开 faces-config.xml 配置文件节点，并在编辑器窗口中选择 Overview 选项卡。选择 Managed Beans 节点（如果尚未选择）。单击 New 添加新的托管 bean。

![](img/553e44bc30e70d0d8e58278a7ece8da8_332_0.png)

在 Create Managed Bean 窗口中，指定托管 bean 名称 PanelGridMBean，并选择 JavaBean 类 PanelGridBean 作为托管 bean 的 Class。选择 request 作为 Scope 并单击 OK。托管 bean PanelGridMBean 被创建。

![](img/553e44bc30e70d0d8e58278a7ece8da8_333_0.png)

JSF 面板网格也可用于指定输入表单中输入文本字段的初始值。在 faces-config.xml 文件中指定托管 bean 属性的初始值。选择 faces-config.xml 文件节点和 Source 选项卡。使用 <managed-property/> 元素为 PanelGridMBean 添加托管 bean 属性的初始值。指定了托管 bean 属性的 faces-config.xml 中的 managed-bean 元素如下所示：

```
<managed-bean>
    <managed-bean-name>backing_catalog</managed-bean-name>
    <managed-bean-class>panelgrid.backing.Catalog</managed-bean-class>
    <managed-bean-scope>request</managed-bean-scope>
    <!--oracle-jdev-comment:managed-bean-jsp-link:1catalog.jsp-->
</managed-bean>
<managed-bean>
    <managed-bean-name>PanelGridMBean</managed-bean-name>
    <managed-bean-class>example.panelgrid.PanelGridBean</managed-bean-class>
    <managed-bean-scope>request</managed-bean-scope>
    <managed-property>
    <property-name>catalogId</property-name>
    <value>1</value>
    </managed-property>
    <managed-property>
    <property-name>journal</property-name>
    <value>Oracle Magazine</value>
    </managed-property>
    <managed-property>
    <property-name>publisher</property-name>
```

向 JSF 页面 catalog.jsp 添加一个面板网格。在 JSF 页面中定位光标，并在 Component Palette 中选择 Panel Grid。

![](img/553e44bc30e70d0d8e58278a7ece8da8_334_0.png)

**创建 PanelGrid 向导** 开始运行。单击 **下一步**。在 **面板网格选项** 窗口中选择 **从托管 Bean 或表达式创建面板网格** 单选按钮。单击 **绑定** 将面板网格与托管 bean 绑定。

![](img/553e44bc30e70d0d8e58278a7ece8da8_335_0.png)

在 **表达式生成器** 窗口中，选择 **JSF 托管 Bean | PanelGridMBean** 节点，并使用 > 按钮将 **PanelGridMBean** 添加到 **表达式** 字段。

![](img/553e44bc30e70d0d8e58278a7ece8da8_335_1.png)

## 创建 JSF 面板网格

在 **面板网格选项** 窗口中，在 **类** 字段中选择 JavaBean 类 **PanelGridBean** 并单击 **下一步**。

![](img/553e44bc30e70d0d8e58278a7ece8da8_336_0.png)

在 **字段选择** 窗口中对所选字段的顺序进行排序，使其对应于 **OE.CATALOG** 数据库表列。单击 **下一步**。

![](img/553e44bc30e70d0d8e58278a7ece8da8_336_1.png)

在 **组件详细信息** 中选择 `<header>` 行并在 **标签** 字段中指定一个标题。选择 `Catalog Id` 行并将 **类型** 选择为 **输入文本**。通过选择 **必填** 复选框将 `Catalog Id` 字段指定为必填字段。选择 **关联消息** 复选框以向面板网格添加消息组件列。错误消息将在消息组件中显示。单击 **高级** 按钮为 Catalog Id 输入文本字段注册验证器和转换器。JSF 验证器用于验证输入字段值，JSF 转换器用于转换输入字段值。

![](img/553e44bc30e70d0d8e58278a7ece8da8_337_0.png)

在 **高级属性** 窗口中，选择 **整数** 转换器。Catalog Id 输入文本字段的值将通过转换器转换为 `Integer` 类型。

![](img/553e44bc30e70d0d8e58278a7ece8da8_337_1.png)

## 创建 JSF 面板网格

接下来，选择 Validator 选项卡并选择 Validate Long Range 验证器。为验证器指定 Minimum 和 Maximum 值。验证器验证 Catalog Id 输入字段值是否在指定范围内。

![](img/553e44bc30e70d0d8e58278a7ece8da8_338_0.png)

在 Components Detail 窗口中，为其他字段将 Type 指定为 Input Text 并单击 Next。在 Finish 窗口中单击 Finish。一个 JSF 面板网格已添加到 JSF 页面 catalog.jsp。向 JSF 页面添加一个命令按钮 Submit。

![](img/553e44bc30e70d0d8e58278a7ece8da8_338_1.png)

接下来，我们将修改 backing bean 中的命令按钮 action 方法。在 Design 选项卡中双击命令按钮。在 Bind Action 属性窗口中为 Managed Bean 和 Method 选择默认设置并单击 OK。在命令按钮 action 方法中检索输入字段值并更新指定 catalog id 的数据库表。在 backing bean 类中为 java.sql 包添加 import 语句。在 commandButton1_action 方法中使用 Class.forName 方法加载 Oracle JDBC 驱动程序 oracle.jdbc.OracleDriver。

使用 DriverManager 类的 getConnection 方法创建 Connection 对象。使用 createStatement 方法从 Connection 对象创建 Statement 对象。将结果集类型指定为可滚动，结果集并发指定为只读。使用 getValue 方法检索 Input Text 字段值。通过使用 executeQuery 方法运行 UPDATE SQL 语句来更新数据库表 Catalog。commandButton1_action 方法如下所示：

```java
public String commandButton1_action() {
    // Add event code here...
    ResultSet rs=null;
    try{Class.forName("oracle.jdbc.OracleDriver");
        String url="jdbc:oracle:thin:@localhost:1521:ORCL";
        Connection connection =
        DriverManager.getConnection(url,"OE", "pw");
    Statement
    stmt=connection.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE,
    ResultSet.CONCUR_READ_ONLY);
        int catalogId=((Integer)(inputText0.getValue())).
intValue();
        String journal=(String)inputText1.getValue();
        String publisher=(String)inputText2.getValue();
        String edition=(String)inputText3.getValue();
        String title=(String)inputText4.getValue();
        String author=(String)inputText5.getValue();
        String href=(String)inputText6.getValue();
    String query="UPDATE OE.CATALOG SET
    JOURNAL="+"'"+journal+"'"+","+"PUBLISHER="+"'"+publisher+"'"+
    ","+"EDITION="+"'"+edition+"'"+","+"  TITLE=" +"'"+title+"'"+","+"AUTHOR="+"'"+author+"'"+"," + " HREF="+"'"+href+"'"+"WHERE
    CATALOGID="+"'"+catalogId+"'";
    stmt.executeUpdate(query);stmt.close();connection.close();}
    catch(SQLException e){System.out.println(e.getMessage());}
    catch(ClassNotFoundException e)
    {System.out.println(e.getMessage());}return null;}
```

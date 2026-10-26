

```vb
' build the strings with the fields we want to index
strTitle = ModInfo.ModuleTitle + " - " + objEmployee.ContactInfo()
strDescription = objEmployee.EmpFirstName & " " & objEmployee.EmpLastName & " - Hired " & objEmployee.FormattedHireDate & " New Hire: " & objEmployee.NewHire & " Email:" & objEmployee.EmailAddress
intAuthor = objEmployee.CreatedByUser
dtePubDate = objEmployee.CreatedDate
strSearchKey = objEmployee.ItemId.ToString
strContent = System.Web.HttpUtility.HtmlDecode(objEmployee.EmpFirstName & " " & objEmployee.EmpLastName & " " & objEmployee.Salary.ToString & " " & objEmployee.HireDate.ToString & " " & objEmployee.EmailAddress)

' Create a search item and pass the strings as arguments
Dim SearchItem As SearchItemInfo = New SearchItemInfo(strTitle, strDescription, intAuthor, dtePubDate, ModInfo.ModuleID, strSearchKey, strContent, "ItemId=" & objEmployee.ItemId.ToString)
SearchItemCollection.Add(SearchItem)

Next

Return SearchItemCollection

End Function
```

- 5. 现在我们已经定义了模块中可搜索的内容，重建并重新部署模块。
- 6. 下一步是对模块数据进行索引以使其可搜索。以 SuperUser 身份登录。
- 7. 从 **Host** 菜单中选择 **Search Admin**。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_340_0.png)

## 导出模块数据

DNN 模块提供的一项功能是可以将数据库中的数据导出到 XML 文件中。这使得可以通过一个门户收集的数据保存或传输到另一个门户或其他应用程序。此功能出现在模块操作菜单中，也出现在页面导出和门户模板创建中。导出和导入使用 `iPortable` 接口完成，并且需要定义模块中应包含在导出中的字段。

在本教程中，我们将使用来自 *第 5 章，构建、调试和部署模块* 的示例 Employee 模块，并查看我们必须添加的代码以实现接口并执行导出。

### 准备工作

要跟随本教程，你必须已完成 *第 5 章* 中的以下教程：

- - *作为独立包部署模块*

### 如何操作...

- 1. 首先启动开发工具并加载 Employee 项目。
- 2. 双击打开 `EmployeeController.vb` 文件。
- 3. 滚动到 **Optional Interfaces** 区域并查找 `ExportModule` 函数。
- 4. 我们需要定义模块中可用于导出的字段。原始 Starter Kit 创建了一个名为 Content 的临时字段。我们需要用实际的模块字段替换 Content，方法如下：

```
Public Function ExportModule(ByVal ModuleID As Integer) As String
    Implements DotNetNuke.Entities.Modules.IPortable.ExportModule

    Dim strXML As String = ""

    Dim colEmployees As List(Of EmployeeInfo) = _
                                      GetEmployees(ModuleID)
    If colEmployees.Count <> 0 Then
        strXML += "<Employees>"
        Dim objEmployee As EmployeeInfo
        For Each objEmployee In colEmployees
            strXML += "<Employee>"

                strXML += "<EmpFirstName>" &
```

```vb
XMLEncode(objEmployee.EmpFirstName) & "</EmpFirstName>"
strXML += "<EmpLastName>" &
    XMLEncode(objEmployee.EmpLastName) & "</EmpLastName>"
strXML += "<ManagerNo>" &
    XMLEncode(objEmployee.ManagerNo) & "</ManagerNo>"
strXML += "<HireDate>" &
    XMLEncode(objEmployee.HireDate) & "</HireDate>"
strXML += "<Salary>" &
    XMLEncode(objEmployee.Salary) & "</Salary>"

strXML += "</Employee>"
Next
strXML += "</Employees>"
End If

Return strXML

End Function
```

- 5. 这就是所需的所有代码。要测试结果，首先重建并重新部署模块。
- 6. 转到包含 Employee 模块的页面并从模块操作菜单中选择 **Export Content**。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_343_0.png)

- 7. 选择一个临时文件夹（例如 Root）并单击 **Export** 链接。
- 8. 要查看它创建的 XML 文件，请查看门户的根文件夹（例如，`dotnetnuke\\Portals\\0`）。查找文件 content `.ACMEEmployee.ACMEEmployees.xml`。
- 9. 根据员工表中记录的数量，该文件看起来可能如下所示：

```
```xml
<?xml version="1.0" encoding="utf-8" ?>
- <content type="ACMEEmployee" version="01.00.00">
  - <Employees>
    - <Employee>
      - <EmpFirstName>
        - <![CDATA[ John
]]>
```

## 具有挑战性的自定义模块

```xml
</EmpFirstName>
- <EmpLastName>
  - <![CDATA[ Smith
]]>
</EmpLastName>
- <ManagerNo>
  - <![CDATA[ 0
]]>
</ManagerNo>
- <HireDate>
  - <![CDATA[ 3/23/2010
]]>
</HireDate>
- <Salary>
  - <![CDATA[ 25000
]]>
</Salary>
</Employee>
</Employees>
</content>
```

### 工作原理...

向 `ExportModule` 函数添加代码提供了保存自定义数据到 XML 文件所需的 XML 标签。`iPortable` 接口完成了其余工作。

### 还有更多...

你可能想导出模块中的所有字段，但在选择导出字段时有几个简单的规则：

- - 不要包含 ModuleId 或 ItemId，因为系统在导入时会重新生成它们
- - 同样，没有必要导出创建和更新日期
- - 除非导出是用于另一个模块或应用程序，否则你不需要包含计算字段
- - 通常，只导出你的导入代码使用的插入例程所需的字段（例如 `AddEmployee` 例程）

### 另请参阅

要了解如何使用此导出加载模块表中的记录，请参阅下一个教程  
*导入模块数据。*

## 导入模块数据

正如我们在导出模块数据教程中看到的，DNN 提供了使用 iPortable 接口在数据库中导入和导出数据的工具。要使用该接口，我们必须定义导出文件中的字段并定义导入文件时如何保存这些字段。

在本教程中，我们将使用上一教程中的导出文件导入到示例 Employee 模块中。

### 准备工作

要跟随本教程，你必须已完成以下先前教程：

- - 部署模块作为独立包
- - 导出模块数据

### 如何操作...

- 1. 首先启动开发工具并加载 Employee 项目。
- 2. 双击打开 EmployeeController.vb 文件。
- 3. 滚动到 Optional Interfaces 区域并查找 ImportModule 子程序。
- 4. 我们需要定义可以在 XML 文件中导入的字段。原始 starter kit 创建了一个名为 Content 的临时字段。我们需要用实际字段替换 Content，方法如下：

```
Public Sub ImportModule(ByVal ModuleID As Integer,
    ByVal EmpFirstName As String, ByVal Version As String,
    ByVal UserId As Integer) Implements
        DotNetNuke.Entities.Modules.IPortable.ImportModule

Dim xmlEmployee As XmlNode
Dim xmlEmployees As XmlNode = GetContent(EmpFirstName,
    "Employees")

For Each xmlEmployee In xmlEmployees.SelectNodes("Employee")
Dim objEmployee As New EmployeeInfo
objEmployee.ModuleId = ModuleID

objEmployee.EmpFirstName =
    xmlEmployee.SelectSingleNode("EmpFirstName").InnerText
objEmployee.EmpLastName =
    xmlEmployee.SelectSingleNode("EmpLastName").InnerText
objEmployee.ManagerNo =
```

```
xmlEmployee.SelectSingleNode("ManagerNo").InnerText
objEmployee.HireDate =
    xmlEmployee.SelectSingleNode("HireDate").InnerText
objEmployee.Salary =
    xmlEmployee.SelectSingleNode("Salary").InnerText

objEmployee.CreatedByUser = UserId
AddEmployee(objEmployee)
    Next

End Sub
```

- 5. 这就是导入方所需的所有代码。要测试结果，首先重建并重新部署模块。
-   
- 6. 转到包含 Employee 模块的页面并从模块操作菜单中选择 **Import Content**。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_346_0.png)

- 7. 为文件夹选择 Root 并选择上次导出中的文件。
-   
- 8. 单击 Import 链接。
-   
- 9. 记录现在将从 XML 导出文件加载到 Employee 表中。如果导入回创建导出的表，你可能会看到重复记录。这是因为我们没有在导出中包含 ItemID。当新记录被导入时，会生成新的 ItemID。

### 工作原理...

向 `ImportModule` 函数添加代码定义了 XML 文件中的字段如何映射到 `EmployeeInfo` 对象，该对象将数据保存到数据库中。

### 导出和导入用户定义表

在前两个教程中，我们看到了如何使用 XML 文件在模块中导入和导出数据。这是所有 DNN 模块都可用的内置功能。使用 Form 和 List 模块，你有额外的选项可以使用逗号分隔值 CSV 文件而不是 XML 来导入和导出自定义表中的数据。


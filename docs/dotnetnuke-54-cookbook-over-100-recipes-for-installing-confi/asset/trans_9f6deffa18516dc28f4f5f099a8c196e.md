

## 创建自己的模块

+   15. 将第二部分改为：

```
values (
    @ModuleId,
    @EmpFirstName,
    @EmpLastName,
    @ManagerNo,
    @HireDate,
    @Salary,
    @UserID,
    getdate()
)
```

+   16. 因此整个存储过程应为：

```
create procedure {databaseOwner}{objectQualifier}
                         ACME_AddEmployee

    @ModuleId       int,
    @EmpFirstName   nvarchar(30),
    @EmpLastName    nvarchar(30),
    @ManagerNo      int,
    @HireDate       datetime,
    @Salary         float,
    @UserID         int

as

insert into {objectQualifier}_Employee (
    ModuleId,
    EmpFirstName,
    EmpLastName,
    ManagerNo,
    HireDate,
    Salary,
    CreatedByUser,
    CreatedDate
)
values (
    @ModuleId,
    @EmpFirstName,
    @EmpLastName,
    @ManagerNo,
    @HireDate,
    @Salary,
    @UserID,
    getdate()
```

)

```
GO
```

17. 下一个存储过程 ACME_UpdateEmployee 类似，因此我们有两个地方需要进行更改。将五个字段添加到存储过程的参数列表中，然后将五个字段添加到更新语句中。存储过程现在应如下所示：

```
create procedure
{databaseOwner}.{objectQualifier}ACME_UpdateEmployee

@ModuleId          int,
@ItemId            int,
@EmpFirstName      nvarchar(30),
@EmpLastName       nvarchar(30),
@ManagerNo         int,
@HireDate          datetime,
@Salary            float,
@UserID            int

as

update {objectQualifier}ACME_Employee
set    EmpFirstName   = @EmpFirstName,
       EmpLastName    = @EmpLastName ,
       ManagerNo      = @ManagerNo,
       HireDate       = @HireDate,
       Salary         = @Salary,
       CreatedByUser  = @UserID,
       CreatedDate    = getdate()
where  ModuleId = @ModuleId
and    ItemId = @ItemId

GO
```

18. 最后的存储过程 ACME_DeleteEmployee 无需修改，因为它不引用任何字段。

## 其工作原理...

DNN 中的模块遵循 Microsoft 的 N-Tier 架构。这是一种将应用程序逻辑与业务和数据库逻辑分离的机制。在本教程中，我们构建了数据库层的存储过程。

> 你可以从 David Chappell 和 Steve Kirk 的这篇优秀文章中了解更多关于 N-Tier 架构的信息，网址为 http://msdn.microsoft.com/en-us/library/ms978384.aspx。

## 还有更多...

创建存储过程后，为新模块的最后一步是将其连接到数据库。教程《将模块连接到数据库》演示了如何创建支持数据库和业务逻辑层的过程。

## 将模块连接到数据库

要将向导生成的代码连接到模块的数据库表，我们必须遍历项目代码并用我们的字段和数据类型替换名为 Content 的占位符。

## 准备工作

要跟随本教程，你必须已完成以下先前的教程：

-   - 使用 Starter Kit 创建新模块
-   - 创建带键的数据库表
-   - 创建存储过程

## 如何操作...

-   1. 首先启动你喜欢的开发工具（Visual Studio 或 Visual Web Developer Express）。
-   2. 在屏幕左侧查找“最近的项目”。点击 Employee 以加载项目。
-   3. 在屏幕右侧查看“解决方案资源管理器”，展开名为 Components 的文件夹并双击 DataProvider.vb。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_123_0.png)

4. 滚动到文件底部并查找名为 Abstract methods 的代码区域。我们必须在这两个地方用实际的字段名替换占位符 Content。将代码改为：

```
#Region "Abstract methods"

Public MustOverride Function GetEmployees(ByVal ModuleId As Integer) As IDataReader
Public MustOverride Function GetEmployee(ByVal ModuleId As Integer,ByVal ItemId As Integer) As IDataReader
Public MustOverride Sub AddEmployee(ByVal ModuleId As Integer, ByVal EmpFirstName As String, ByVal EmpLastName As String, ByVal ManagerNo As Integer, ByVal HireDate As Datetime, ByVal Salary As Double, ByVal UserId As Integer)
Public MustOverride Sub UpdateEmployee(ByVal ModuleId As Integer, ByVal ItemId As Integer, ByVal EmpFirstName As String, ByVal EmpLastName As String, ByVal ManagerNo As Integer, ByVal HireDate As Datetime, ByVal Salary As Double, ByVal UserId As Integer)
```

## 创建自己的模块

```
```vb
Public MustOverride Sub DeleteEmployee(ByVal ModuleId As
                                        Integer,ByVal ItemId As
Integer)

#End Region
```
```

-   5. 现在我们在下一个文件中重复该过程。双击 `SqlDataProvider.vb` 并滚动到文件底部的 Public Methods 区域。我们需要在两个参数列表和两个参数列表中更改代码：

```
```vb
#Region "Public Methods"

Public Overrides Function GetEmployees(ByVal ModuleId As
                                       Integer) As IDataReader
    Return CType(SqlHelper.ExecuteReader(ConnectionString,
                         GetFullyQualifiedName("GetEmployees"),
                         ModuleId), IDataReader)
End Function

Public Overrides Function GetEmployee(ByVal ModuleId As
    Integer, ByVal ItemId As Integer) As IDataReader
    Return CType(SqlHelper.ExecuteReader(ConnectionString,
                         GetFullyQualifiedName("GetEmployee"), ModuleId,
                         ItemId), IDataReader)
End Function

Public Overrides Sub AddEmployee(ByVal ModuleId As
    Integer, ByVal EmpFirstName As String,
    ByVal EmpLastName As String, ByVal ManagerNo As
                                             Integer,
    ByVal HireDate As DateTime,
    ByVal Salary As Double, ByVal UserId As Integer)
    SqlHelper.ExecuteNonQuery(ConnectionString,
                GetFullyQualifiedName("AddEmployee"),
                ModuleId, EmpFirstName,
                EmpLastName, ManagerNo, HireDate,
                Salary, UserId)
End Sub

Public Overrides Sub UpdateEmployee(ByVal ModuleId As
                                    Integer, ByVal ItemId As Integer,
                                    ByVal EmpFirstName As String,
                                    ByVal EmpLastName As String,
                                    ByVal ManagerNo As Integer,
                                    ByVal HireDate As DateTime,
                                    ByVal Salary As Double,
                                    ByVal UserId As Integer)
    SqlHelper.ExecuteNonQuery(ConnectionString,
                GetFullyQualifiedName("UpdateEmployee"),
                ModuleId, ItemId, EmpFirstName,
                EmpLastName, ManagerNo, HireDate,
```

8. 到目前为止一切顺利。现在我们需要在两个地方更新 EmployeeInfo 文件。双击 EmployeeInfo.vb 并向下滚动一点到本地属性声明部分。

9. 将包含 Content 的一行改为我们字段的五行：

```
' local property declarations
Private _ModuleId As Integer
Private _ItemId As Integer
Private _EmpFirstName As String
Private _EmpLastName As String
Private _ManagerNo As Integer
Private _HireDate As DateTime
Private _Salary As Double
Private _CreatedByUser As Integer
Private _CreatedDate As DateTime
Private _CreatedByUserName As String
```

10. 在文件的其余部分，我们需要再次删除 Content 属性并用我们的五个字段替换它。向下滚动并找到包含 Content 属性的部分。现在将代码改为我们的五个属性过程：

```
Public Property ItemId() As Integer
    Get
        Return _ItemId
    End Get
    Set(ByVal Value As Integer)
        _ItemId = Value
    End Set
End Property
```

```
Public Property EmpFirstName() As String
    Get
        Return _EmpFirstName
    End Get
    Set(ByVal Value As String)
        _EmpFirstName = Value
    End Set
End Property
```

```
Public Property EmpLastName() As String
    Get
        Return _EmpLastName
    End Get
    Set(ByVal Value As String)
        _EmpLastName = Value
    End Set
End Property
```


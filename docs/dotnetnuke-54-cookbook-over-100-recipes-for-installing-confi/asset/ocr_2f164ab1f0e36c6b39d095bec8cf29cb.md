```
End Property

Public Property ManagerNo() As Integer
    Get
        Return _ManagerNo
    End Get
    Set(ByVal Value As Integer)
        _ManagerNo = Value
    End Set
End Property

Public Property HireDate() As Datetime
    Get
        Return _HireDate
    End Get
    Set(ByVal Value As Datetime)
        _HireDate = Value
    End Set
End Property

Public Property Salary() As Double
    Get
        Return _Salary
    End Get
    Set(ByVal Value As Double)
        _Salary = Value
    End Set
End Property

Public Property CreatedByUser() As Integer
    Get
        Return _CreatedByUser
    End Get
    Set(ByVal Value As Integer)
        _CreatedByUser = Value
    End Set
End Property
```

11. Select Save All from the File menu to save your project files.
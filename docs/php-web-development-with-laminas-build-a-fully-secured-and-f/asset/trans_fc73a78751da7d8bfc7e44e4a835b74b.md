

## 角色和资源

授予权限

| 代码 | 角色 | 资源 |
|---|---|---|
| 8 | ADMIN | discount.edit |
| 7 | ADMIN | discount.edit |
| 6 | ADMIN | discount.index |
| 5 | ADMIN | discount.index |
| 1 | ADMIN | discountapi.create |
| 4 | ADMIN | discountapi.delete |
| 2 | ADMIN | discountapi.get |
| 3 | ADMIN | discountapi.update |
| 16 | ADMIN | employee.edit |
| 15 | ADMIN | employee.edit |
| 13 | ADMIN | employee.index |
| 14 | ADMIN | employee.index |
| 9 | ADMIN | employeeapi.create |
| 12 | ADMIN | employeeapi.delete |

图 10.1: 角色及其资源

图 10.1 所示页面允许您为角色移除权限。如您所见，列表按角色名称排序，而非按权限代码排序。该列表由 RoleResourceTable->getAll 方法生成。该方法构建并执行一条 SQL SELECT 语句，用于连接 resources_role、resources 和 roles 表。getAll 方法已在后文列出。事实上，您在第 7 章“请求控制和数据视图”的“测试 Web 界面与 API 之间的交互”一节中，已经见过与 Select 类一起使用 INNER JOIN 子句的示例。以下源代码中的新增内容是使用 order 方法，它会生成 SQL 的 ORDER BY 子句：

```php
public function getAll($where = null) : iterable
{
    $select = new Select($this->tableGateway->
        getTable());
    $select->join('resources','resources_role.code_resource=resources.code', ['resource' => 'name'])
->join('roles', 'resources_role.code_role=roles.code', ['role' => 'name'])
->order(['roles.name', 'resources.name']);
if (!is_null($where)) {
    $select->where($where);
}
return $this->tableGateway->selectWith($select);
}
```

您可以看到，图 10.1 所示页面有一个“授予权限”超链接。该超链接会调用 RoleResourceController->editAction 方法。该方法使用两个表网关对象来填充两个不同的下拉列表，如以下源代码所示：

```php
public function editAction()
{
    $form = new RoleResourceForm();
    $resources = [];
    $rows = $this->resourceTable->getAll();
    foreach($rows as $row) {
        $resources[$row->code] = $row->name;
    }

    $roles = [];
    $rows = $this->roleTable->getAll();
    foreach($rows as $row) {
        $roles[$row->code] = $row->name;
    }

    $form->get('code_resource')->setValueOptions
        ($resources);
    $form->get('code_role')->setValueOptions($roles);
    return new ViewModel([
        'form' => $form
    ]);
}
```

该方法提供一个包含图 10.2 所示表单的 HTML 页面：

![](img/e6cf5550804ccae9d1859270da8688f5_271_0.png)

图 10.2: 向角色授予权限的表单

图 10.2 所示表单允许您选择一个角色并授予其访问资源的权限。如果您单击保存按钮，该关联将被持久化（如果尚不存在）。持久化由 RoleResourceTable->save 方法完成，如以下源代码所示：

```php
public function save(AbstractModel $model): bool
{
    $set = $model->toArray();
    $resourcesRole = $this->getByFields([
        'code_resource' => $model->resource->code,
        'code_role' => $model->role->code,
    ]);
    try {
        if (empty($resourcesRole->code)) {
            $this->tableGateway->insert($set);
        }
    } catch (\Exception $e) {
        error_log($e->getMessage());
        return false;
    }
    return true;
}
```

请注意，此 save 方法会调用 getByFields 方法。getByFields 方法已添加到 Generic 模块的 AbstractTable 类中，以允许使用多个过滤条件进行查询。getByFields 方法接收一个数组作为参数，如以下源代码所示：

```php
public function getByFields(array $fields): AbstractModel
{
    $where = [];
    foreach($fields as $field => $value) {
        $where[$field] = $value;
    }
    $rowSet = $this->getAll($where);
    if ($rowSet->count() == 0) {
        $modelName = $this->modelName;
        return new $modelName();
    }
    return $rowSet->current();
}
```

现在，您可以将资源与角色关联，从而允许角色访问这些资源。

在下一节中，我们将了解如何实现角色与用户之间的关联。

## 将角色与用户关联

我不再重复我们已介绍过的注册细节。员工角色的实现代码可在本章的源代码文件夹中找到（在“技术要求”部分中提及）。在此，我们将仅关注与库存模块（即员工）中角色与用户之间关联相关的差异。员工角色的注册基于 employee_roles 表。

好了，既然我们已实现库存模块的最后一个注册，是时候为我们的注册向菜单页面添加超链接了。因此，我们需要修改 module/Inventory/view/inventory/menu 文件夹中的 index.phtml 文件，如下所示：

### index.phtml

```php
<h1>Inventory Module</h1>
<p>Hi, <?=$this->user?></p>
<ul>
<li><a href="<?=$this->url('inventory', ['controller' => 'product'])?>">Product Registration</a></li>
<li><a href="<?=$this->url('inventory', ['controller' => 'discount'])?>">Discount Registration</a></li>
<li><a href="<?=$this->url('inventory', ['controller' => 'employee'])?>">Employee Registration</a></li>
<li><a href="<?=$this->url('inventory', ['controller' => 'role'])?>">Role Registration</a></li>
<li><a href="<?=$this->url('inventory', ['controller' => 'resource'])?>">Resource Registration</a></li>
<li><a href="<?=$this->url('inventory', ['controller' => 'roleresource'])?>">Resources of a role</a></li>
<li><a href="<?=$this->url('inventory', ['controller' => 'employeerole'])?>">Roles of an employee</a></li>
</ul>

<a href="<?=$this->url('inventory', ['action' => 'logout'])?>">Logout</a>
```

此 index.phtml 文件将显示以下页面：

![](img/e6cf5550804ccae9d1859270da8688f5_274_0.png)

图 10.3: 库存模块的菜单页面

**角色的资源** 超链接将带您进入前一节中展示的注册列表页面。**员工的角色** 超链接将带您进入以下页面：

![](img/e6cf5550804ccae9d1859270da8688f5_274_1.png)

图 10.4: 员工角色列表页面

图 10.4 所示页面允许您为员工移除角色。该列表由 EmployeeResourceTable->getAll 方法生成。该方法构建并执行一条 SQL SELECT 语句，用于连接 resources_role、resources 和 roles 表。getAll 方法已在后文列出。这是 INNER JOIN 子句的另一个用法示例，如前一节所示。

在以下代码块中，我们可以看到 EmployeeResourceTable->getAll 方法的源代码：

```php
public function getAll($where = null): iterable
{
    $select = new Select($this->tableGateway->
    getTable());
    $select->join('roles','employee_roles.code_role=roles.code', ['role' => 'name'])
    ->join('employees','employee_roles.ID_employee=employees.ID', ['employee' => 'name'])
    ->order(['employees.name', 'roles.name']);
    if (!is_null($where)) {
    $select->where($where);
    }
    return $this->tableGateway->selectWith($select);
}
```

如您所见，图 10.4 所示页面具有一个“向用户添加角色”超链接。该超链接会调用 EmployeeRoleController->editAction 方法。该方法使用两个表网关对象来填充两个不同的下拉列表，如以下源代码所示：

```php
public function editAction()
{
    $form = new EmployeeRoleForm();
    $employees = [];
    $rows = $this->employeeTable->getAll();
    foreach($rows as $row) {
    $employees[$row->ID] = $row->name;
    }
    $roles = [];
    $rows = $this->roleTable->getAll();
    foreach($rows as $row) {
    $roles[$row->code] = $row->name;
    }
```


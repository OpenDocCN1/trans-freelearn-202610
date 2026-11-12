```
resource=resources.code', ['resource' => 'name'])
->join('roles', 'resources_role.code_role=
roles.code', ['role' => 'name'])
->order(['roles.name', 'resources.name']);
if (!is_null($where)) {
    $select->where($where);
}
return $this->tableGateway->selectWith($select);
}
```

You can see that the page shown in Figure 10.1 has a Grant permissions hyperlink. This hyperlink invokes the RoleResourceController->editAction method. This method uses two table gateway objects to populate two different drop-down lists, as we can see in the following source code:

```
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
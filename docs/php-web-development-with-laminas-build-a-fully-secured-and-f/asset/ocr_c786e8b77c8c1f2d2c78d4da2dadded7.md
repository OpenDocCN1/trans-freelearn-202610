The second difference is the following line:

```
$this->auth->getStorage()->write($this->adapter->getResultRowObject(null,'password'));
```

This instruction writes the user object (without the password) to the authentication session. It is necessary because we intend to later revalidate the permission data, so we need to store the user's primary key and not only the credential. This change will require some adjustments where identity is used, but only a few.

Similar to the previous method, the IdentityManager class will have a logout method, which will encapsulate most of the code from IndexController->logoutAction:

```
public function logout() : void
{
    $this->auth->clearIdentity();
    $session = new SessionManager();
    try {
        $session->destroy();
    } catch (\Exception $e) {
        error_log($e->getMessage());
    }
}
```

The IdentityManager class will encapsulate the IndexController->loadAuthorizationData method too. You can see a little difference in the recovery of the employee's primary key employeee:

```
public function loadAuthorizationData() : void
{
    $role = new Role($this->auth->getIdentity()->name);
    $row = $this->auth->getStorage()->read();
    $resources = $this->resourceTable->
        getResourcesFromAnEmployee($row->ID);
    foreach($resources as $resource) {
        $role->addPermission($resource->name . ':' .
            $resource->method);
    }
    $rbac = new Rbac();
}
```
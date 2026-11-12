```
});;
};

$(document).ready(function() {
  $("#login").click(login);
});
```

You can see that index.js associates the login function with the event of the button click. The login function sends an HTTP POST request with data from the login form.

At this point, the tests fail yet, because the loginAction and logoutAction methods do not exist in the IndexController class. So, let us create them:

```
public function loginAction()
{
  $nickname = $this->request->getPost('nickname');
  $password = $this->request->getPost('password');

  $logged = false;

  return new JsonModel([
    'logged' => $logged
  ]);
}

public function logoutAction()
{
  return $this->redirect()->toRoute('inventory');
}
```

At this point, the loginAction method is incomplete. Because of this, the tests fail yet again. You will receive an HTTP 404 error for each one of the new IndexController methods. For example, for the loginAction method test, the output of PHPUnit must be this:

> Failed asserting response code "302", actual status code is
> "404"

If you request http://localhost:8000/inventory/index/login in the browser, you will see the page seen in Figure 9.2:
```
MEFManager.Compose(this);
}
public void Login(string username, string password)
{
    string message = _accountService.Login(username,
                                            password);
    _view.DisplayMessage(message);
}
public void GoToRegister()
{
    _redirector.GoToAccountRegisterPage();
}
public void GoToRecoverPassword()
{
    _redirector.GoToAccountRecoverPasswordPage();
}
}
}
}
```

Note that in the **Init()** method, the presenter sets up the objects that it needs to get its job done by using MEF framework. If a page needs to display data on its initial load, this is where it would happen. Beyond the initialization of the presenter, you will notice that the presenter has three other methods: Login(), GoToRegister(), and GoToRecoverPassword().

The Login() method handles the button-click event that the **View** passes to it. Note that even here we do not have a lot of logic to manage. The presenter is quick to pass off the responsibility of logging the user in to the AccountService object that we discussed earlier. It simply expects a friendly message back from the AccountService to describe how the login process went. As we know, if it gets a message back, it means that the login failed; otherwise the AccountService will redirect the user appropriately. Once the login is complete, the presenter uses the view's DisplayMessage() method to inform the user of its status.

The GoToRegister() and GoToRecoverPassword() methods simply utilize the Redirector object to send the user to the appropriate page on the site. While this may seem a bit extreme, remember that it follows the good design principles. If you follow this across your entire site, you will reap the following three benefits:

+   1. You can easily swap out the UI and expect the same results with minimum efforts.
+   2. As your redirection code is in one place, when several links use the same method to redirect to a location, you can change this redirection in that place and impact all the links across your site.
+   3. This aids the testability of your site!
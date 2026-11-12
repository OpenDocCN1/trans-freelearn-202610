```
Failed asserting response code "302", actual status code is
"500"

Exceptions raised:
Exception 'Error' with message 'Class "Laminas\\Session\\
Container" not found' in /opt/lampp/htdocs/whatstore/vendor/
laminas/laminas-authentication/src/Storage/Session.php:62
```

This error occurs in a class of laminas-authentication. But, wait! If laminas-
authentication needs this Container class, why didn't Composer install it? It is because
Laminas components try to keep a weak coupling among them. You are not required to use Container
to store authentication data. You can use another class in the AuthenticationService constructor. It is possible to inject another storage class
in the AuthenticationService constructor. However, as we don't use a customized storage
class, laminas-authentication requires one and asks for implementation available at another
component – laminas-session, by default.

No problem. We will use laminas-session in the next chapter too. Add and install the laminas-
session component, similar to how we did so with the laminas-authentication component
using Composer in this chapter. Open the composer.json file in Eclipse, and through the
Dependencies tab, search for the laminas/laminas-session package and add it. Don't forget
to save the file and click on the Update dependencies buttons. Are you forgetting to save the file again,
right? Don't worry, we will repeat as many times as it is necessary.

After installing laminas-session, you can try logging in again by the web interface. The result
will be a window with the message Invalid data!, as seen in Figure 9.5:

![](bbox=[0.175, 0.573, 0.792, 0.743])

Figure 9.5 – A warning message after a login attempt

Of course, at this moment, whichever nickname and password you type are invalid data, because
there are no registered employees. For that, we need to create the employee registration pages. This
is what we will do in the next section.
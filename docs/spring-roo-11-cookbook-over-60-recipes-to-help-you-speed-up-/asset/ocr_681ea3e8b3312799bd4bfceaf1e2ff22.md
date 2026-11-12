In the given code, UserServiceFactory is a GAE-specific class whose getUserService method returns an instance of UserService. The UserService interface defines methods to create login and logout URLs, get details of the currently signed in user, check if the user is logged in, and so on. In this code, GaeAuthFilter checks if the user is logged in by calling the isUserLoggedIn() method. If the user is not logged in, GaeAuthFilter makes use of the createLoginURL(..) method of UserService to create a login URL and redirects the user to it.

Another interesting point to notice about GaeAuthFilter is its mapping. The following listing shows the mapping of GWT's RequestFactoryServlet and GaeAuthFilter in the web.xml file:

```
<filter-mapping>
  <filter-name>GaeAuthFilter</filter-name>
  <url-pattern>/gwtRequest/*</url-pattern>
</filter-mapping>

<servlet-mapping>
  <servlet-name>requestFactory</servlet-name>
  <url-pattern>/gwtRequest</url-pattern>
</servlet-mapping>
```

The <url-pattern> elements of GaeAuthFilter and GWT's RequestFactoryServlet show that a web request sent to RequestFactoryServlet is intercepted by GaeAuthFilter. This ensures that if the session expires, the application user is redirected to the Google Accounts sign-in page.

UserService provides a getCurrentUser method that returns a User object if the user is logged in. The User object contains user id, nickname, and e-mail information of the authenticated user. If your application requires capturing more information about the user, such as their preferences, address, and so on, then you need to save such information as part of your application data.

By default, the only role defined by App Engine is admin, which you can specify as the value of the <role-name> element. The admin role is assigned to users that are application administrators, that is, users that you add using the Admin Console of GAE. Admin Console gives you complete control over your deployed application on GAE. It allows you to administer your datastore, test different versions of your application, create application, and so on. You can access Admin Console by going to the following URL: http://appengine.google.com. You can use the isUserAdmin method of UserService to determine if the logged in user belongs to admin role or not.

Even though GAE supports only admin role, you can still incorporate role-based security in your App Engine applications by introducing application-specific roles. You can save application-specific role information as part of application data. For instance, you can use Spring Security framework with your GWT or Spring Web MVC application to implement web request security and method-level security based on the roles assigned to users.
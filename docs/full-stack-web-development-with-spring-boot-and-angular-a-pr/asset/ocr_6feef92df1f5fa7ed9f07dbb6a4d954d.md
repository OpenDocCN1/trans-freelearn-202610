HttpSecurity allows us to call methods that will implement configuration for web-based security requests for the HTTP requests. By default, the security configuration will be applied to all HTTP requests, but we can also set only specific requests by using the requestMatcher() methods. HttpSecurity is the same as the Spring Security XML configuration.

Let's discuss the standard methods under HttpSecurity:

+   - csrf(): Enables the CSRF protections. This is enabled by default.
+   - disable(): Disables the initial configurations; a new version can be applied after calling the method.
+   - antMatcher("/**"): By default, our configuration will be applied to all HTTP requests. We can use the antMatcher() method to specify the URL patterns where we want to apply the configuration.
+   - antMatchers("/**"): Similar to the antMatcher() method, but accepts a list of patterns where we want to apply the configuration.
+   - permitAll(): Specifies that access to any URL endpoints are allowed by anyone.
+   - anyRequest(): Applies to any type of HTTP request.
+   - authenticated(): Specifies that any URL endpoints are allowed by any authenticated user.
+   - exceptionHandling(): Exception handling configuration.
+   - sessionManagement(): This method is commonly used to manage how many allowed sessions for a user can be active; for example, we configure sessionManagement().maximumSessions(1).expiredUrl("/login?expired"), which indicates that when the user is logged in to another terminal and attempts to log in to another instance, it will automatically log them out of the other instance.
+   - sessionCreationPolicy(): Allows us to create a policy for when a session should get created; the possible values are ALWAYS, IF_REQUIRED, NEVER, and STATELESS.

In our code, let's configure a basic configuration for our security. Let's place the following code inside the configure(HttpSecurity http) method:

```
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        // first chain
        .csrf()
        .disable()
        // second chain
        .antMatcher("/**")
```
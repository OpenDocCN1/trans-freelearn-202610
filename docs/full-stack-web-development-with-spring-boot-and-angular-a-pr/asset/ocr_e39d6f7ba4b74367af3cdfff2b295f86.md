```
.authorizeRequests()
// third chain
.antMatchers("/**")
.permitAll()
// fourth chain
.and()
.sessionManagement()
.sessionCreationPolicy(
    SessionCreationPolicy.STATELESS);
}
```

In the preceding example configuration, we have implemented several configurations for our application. You can notice that we have divided the methods into chains. This is to show the methods are related to each other.

The first chain, with .csrf().disable(), disables the use of CSRF protection. This is just an example, and disabling CSRF is not recommended when building your application. The second chain, with .antMatcher("/**").authorizedRequests(), states that any requests are authorized to be accessed by any users regardless of the role.

This can be modified by specifying the role in the hasRole() method by restricting the users based on the assigned roles. The third chain is .antMatchers("/**").permitAll(), which indicates that any users can access all the URLs, and lastly, sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS) indicates that no session should be created by Spring Security.

We have successfully created SecurityConfig, which contains all of our configurations for Spring Security; our code should look like the following:

```
@AllArgsConstructor
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends
WebSecurityConfigurerAdapter {
    // removed some code for brevety

    @Override
    protected void configure(HttpSecurity http) throws
    Exception {
        http
            // first chain
```
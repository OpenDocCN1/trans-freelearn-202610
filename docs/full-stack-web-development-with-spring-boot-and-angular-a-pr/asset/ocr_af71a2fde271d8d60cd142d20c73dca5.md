The next step is we need to add the filter we have created; under the configure (HttpSecurity http) method, we will place the following code:

```
private final JwtRequestFilter jwtFilter;

@Override
protected void configure(HttpSecurity http) throws Exception {
    ... Http security configurations
    http.addFilterBefore(jwtFilter,
        UsernamePasswordAuthenticationFilter.class);
}
```

Now that our security configuration is complete, our final step is to add authentication to our anti-hero endpoints. A valid JWT is required upon making a request to the anti-hero endpoints.

To achieve this, we will annotate AntiHeroController with @PreAuthorize("isAuthenticated()") to configure the endpoints with the authentication process:

```
@PreAuthorize("isAuthenticated()")
public class AntiHeroController {
... methods
}
```

We have successfully implemented Spring Security and JWT on our application; let's simulate the endpoints created.

We will send an HTTP GET request for the anti-hero controller to get the list of all anti-heroes:

![](bbox=[0.22, 0.66, 0.8, 0.89])

Figure 7.2 – 403 Forbidden on getting the anti-heroes list
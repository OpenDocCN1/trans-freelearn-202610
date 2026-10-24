```
return new AuthenticationResponse(jwt);
}
}
```

The authenticate() method accepts an AuthenticationRequest body, which requires an email address and password. We will use service.authenticate() we previously created to check whether the credentials are valid. Once this is confirmed, we can generate the token using generateToken() from our utilities and return an AuthenticationResponse object.

# Creating the authentication filters

The last step we need to accomplish is to create the filter for our authentication. We will use filters to validate each HTTP request with a valid JWT in the request headers. We need to make sure that a filter is invoked only once for each request. We can achieve this by using OncePerRequestFilter. We will extend our filter class with the filter to ensure that the filter is only executed once for a specific request.

Now, let's create our authentication filter; first, let's create a class named JwtRequestFilter under the filters package, and we will extend this class with OncePerRequestFilter, then we will override the doFilterInternal() method, which has parameters of HttpServletRequest, HttpServletResponse, and FilterChain. We will also inject ApplicationUserDetailsService and JwtUtil for the credentials and token validation.

Our code will look like the following:

```
@AllArgsConstructor
@Component
public class JwtRequestFilter extends OncePerRequestFilter {

    private final ApplicationUserDetailsService
        userDetailsService;

    private final JwtUtil jwtUtil;

    @Override
    protected void doFilterInternal(
        HttpServletRequest request,
        HttpServletResponse response,
        FilterChain chain
    ) throws ServletException, IOException {
```
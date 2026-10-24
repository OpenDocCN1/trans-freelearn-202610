
```
}

@Override
public String getPassword() {
    return null;
}

@Override
public String getUsername() {
    return this.userEntity.getEmail();
}
// Code removed for brevity. Please refer using the
// GitHub repo.
@Override
public boolean isEnabled() {
    return false;
}
}
```

The use principal model implements UserDetails as this will be our custom user for Spring Security. We have overridden several methods, such as getAuthorities(), which retrieves the list of authorizations of the user, isAccountNonLocked(), which checks whether the user is locked, isAccountNonExpired(), which validates that the user is valid and not yet expired, and isEnabled(), which checks whether the user is active.

## Creating the authentication utilities

We need to create the utilities for our authentication; the utilities will be responsible for the JWT creation, validation and expiration checks, and extraction of the information. These are the methods we will use to validate our token.

We will create a class named JwtUtil under the util package, and we will annotate this with an @Service annotation. Let’s start with the methods needed for util.

Let’s create the first two methods that we need to create a valid token:

```
private String createToken(Map<String, Object> claims, String subject) {
    Keys.
    return Jwts
        .builder()
```

```
.setClaims(claims)
.setSubject(subject)
.setIssuedAt(new Date(System.currentTimeMillis()))
.setExpiration(new Date(System.currentTimeMillis() +
                1000 * 60 * 60 * 10))
    .signWith(SignatureAlgorithm.HS256, SECRET_KEY)
.compact();
}
public String generateToken(UserDetails userDetails) {
    Map<String, Object> claims = new HashMap<>();
    return createToken(claims, userDetails.getUsername());
}
```

The preceding implementation calls several methods from the JWT extension:

- The builder() method, which is responsible for the building of the JWT.
- The setClaims() method, which sets the claims of the JWT.
- The setSubject() method, which sets the subject; in this case, the value is the email address of the user.
- The setIssuedAt() method, which sets the date when the JWT is created.
- The setExpiration() method, which sets the expiration date of the JWT.
- The signWith() method, which signs the JWT with the provided key and algorithm.

The next method we need to implement is the claims extraction. We will use this method mainly to get useful information, such as the subject and the expiration of the token.

Let's have a look at the following code implementation:

```
public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
    final Claims claims = extractAllClaims(token);
    return claimsResolver.apply(claims);
}

private Claims extractAllClaims(String token) {
    return Jwts
        .parserBuilder()
        .setSigningKey(SECRET_KEY)
```

```
.build()
.parseClaimsJws(token)
.getBody();
}
```

The extractAllClaims() method receives the token and uses the secret key provided by the application. We have called the parseClaimsJWS() method to extract the claims from the JWT.

Now, we will create the methods to extract and check whether the token is expired and extract the username using the extractClaims() method we have created.

Let's have a look at the following code implementation:

```
public Date extractExpiration(String token) {
    return extractClaim(token, Claims::getExpiration);
}
private Boolean isTokenExpired(String token) {
    return extractExpiration(token).before(new Date());
}
public String extractUsername(String token) {
    return extractClaim(token, Claims::getSubject);
}
```

We have used the getExpiration and getSubject built-in functions to get the expiration date and subject from the claims.

Lastly, we will create a method to validate that the token is not yet expired or a valid user is using the token.

Let's have a look at the following code implementation:

```
public Boolean validateToken(String token,
                        UserDetails userDetails) {
    final String username = extractUsername(token);
    return (
        username.equals(userDetails.getUsername()) &&
        !isTokenExpired(token)
    );
}
```

## Creating the authentication service

Now, we will create the service for our authentication, as we know that services are responsible for the logic of our application. We will make the following methods, which will verify whether the password is correct using the hash, check whether the user has valid credentials, and provide a method that will override the default authentication.

The first step is to create a new class named ApplicationUserDetailsService under the service package, and we will implement the class using UserDetailsService from Spring Security. We will override the loadUserByUsername() method and execute the following code:

```
@Override
public UserDetails loadUserByUsername(String email)
    throws UsernameNotFoundException {
    return new UserPrincipal(
        userService.searchByEmail(email));
}
```

The preceding code calls the searchByEmail() method, which is our custom implementation for checking whether a user exists, and we will return the user as a UserPrincipal object.

The next step is to create the verifyPasswordHash() method, which will validate the user’s password.

Let’s have a look at the following code implementation:

```
private Boolean verifyPasswordHash(
    String password,
    byte[] storedHash,
    byte[] storedSalt
) throws NoSuchAlgorithmException {
    // Code removed for brevity. Please refer to the GitHub
    // repo

    for (int i = 0; i < computedHash.length; i++) {
        if (computedHash[i] != storedHash[i]) return false;
    }

    // The above for loop is the same as below

    return MessageDigest.isEqual(computedHash, storedHash);
}
```

The method we have created accepts the password, the stored salt, and the user’s hash. We will first check whether storedHash has a length of 64 and storedSalt has a size of 128 to validate whether it is 64 bytes. We will get the computed hash by using the stored salt and message digest for the password, and lastly, we will check whether the passwords match by seeing whether the calculated hash and stored hash are equal.

The last method we need to implement is the authenticate() method. This is the primary method that our authenticate endpoint will call.

Let’s have a look the following code implementation:

```
public UserEntity authenticate(String email, String password)
throws NoSuchAlgorithmException {
    if (
        email.isEmpty() || password.isEmpty()
    ) throw new BadCredentialsException("Unauthorized");

    var userEntity = userService.searchByEmail(email);

    if (userEntity == null) throw new
        BadCredentialsException("Unauthorized");

    var verified = verifyPasswordHash(
        password,
        userEntity.getStoredHash(),
        userEntity.getStoredSalt()
    );

    if (!verified) throw new
        BadCredentialsException("Unauthorized");

    return userEntity;
}
```

The method first checks whether the user exists using the searchByEmail() method and checks whether the password is valid using the verifyPasswordHash() method that we have created.

## Creating the authentication controller

Now, we will create the controllers of our authentication. This would create the primary endpoint for our login. The first step is to create a class named AuthenticateController under the controllers’ package, and next, we will make authenticate() with the following implementation:

```
@RestController
@AllArgsConstructor
class AuthenticateController {

    private final AuthenticationManager authenticationManager;
    private final JwtUtil jwtTokenUtil;
    private final ApplicationUserDetailsService userDetailsService;

    @RequestMapping(value = "/authenticate")
    @ResponseStatus(HttpStatus.CREATED)
    public AuthenticationResponse authenticate(
        @RequestBody AuthenticationRequest req

    ) throws Exception {
        UserEntity user;

        try {

            user = userDetailsService.authenticate(
                req.getEmail(), req.getPassword());

        } catch (BadCredentialsException e) {
            throw new Exception("Incorrect username or password", e);
        }
    }
```

Then, we get the details of the user by calling loadUserByUsername from userDetailsService but don’t forget to pass the email address of the user like so:

```
var userDetails = userDetailsService.loadUserByUsername(user.getEmail());

System.out.println(userDetails);
var jwt = jwtTokenUtil.generateToken(userDetails);
```


HttpSecurity allows us to call methods that will implement configuration for web-based security requests for the HTTP requests. By default, the security configuration will be applied to all HTTP requests, but we can also set only specific requests by using the requestMatcher() methods. HttpSecurity is the same as the Spring Security XML configuration.

Let's discuss the standard methods under HttpSecurity:

- `csrf()`: Enables the CSRF protections. This is enabled by default.
- `disable()`: Disables the initial configurations; a new version can be applied after calling the method.
- `antMatcher("/**")`: By default, our configuration will be applied to all HTTP requests. We can use the antMatcher() method to specify the URL patterns where we want to apply the configuration.
- `antMatchers("/**")`: Similar to the antMatcher() method, but accepts a list of patterns where we want to apply the configuration.
- `permitAll()`: Specifies that access to any URL endpoints are allowed by anyone.
- `anyRequest()`: Applies to any type of HTTP request.
- `authenticated()`: Specifies that any URL endpoints are allowed by any authenticated user.
- `exceptionHandling()`: Exception handling configuration.
- `sessionManagement()`: This method is commonly used to manage how many allowed sessions for a user can be active; for example, we configure `sessionManagement().maximumSessions(1).expiredUrl("/login?expired")`, which indicates that when the user is logged in to another terminal and attempts to log in to another instance, it will automatically log them out of the other instance.
- `sessionCreationPolicy()`: Allows us to create a policy for when a session should get created; the possible values are ALWAYS, IF_REQUIRED, NEVER, and STATELESS.

In our code, let's configure a basic configuration for our security. Let's place the following code inside the `configure(HttpSecurity http)` method:

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        // first chain
        .csrf()
        .disable()
        // second chain
        .antMatcher("/**")
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

The first chain, with `.csrf().disable()`, disables the use of CSRF protection. This is just an example, and disabling CSRF is not recommended when building your application. The second chain, with `.antMatcher("/**").authorizeRequests()`, states that any requests are authorized to be accessed by any users regardless of the role.

This can be modified by specifying the role in the `hasRole()` method by restricting the users based on the assigned roles. The third chain is `.antMatchers("/**").permitAll()`, which indicates that any users can access all the URLs, and lastly, `sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)` indicates that no session should be created by Spring Security.

We have successfully created SecurityConfig, which contains all of our configurations for Spring Security; our code should look like the following:

```java
@AllArgsConstructor
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends
        WebSecurityConfigurerAdapter {
    // removed some code for brevity

    @Override
    protected void configure(HttpSecurity http) throws
    Exception {
        http
            // first chain
            .csrf()
            .disable()
            // second chain
            .antMatcher("/**")
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
}
```

Now, we will move on to the next step, where we will create our endpoints for our user entity.

## Creating user endpoints

When implementing CRUD, we need to create our user endpoints. We need to develop these endpoints such that they will be used for the registration of a new user in our database. In this example, we will repeat the steps on how to develop endpoints discussed in the chapter titled “Documenting APIs with OpenAPI Specification,” but we will also create a whole CRUD capability for the user entity.

Let’s create a new user package and make the controller, data, entity, repository, and service packages under the user package.

### Creating the user entity

Let’s create the user entity first by creating a new class named UserEntity under the entity package, and we will place the following code:

```java
@Entity
@Data
@AllArgsConstructor
@NoArgsConstructor
public class UserEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO,
                    generator = "UUID")
    @Column(nullable = false, updatable = false)
    private UUID id;

    @Column(unique = true)
    private String email;

    private String mobileNumber;
    private byte[] storedHash;
    private byte[] storedSalt;

    public UserEntity(String email, String mobileNumber) {
        this.email = email;
        this.mobileNumber = mobileNumber;
    }
}
```

In the preceding example, we have assigned several properties for UserEntity. We have annotated it with `@Entity` to indicate that this is a Java Persistence API (JPA) entity. We have configured it with an email address, mobile number, storedHash, and a storedSalt property. storedHash and storedSalt will be used for hashing and verifying the user’s password.

### Creating the user DTO

After creating the user entity, we will make the Data Transfer Object (DTO). The DTO is an object that we commonly use for responses or to hide unnecessary properties. We will create a new class named UserDto under the data package, and we will place the following code:

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class UserDto {

    private UUID id;
    private String email;
    private String mobileNumber;
    private String password;
}
```

### Creating the user repository

The next thing we need to do is create the repository for our user. Under the repository package, create a new class named UserRepository, and we will extend the class with JpaRepository by adding the following code:

```java
@Repository
public interface UserRepository extends JpaRepository<UserEntity, UUID> {
    @Query(
        "" +
        "SELECT CASE WHEN COUNT(u) > 0 THEN " +
        "TRUE ELSE FALSE END " +
        "FROM UserEntity u " +
        "WHERE u.email = ?1"
    )
    Boolean selectExistsEmail(String email);

    UserEntity findByEmail(String email);
}
```

In the preceding example, we extended UserRepository with JpaRepository, which grants all the CRUD capabilities to our repository. We have also created two methods with an `@Query` annotation, which checks whether the email address already exists.

### Creating the user service

The next step is now to create our user service where we will implement the business logic of the application. Under the service package, we will create a new class named UserService.

After the creation of the service, we will place `@AllArgsConstructor` for the constructor injecting the dependencies and the `@Service` annotation to let Spring know that this is a service layer, and we will also inject `ModelMapper` and `UserRepository` into our service after the annotations and dependency injection.

We can create two methods that allow us to convert an entity into a DTO and vice versa by placing the following code:

```java
private UserDto convertToDto(UserEntity entity) {
    return mapper.map(entity, UserDto.class);
}

private UserEntity convertToEntity(UserDto dto) {
    return mapper.map(dto, UserEntity.class);
}
```

Now, we will create the code for the basic CRUD functionalities:

- **Getting all users**: To get all of the users, we will place the following code:

```java
public List<UserDto> findAllUsers() {
    var userEntityList =
        new ArrayList<>(repo.findAll());

    return userEntityList
        .stream()
        .map(this::convertToDto)
        .collect(Collectors.toList());
}
```

The example code returns all the list of users converted into a DTO.

- **Getting users by ID**: To get a specific user by ID, we will place the following code:

```java
public UserDto findUserById(final UUID id) {
    var user = repo
        .findById(id)
        .orElseThrow(
            () -> new NotFoundException("User by id " + id +
                " was not found")
        );

    return convertToDto(user);
}
```

This example method retrieves a specific user using the `findById()` method of the user repository.

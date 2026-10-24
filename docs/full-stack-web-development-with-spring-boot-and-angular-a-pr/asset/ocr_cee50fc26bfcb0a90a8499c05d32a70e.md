We also need to import the JWT module into our `app.module.ts` file:

```
```typescript
imports: [
    JwtModule.forRoot({})
],
```
```

We will use the `isAuthenticated()` function on our login and register pages to check whether a JWT is present in our local storage. If there is a valid JWT, we will redirect the application to the anti-hero list page.

Let’s have a look at the following implementation:

```
```typescript
//login page (TS File)
constructor(private authService: AuthenticateService, private
router: Router,) {
    this.checkJWT();
}
checkJWT() {
    if(this.authService.isAuthenticated()) {
        this.router.navigate(['/anti-heroes'])
    }
}
```
```

# Logout implementation

The last feature we need to implement is the logout function. To add this feature, the only function we need to add is a function that will remove the token from our storage. Let’s have the code implementation as follows:

# authenticate.service.ts

```
```typescript
export class AuthenticateService {
... other functions
doLogout() {
    let removeToken = localStorage.removeItem('token');
    if (removeToken == null) {
        this.router.navigate(['login']);
    }
}
}
```
```
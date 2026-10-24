We can also observe that our valid JWT is already placed in our local storage as the HTTP interceptor is using the JWT. As we open a request made by our application, we can see that the headers have the generated JWT:

```
Accept:
application/json, text/plain, */*
Accept-Encoding:
gzip, deflate, br
Accept-Language:
en-AU,en-US;q=0.9,en;q=0.8,bs;q=0.7,fr-CA;q=0.6,fr;q=0.5,tl;q=0.4
Authorization:
Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0ZXN0OQGdtYWlsLmNvbSIIm1hdCI6MTY1OTY5ODM2NSwiZXhwIjoxNjU5NzM0MzY1fQ.2SDLmvQcME5Be9Xj-zTeRlc6kGfQVNCMIWUBOBS5afg
```

In the preceding example headers, we can see that the Authorization header contains the valid JWT for every API request the application calls. The placement of the JWT in the header is done when our login is successful and we are redirected to the AntiHeromodule.

# Token validation

The next step we need to do is to add token validation to check whether our token already expired. To implement this feature, we add the @auth0/angular-jwt library by executing the following command:

```
npm install @auth0/angular-jwt --save
```

The @auth0/angular-jwt library provides useful functions, such as isTokenExpired(), which checks whether the JWT is expired, and decodeToken(), which retrieves the information from the JWT.

After successfully installing the library, we will add the following code to our authenticate service:

```
isAuthenticated(): boolean {
  const token = localStorage.getItem('token') ?? '';
  // Check whether the token is expired and return
  // true or false
  return !this.jwtHelper.isTokenExpired(token);
}
```